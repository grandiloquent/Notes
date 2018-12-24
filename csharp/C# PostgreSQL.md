# C# PostgreSQL

```csharp
namespace NpgSQLSample
{
	using Npgsql;
	using Npgsql.PostgresTypes;
	using Npgsql.Json.NET;
	using System;
	using System.Collections.Generic;
	using System.IO;
	using System.Threading.Tasks;
	using Newtonsoft.Json.Linq;
	using Newtonsoft.Json;
	class Order
	{
		[JsonProperty(PropertyName = "customer")]
		public string Customer;
		
		[JsonProperty(PropertyName = "items")]
		public List<Item> Items;
		
 
		public override string ToString()
		{
			return  JsonConvert.SerializeObject(this);
		}
	}
	class Item
	{
		[JsonProperty(PropertyName = "product")]
		public string Product;
		[JsonProperty(PropertyName = "qty")]
		public int Quantity;
	}
	static  class Extensions
	{
		static readonly object sLock = new object();
		
		public static void DumpReader(this NpgsqlConnection conn, string sql)
		{
			NpgsqlDataReader reader = conn.ExecuteReader(sql);
			var fieldCount = reader.FieldCount;
			
			while (reader.Read()) {
				for (int i = 0; i < fieldCount; i++) {
					
					try {
						string.Format("{0} > {1} > {2}", i, reader.GetName(i), reader.GetValue(i))
							.LogInfo();
					} catch (Exception e) {
						string.Format("{0} > {1} > {2}", i, reader.GetName(i), e.Message).LogError();
					}
				}
				
			}
			reader.Close();
		}
		public static NpgsqlDataReader ExecuteReader(this NpgsqlConnection conn, string sql)
		{
			using (var cmd = new NpgsqlCommand(sql, conn)) {
				return cmd.ExecuteReader();
			}
		}
		public static void ExecuteNonQuery(this NpgsqlConnection conn, string sql)
		{
			using (var cmd = new NpgsqlCommand(sql, conn)) {
				cmd.ExecuteNonQuery();
			}
		}
		
		public static void FindingCurrentSettings(this NpgsqlConnection conn)
		{
			const string sql = "SELECT * FROM pg_settings WHERE name = 'work_mem';";
			conn.DumpReader(sql);
		}
		public static void FindingNonDefaultSettigns(this NpgsqlConnection conn)
		{
			var stringBuilder = new System.Text.StringBuilder(215);
			stringBuilder.AppendLine(@"SELECT name, source, setting ");
			stringBuilder.AppendLine(@"                        FROM pg_settings  ");
			stringBuilder.AppendLine(@"                        WHERE source != 'default' ");
			stringBuilder.AppendLine(@"                        AND source != 'override' ");
			stringBuilder.AppendLine(@"                        ORDER by 2, 1");
			
			conn.DumpReader(stringBuilder.ToString());
		}
		public static void JsonSetup(this NpgsqlConnection conn)
		{
			// https://github.com/npgsql/npgsql/blob/e05caf4075d904347d4db4335717fb732cb08630/doc/types/jsonnet.md
			
			// Place this at the beginning of your program to use Json.NET everywhere (recommended)
			NpgsqlConnection.GlobalTypeMapper.UseJsonNet();

			// Or to temporarily use JsonNet on a single connection only:
			// conn.TypeMapper.UseJsonNet();
		}
		public static void JsonCreateTable(this NpgsqlConnection conn)
		{
			var stringBuilder = new System.Text.StringBuilder(80);
			stringBuilder.AppendLine(@"CREATE TABLE IF NOT EXISTS orders (");
			stringBuilder.AppendLine(@" ID serial NOT NULL PRIMARY KEY,");
			stringBuilder.AppendLine(@" info json NOT NULL");
			stringBuilder.AppendLine(@");");

			conn.ExecuteNonQuery(stringBuilder.ToString());
		}
		public static void JsonInsertDatas(this NpgsqlConnection conn)
		{
//			var stringBuilder = new System.Text.StringBuilder(285);
//			stringBuilder.AppendLine(@"INSERT INTO orders (info)");
//			stringBuilder.AppendLine(@"VALUES");
//			stringBuilder.AppendLine(@" (");
//			stringBuilder.AppendLine(@" '{ ""customer"": ""Lily Bush"", ""items"": {""product"": ""Diaper"",""qty"": 24}}'");
//			stringBuilder.AppendLine(@" ),");
//			stringBuilder.AppendLine(@" (");
//			stringBuilder.AppendLine(@" '{ ""customer"": ""Josh William"", ""items"": {""product"": ""Toy Car"",""qty"": 1}}'");
//			stringBuilder.AppendLine(@" ),");
//			stringBuilder.AppendLine(@" (");
//			stringBuilder.AppendLine(@" '{ ""customer"": ""Mary Clark"", ""items"": {""product"": ""Toy Train"",""qty"": 2}}'");
//			stringBuilder.AppendLine(@" );");
	
			conn.TypeMapper.UseJsonNet();
			var items1 = new List<Item>();
			items1.Add(new Item() {
				Product = "Diaper",
				Quantity = 24
			});
			 
		
//			values.Add(JObject.Parse("{ \"customer\": \"Josh William\", \"items\": {\"product\": \"Toy Car\",\"qty\": 1}}"));
//			values.Add(JObject.Parse("{ \"customer\": \"Mary Clark\", \"items\": {\"product\": \"Toy Train\",\"qty\": 2}}"));
//			
			using (var cmd = new NpgsqlCommand("INSERT INTO orders (info) VALUES (@p)", conn)) {
				cmd.Parameters.Add(new NpgsqlParameter("p", NpgsqlTypes.NpgsqlDbType.Jsonb) { Value = new[] {new Order() {
							Customer = "Lily Bush",
							Items = items1
						}
					}
				});
				cmd.ExecuteNonQuery();
			}

		}
		public static void JsonQueryData(this NpgsqlConnection conn)
		{
			const string sql = "SELECT info FROM orders;";
				 
			conn.DumpReader(sql);
			
			var stringBuilder = new System.Text.StringBuilder(55);
			stringBuilder.AppendLine(@"SELECT");
			stringBuilder.AppendLine(@" info -> 'customer' AS customer");
			stringBuilder.AppendLine(@"FROM");
			stringBuilder.AppendLine(@" orders;");
			
			conn.DumpReader(stringBuilder.ToString());

			stringBuilder.Clear();
			stringBuilder.AppendLine(@"SELECT");
			stringBuilder.AppendLine(@" info ->> 'customer' AS customer");
			stringBuilder.AppendLine(@"FROM");
			stringBuilder.AppendLine(@" orders;");
			conn.DumpReader(stringBuilder.ToString());

			stringBuilder.Clear();
			stringBuilder.AppendLine(@"SELECT");
			stringBuilder.AppendLine(@" info -> 'items' ->> 'product' as product");
			stringBuilder.AppendLine(@"FROM");
			stringBuilder.AppendLine(@" orders");
			stringBuilder.AppendLine(@"ORDER BY");
			stringBuilder.AppendLine(@" product;");
			conn.DumpReader(stringBuilder.ToString());
				
			"Use JSON operator in WHERE clause".LogInfo();
			stringBuilder.Clear();
			stringBuilder.AppendLine(@"SELECT");
			stringBuilder.AppendLine(@" info ->> 'customer' AS customer");
			stringBuilder.AppendLine(@"FROM");
			stringBuilder.AppendLine(@" orders");
			stringBuilder.AppendLine(@"WHERE");
			stringBuilder.AppendLine(@" info -> 'items' ->> 'product' = 'Diaper'");
			conn.DumpReader(stringBuilder.ToString());
			
			"Apply aggregate functions to JSON data".LogInfo();
			stringBuilder.Clear();
			stringBuilder.AppendLine(@"SELECT");
			stringBuilder.AppendLine(@" MIN (");
			stringBuilder.AppendLine(@" CAST (");
			stringBuilder.AppendLine(@" info -> 'items' ->> 'qty' AS INTEGER");
			stringBuilder.AppendLine(@" )");
			stringBuilder.AppendLine(@" ),");
			stringBuilder.AppendLine(@" MAX (");
			stringBuilder.AppendLine(@" CAST (");
			stringBuilder.AppendLine(@" info -> 'items' ->> 'qty' AS INTEGER");
			stringBuilder.AppendLine(@" )");
			stringBuilder.AppendLine(@" ),");
			stringBuilder.AppendLine(@" SUM (");
			stringBuilder.AppendLine(@" CAST (");
			stringBuilder.AppendLine(@" info -> 'items' ->> 'qty' AS INTEGER");
			stringBuilder.AppendLine(@" )");
			stringBuilder.AppendLine(@" ),");
			stringBuilder.AppendLine(@" AVG (");
			stringBuilder.AppendLine(@" CAST (");
			stringBuilder.AppendLine(@" info -> 'items' ->> 'qty' AS INTEGER");
			stringBuilder.AppendLine(@" )");
			stringBuilder.AppendLine(@" )");
			stringBuilder.AppendLine(@" ");
			stringBuilder.AppendLine(@"FROM");
			stringBuilder.AppendLine(@" orders");

			conn.DumpReader(stringBuilder.ToString());
			

		}
		public static void LogError(this string message)
		{
			lock (sLock) {
				Console.ForegroundColor = ConsoleColor.Red;
				Console.WriteLine("[Error]: " + message);
				Console.ForegroundColor = ConsoleColor.White;
			}
		}
		public static void LogInfo(this string message)
		{
			lock (sLock) {
				Console.ForegroundColor = ConsoleColor.Cyan;
				Console.WriteLine("[Info]: " + message);
				Console.ForegroundColor = ConsoleColor.White;
			}
		}
	}
	class Program
	{
		private static NpgsqlConnection ConnectDatabase()
		{
			const string DefaultConnectionString = "Server=localhost;User ID=postgres;Password=Postgre;Database=postgres;Timeout=0;Command Timeout=0";
			
			var conn = new NpgsqlConnection(DefaultConnectionString);
			try {
				conn.Open();
			} catch (PostgresException e) {
				
				Console.WriteLine(e.Message);
				
				throw;
			}
			return conn;
		}
		
	
		private async static void CreateDatabase(NpgsqlConnection conn)
		{
			var list = await ListDatabase(conn);
			const string CreateStatement = "CREATE DATABASE npgsql_test;";
			if (!list.Contains("npgsql_test")) {
				using (var createCommand = new NpgsqlCommand(CreateStatement, conn)) {
					createCommand.ExecuteNonQuery();
				}
			} else {
				Console.WriteLine("The creation action was skiped.because the specified database already exists.");
			}
		}
		private async static Task<List<string>> ListDatabase(NpgsqlConnection conn)
		{
			var list = new List<string>();
			using (var command = new NpgsqlCommand("select datname from pg_database", conn)) {
				var reader = await command.ExecuteReaderAsync();
				while (await reader.ReadAsync()) {
					list.Add(reader.GetString(0));
				}
				// A command is already in progress:
				reader.Close();
			}
			Console.WriteLine("Finished: [ListDatabase]");
			return list;
		}
		
	
		public static void Main(string[] args)
		{
			// http://www.postgresqltutorial.com/
			// https://github.com/JamesNK/Newtonsoft.Json
			// https://github.com/npgsql/npgsql
			
			var conn = ConnectDatabase();
			//conn.JsonSetup();
			conn.JsonCreateTable();
			// conn.JsonInsertDatas();
			conn.JsonQueryData();
			// conn.FindingCurrentSettings();
			// conn.FindingNonDefaultSettigns();
			//CreateDatabase(conn);
			Console.ReadKey();
		}
	}
}
```

