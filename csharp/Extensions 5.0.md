# C#: Extensions 5.0

## FileExtensions


```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using System.Diagnostics.Contracts;
using System.Runtime.Versioning;
using System.Threading;
using System.Collections;
namespace Utils
{
	abstract internal class Iterator<TSource> : IEnumerable<TSource>, IEnumerator<TSource>
	{
		int threadId;
		internal int state;
		internal TSource current;

		public Iterator()
		{
			threadId = Thread.CurrentThread.ManagedThreadId;
		}

		public TSource Current {
			get { return current; }
		}

		protected abstract Iterator<TSource> Clone();

		public void Dispose()
		{
			Dispose(true);
			GC.SuppressFinalize(this);
		}

		protected virtual void Dispose(bool disposing)
		{
			current = default(TSource);
			state = -1;
		}

		public IEnumerator<TSource> GetEnumerator()
		{
			if (threadId == Thread.CurrentThread.ManagedThreadId && state == 0) {
				state = 1;
				return this;
			}

			Iterator<TSource> duplicate = Clone();
			duplicate.state = 1;
			return duplicate;
		}

		public abstract bool MoveNext();

		object IEnumerator.Current {
			get { return Current; }
		}

		IEnumerator IEnumerable.GetEnumerator()
		{
			return GetEnumerator();
		}

		void IEnumerator.Reset()
		{
			throw new NotSupportedException();
		}

	}
	internal class ReadLinesIterator : Iterator<string>
	{
		private readonly string _path;
		private readonly Encoding _encoding;
		private StreamReader _reader;

		[ResourceExposure(ResourceScope.Machine)]
		[ResourceConsumption(ResourceScope.Machine)]
		private ReadLinesIterator(string path, Encoding encoding, StreamReader reader)
		{
			Contract.Requires(path != null);
			Contract.Requires(path.Length > 0);
			Contract.Requires(encoding != null);
			Contract.Requires(reader != null);

			_path = path;
			_encoding = encoding;
			_reader = reader;
		}

		public override bool MoveNext()
		{
			if (this._reader != null) {
				this.current = _reader.ReadLine();
				if (this.current != null)
					return true;

				// To maintain 4.0 behavior we Dispose
				// after reading to the end of the reader.
				Dispose();
			}

			return false;
		}

		protected override Iterator<string> Clone()
		{
			// NOTE: To maintain the same behavior with the previous yield-based
			// iterator in 4.0, we have all the IEnumerator<T> instances share the same
			// underlying reader. If we have already been disposed, _reader will be null,
			// which will cause CreateIterator to simply new up a new instance to start up
			// a new iteration. Dev10 Bugs 904764 has been filed to fix this in next side-
			// by-side release.
			return CreateIterator(_path, _encoding, _reader);
		}

		protected override void Dispose(bool disposing)
		{
			try {
				if (disposing) {
					if (_reader != null) {
						_reader.Dispose();
					}
				}
			} finally {
				_reader = null;
				base.Dispose(disposing);
			}
		}

		internal static ReadLinesIterator CreateIterator(string path, Encoding encoding)
		{
			return CreateIterator(path, encoding, (StreamReader)null);
		}

		private static ReadLinesIterator CreateIterator(string path, Encoding encoding, StreamReader reader)
		{
			return new ReadLinesIterator(path, encoding, reader ?? new StreamReader(path, encoding));
		}
	}
	public static class FileExtensions
	{
		public static readonly char AltDirectorySeparatorChar = '/';
		public static readonly char VolumeSeparatorChar = ':';
		public static readonly char DirectorySeparatorChar = '\\';

		internal static readonly char[] InvalidPathChars = {
			'\"', '<', '>', '|', '\0',
			(char)1, (char)2, (char)3, (char)4, (char)5, (char)6, (char)7, (char)8, (char)9, (char)10,
			(char)11, (char)12, (char)13, (char)14, (char)15, (char)16, (char)17, (char)18, (char)19, (char)20,
			(char)21, (char)22, (char)23, (char)24, (char)25, (char)26, (char)27, (char)28, (char)29, (char)30,
			(char)31
		};
		internal static bool AnyPathHasWildCardCharacters(string path, int startIndex = 0)
		{
			char currentChar;
			for (int i = startIndex; i < path.Length; i++) {
				currentChar = path[i];
				if (currentChar == '*' || currentChar == '?')
					return true;
			}
			return false;
		}
		public static void CreateDirectoryIfNotExists(this String path)
		{
			if (Directory.Exists(path))
				return;
			Directory.CreateDirectory(path);
		}
		public static String GetExtension(this String path) {
            if (path==null)
                return null;

            int length = path.Length;
            for (int i = length; --i >= 0;) {
                char ch = path[i];
                if (ch == '.')
                {
                    if (i != length - 1)
                        return path.Substring(i, length - i);
                    else
                        return String.Empty;
                }
                if (ch == DirectorySeparatorChar || ch == AltDirectorySeparatorChar || ch == VolumeSeparatorChar)
                    break;
            }
            return String.Empty;
        }
		public static string GetValidFileName(this string value, char c=' ')
		{
			var chars = Path.GetInvalidFileNameChars();
			return new string(value.Select<char, char>((i) => {
				if (chars.Contains(i))
					return c;
				return i;
			}).Take(125).ToArray());
		}
		public static String GetFileName(this String path)
		{
			if (path != null) {

				int length = path.Length;
				for (int i = length; --i >= 0;) {
					char ch = path[i];
					if (ch == DirectorySeparatorChar || ch == AltDirectorySeparatorChar || ch == VolumeSeparatorChar)
						return path.Substring(i + 1, length - i - 1);

				}
			}
			return path;
		}
		public static IEnumerable<string> GetFiles(this string dir, string pattern, bool bExclude = false)
		{
			if (bExclude)
				return Directory.GetFiles(dir).Where(i => !Regex.IsMatch(i, "\\.(?:" + pattern + ")$"));
			else
				return Directory.GetFiles(dir).Where(i => Regex.IsMatch(i, "\\.(?:" + pattern + ")$"));

		}
		public static bool FileExists(this String path)
		{
			return  File.Exists(path);
		}
		public static String ReadAllText(this String path)
		{
			using (StreamReader sr = new StreamReader(path, new UTF8Encoding(false), true, 1024))
				return sr.ReadToEnd();
		}
		public static String GetCommandPath(this string fileName)
		{
			var dir = System.Reflection.Assembly.GetEntryAssembly().Location.GetDirectoryName();
			return dir.Combine(fileName);
		}
		public static String GetFileNameWithoutExtension(this String path)
		{
			path = GetFileName(path);
			if (path != null) {
				int i;
				if ((i = path.LastIndexOf('.')) == -1)
					return path; // No path extension found
                else
					return path.Substring(0, i);
			}
			return null;
		}

		public static bool AnyPathHasIllegalCharacters(this string path, bool checkAdditional = false)
		{
			return path.IndexOfAny(InvalidPathChars) >= 0 || (checkAdditional && AnyPathHasWildCardCharacters(path));
		}
		public static String Combine(this String path1, String path2)
		{
			return Path.Combine(path1, path2);
		}
		public static void WriteAllLines(this string path, IEnumerable<string> contents)
		{
			using (var writer = new StreamWriter(path, false, new UTF8Encoding(false))) {
				foreach (var line in contents) {
					writer.WriteLine(line);
				}
			}
		}
		public static string GetDirectoryName(this string path)
		{
			return Path.GetDirectoryName(path);
		}

		public static IEnumerable<String> ReadLines(this String path)
		{


			return ReadLinesIterator.CreateIterator(path, Encoding.UTF8);
		}

		public static void WriteAllText(this String path, String contents)
		{
			using (var sw = new StreamWriter(path, false, new UTF8Encoding(false)))
				sw.Write(contents);
		}
		public static string GetApplicationPath(this string path)
		{
			return Path.Combine(Path.GetDirectoryName(System.Reflection.Assembly.GetEntryAssembly().Location), path);
		}
		public static string GetDesktopPath(this string f)
		{
			return Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.Desktop), f);
		}
		public static string GetFileSha1(this string path)
		{
			using (var fs = new FileStream(path, FileMode.Open))
			using (var bs = new BufferedStream(fs))
			using (var reader = new StreamReader(bs)) {
				using (var sha1 = new System.Security.Cryptography.SHA1Managed()) {
					var hash = sha1.ComputeHash(bs);
					var formatted = new StringBuilder(2 * hash.Length);
					foreach (var b in hash) {
						formatted.AppendFormat("{0:X2}", b);
					}
				}
				return reader.ReadToEnd();
			}
		}
		public static string GetUniqueFileName(this string v)
		{
			var i = 1;
			var regex = new Regex(" \\- [0-9]+");
			var t = Path.Combine(Path.GetDirectoryName(v),
				        regex.Split(Path.GetFileNameWithoutExtension(v), 2).First() + " - " + i.ToString().PadLeft(3, '0') +
				        Path.GetExtension(v));
			while (File.Exists(t)) {
				i++;
				t = Path.Combine(Path.GetDirectoryName(v),
					regex.Split(Path.GetFileNameWithoutExtension(v), 2).First() + " - " + i.ToString().PadLeft(3, '0') +
					Path.GetExtension(v));
			}
			return t;
		}


		public static string[] ReadAllLines(this String path)
		{
			string line;
			var lines = new List<string>();
			using (var sr = new StreamReader(path, new UTF8Encoding(false)))
				while ((line = sr.ReadLine()) != null)
					lines.Add(line);
			return lines.ToArray();
		}

	}
}
```

## GenericExtensions


```csharp

using System;
using System.Collections.Generic;
using System.Linq;
namespace Utils
{

	public static class GenericExtensions
	{
		public static void ForEach<T>(IEnumerable<T> collections, Action<T> action)
		{
			var enumerable = collections as T[] ?? collections.ToArray();
			if (enumerable.Any()) {
				var size = enumerable.Count();
				for (var i = 0; i < size; i++) {
					action(enumerable.ElementAt(i));
				}
			}
		}
	}
}
```

## HtmlAgilityPackExtensions


```csharp

using System;
using HtmlAgilityPack;
namespace Utils
{

	public static class HtmlAgilityPackExtensions
	{
		public static string  DeEntitize(this string value)
		{
			return HtmlAgilityPack.HtmlEntity.DeEntitize(value);
		}
		public static HtmlAgilityPack.HtmlNode GetHtmlNode(this string value)
		{
			var hd = new HtmlAgilityPack.HtmlDocument();
			hd.LoadHtml(value);
			return hd.DocumentNode;
		}
	}
}
```

## StringExtensions


```csharp

using System;
using System.Linq;
using System.Collections.Generic;
using System.Text;
using System.Text.RegularExpressions;
using System.IO;
namespace Utils
{

	public static class StringExtensions

	{
		private const string Quote = "\"";

		public static string StringbuilderizeInCs(this string txt, string sbName = "stringBuilder")
		{
			// https://github.com/martinjw/SmartPaster2013

			//sb to work with
			var sb = new StringBuilder(txt);
			//escape \,", and {}
			sb.Replace(Quote, Quote + Quote);
			//process the passed string (txt), one line at a time
			//dump the stringbuilder into a temp string
			string fullString = sb.ToString();
			sb.Clear(); //lovely .net 4 - sb.Remove(0, sb.Length);
			//the original was horrible
			using (var reader = new StringReader(fullString)) {
				string line;
				while ((line = reader.ReadLine()) != null) {
					sb.Append(sbName + ".AppendLine(");
					sb.Append("@" + Quote);
					sb.Append(line);
					sb.AppendLine(Quote + ");");
				}
			}
			//TODO: Better '@"" + ' replacement to not cover inside strings
			sb.Replace("@" + Quote + Quote + " + ", "");
			//add the dec statement
			sb.Insert(0, "var " + sbName + " = new System.Text.StringBuilder(" + txt.Length + ");" + Environment.NewLine);
			//and return
			return sb.ToString();
		}
		public static string SubstringBefore(this string value, char delimiter)
		{
			var index = value.IndexOf(delimiter);
			if (index == -1)
				return value;
			else
				return value.Substring(0, index);
		}
		public static string SubstringBefore(this string value, string delimiter)
		{
			var index = value.IndexOf(delimiter);
			if (index == -1)
				return value;
			else
				return value.Substring(0, index);
		}
		public static string SubstringBeforeLast(this string value, char delimiter)
		{
			var index = value.LastIndexOf(delimiter);
			if (index == -1)
				return value;
			else
				return value.Substring(0, index);
		}
		public static string SubstringBeforeLast(this string value, string delimiter)
		{
			var index = value.LastIndexOf(delimiter);
			if (index == -1)
				return value;
			else
				return value.Substring(0, index);
		}
		public static IEnumerable<string> ToBlocks(this string value)
		{
			var count = 0;
			var sb = new StringBuilder();
			var ls = new List<string>();
			for (var i = 0; i < value.Length; i++) {
				sb.Append(value[i]);
				if (value[i] == '{') {
					count++;
				} else if (value[i] == '}') {
					count--;
					if (count == 0) {
						ls.Add(sb.ToString());
						sb.Clear();
					}
				}
			}
			return ls;
		}
		public static string ToLine(this IEnumerable<string> value, string separator = "\r\n")
		{
			return string.Join(separator, value);
		}
		public static string[] ToLines(this string value)
		{
			return value.Split(new[] { '\r', '\n' }, StringSplitOptions.RemoveEmptyEntries);
		}
		public		  static char ToLowerAsciiInvariant(this char c)
		{
			if ('A' <= c && c <= 'Z') {
				c = (char)(c | 0x20);
			}
			return c;
		}

		public static string StripHtmlTag(this string value)
		{
			return Regex.Replace(value, "<[^>]*?>", "");
		}

		public   static  char ToUpperAsciiInvariant(this char c)
		{
			if ('a' <= c && c <= 'z') {
				c = (char)(c & ~0x20);
			}
			return c;
		}
		public static string TrimComments(this string code)
		{
			const string re = @"(@(?:""[^""]*"")+|""(?:[^""\n\\]+|\\.)*""|'(?:[^'\n\\]+|\\.)*')|//.*|/\*(?s:.*?)\*/";
			return Regex.Replace(code, re, "$1");
		}

		public 	 static   bool IsAscii(this char c)
		{
			return c < 0x80;
		}

		public static string LiterallyInCs(this string txt)
		{
			//escape appropriately
			//escape the quotes with ""
			txt = txt.Trim() //ignore leading and trailing blank lines
                .Replace("\\", "\\\\") //escape backslashes
                .Replace(Quote, "\\\"") //escape quotes
                .Replace("\t", "\\t") //escape tabs
                .Replace("\r", "\\r") //cr
                .Replace("\n", "\\n") //lf
			//.Replace("\"\" + ", "") //"" +
				.Replace("\\r\\n", "\" + Environment.NewLine + \r\n\""); //escaped crlf to Env.NewLine;
			return Quote + txt + Quote;
		}
		public static bool IsReadable(this string value)
		{
			return !string.IsNullOrWhiteSpace(value);
		}

		public static bool IsVacuum(this string value)
		{
			return string.IsNullOrWhiteSpace(value);
		}
		public static string Capitalize(this string value)
		{
			//  && char.IsLower(value[0])
			if (!string.IsNullOrEmpty(value)) {
				return value.Substring(0, 1).ToUpper() + value.Substring(1).ToLower();
			}
			return value;
		}
		public static string GetFirstReadable(this string value)
		{
			return  value.TrimStart().Split(new char[] { '\n' }, 2).First().Trim();
		}
		public static string TrimNonLetterOrDigitStart(this string value)
		{
			var len = value.Length;
			var pos = 0;
//			int a='`';
//			int a1='a';
//			int a2='z';
//			int a3='A';
//			int a4='Z';
//			int a5='0';
//			int a6='9';

			for (int i = 0; i < len; i++) {
				if (('a' <= value[i] && value[i] <= 'z') ||
				    ('A' <= value[i] && value[i] <= 'Z') ||
				    ('0' <= value[i] && value[i] <= '9'))
					break;
				pos = i;
			}
			if (pos > 0)
				return value.Substring(pos + 1);
			return value;
		}

		public static string SubstringAfter(this string value, char delimiter)
		{
			var index = value.IndexOf(delimiter);
			if (index == -1)
				return value;
			else
				return value.Substring(index + 1);
		}
		public static string SubstringAfter(this string value, string delimiter)
		{
			var index = value.IndexOf(delimiter);
			return index == -1 ? value : value.Substring(index + delimiter.Length);
		}
		public static string SubstringAfterLast(this string value, char delimiter)
		{
			var index = value.LastIndexOf(delimiter);
			return index == -1 ? value : value.Substring(index + 1);
		}
		public static string SubstringAfterLast(this string value, string delimiter)
		{
			var index = value.LastIndexOf(delimiter);
			return index == -1 ? value : value.Substring(index + 1);
		}
	}
}
```

## TextBoxExtensions


```csharp

using System;
using System.Windows.Forms;
using System.Text;
namespace  Utils
{

	public static class TextBoxExtensions
	{
		    public static void Format(this TextBox textBox)
        {
            var sb = new StringBuilder();
            for (int i = 0; i < textBox.Lines.Length; i++)
            {
                if (textBox.Lines[i].IsVacuum())
                {
                    while (i + 1 < textBox.Lines.Length && textBox.Lines[i + 1].IsVacuum())
                    {
                        i++;
                    }
                    sb.AppendLine();
                }
                else
                {
                    sb.AppendLine(textBox.Lines[i]);
                }
            }
            textBox.Text = sb.ToString();
        }
		 public static void SelectLine(this TextBox textBox, bool trimEnd = false)
        {
            var start = textBox.SelectionStart;
            var length = textBox.Text.Length;
            var end = textBox.SelectionStart;
            var value = textBox.Text;
            while (start - 1 > -1 && value[start - 1] != '\n')
            {
                start--;
            }
            while (end + 1 < length && value[end + 1] != '\n')
            {
                end++;
            }
            if (trimEnd)
            {
                while (char.IsWhiteSpace(value[start]))
                {
                    start++;
                }
                while (char.IsWhiteSpace(value[end]))
                {
                    end--;
                }
            }
            textBox.SelectionStart = start;
            if (end > start)
                textBox.SelectionLength = end - start + 1;
        }
	}
}
```


