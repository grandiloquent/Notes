# C# Utilities


```csharp
	public static class Utilities
	{
		public static String ReadAllText(this String path)
		{
			var encoding = new UTF8Encoding(false);
			
			using (StreamReader sr = new StreamReader(path, encoding, true, 1024))
				return sr.ReadToEnd();
		}
	}
```

