# C#: Extensions 7.0

## CodeExtensions


```csharp

namespace Utils
{
    using System;
    using System.Text;
    public static class CodeExtensions
    {
        public static string GenerateFSharpList(this string value)
        {
            ReadOnlySpan<char> span = value.AsSpan();
            int offset = 0;
            var sb = new StringBuilder();
            for (int i = 0; i < span.Length; i++)
            {
                if (span[i] == ':')
                {
                    if (i == offset)
                    {
                        continue;
                    }
                    var line = span.Slice(offset, i - offset);
                    sb.Append($"\"{new string(line.Trim().ToArray())}\",");
                    offset = i+1;
                }
                else
                if (span[i] == '\r' || span[i] == '\n')
                {
                    if (i - offset == 1)
                    {
                        continue;
                    }
                    var line = span.Slice(offset, i - offset);

                    sb.Append($"\"{new string(line.Trim(new char[] { '\"', ' ' }).ToArray())}\"\n");
                    offset = i;
                }
            }
            return sb.ToString();
        }
    }
}
```

## FileExtensions


```csharp

namespace Utils
{
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.CompilerServices;
    using System.Text;
    using System.Text.RegularExpressions;
    public static class FileExtensions
    {
        internal const char AltDirectorySeparatorChar = '/';
        internal const char DirectorySeparatorChar = '\\';
        private static readonly char[] InvalidFileNameChars = { '\"', '<', '>', '|', '\0', ':', '*', '?', '\\', '/' };
        public static string ChangeExtension(this string path, string extension)
        {
            if (path != null)
            {
                string s = path;
                for (int i = path.Length - 1; i >= 0; i--)
                {
                    char ch = path[i];
                    if (ch == '.')
                    {
                        s = path.Substring(0, i);
                        break;
                    }
                    if (IsDirectorySeparator(ch)) break;
                }
                if (extension != null && path.Length != 0)
                {
                    s = (extension.Length == 0 || extension[0] != '.') ?
                        s + "." + extension :
                        s + extension;
                }
                return s;
            }
            return null;
        }
        public static string Combine(this string path, string fileName)
        {
            return Path.Combine(path, fileName);
        }
        public static void CreateDirectoryIfNotExists(this string path)
        {
            if (Directory.Exists(path))
                return;
            Directory.CreateDirectory(path);
        }
        public static bool FileExists(this string path)
        {
            return File.Exists(path);
        }
        public static string GetDesktopPath(this string f)
        {
            return Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.Desktop), f);
        }
        public static string GetDirectoryFileName(this string v)
        {
            return Path.GetFileName(Path.GetDirectoryName(v));
        }
        public static string GetDirectoryName(this string path)
        {
            if (path == null || path.AsSpan().IsEffectivelyEmpty())
                return null;
            int end = GetDirectoryNameOffset(path.AsSpan());
            return end >= 0 ? NormalizeDirectorySeparators(path.Substring(0, end)) : null;
        }
        private static int GetDirectoryNameOffset(ReadOnlySpan<char> path)
        {
            // int rootLength = path.Length;end > rootLength &&
            int end = path.Length;
            //if (end <= rootLength)
            //    return -1;
            while (!IsDirectorySeparator(path[--end])) ;
            // Trim off any remaining separators (to deal with C:\foo\\bar)
            while (IsDirectorySeparator(path[end - 1]))
                end--;
            return end;
        }
        public static string GetExePath(this string f)
        {
            return Path.Combine(Path.GetDirectoryName(System.Reflection.Assembly.GetEntryAssembly().Location), f);
        }
        public static ReadOnlySpan<char> GetFileName(this ReadOnlySpan<char> path)
        {
            // We don't want to cut off "C:\file.txt:stream" (i.e. should be "file.txt:stream")
            // but we *do* want "C:Foo" => "Foo". This necessitates checking for the root.
            for (int i = path.Length; --i >= 0;)
            {
                if (IsDirectorySeparator(path[i]))
                    return path.Slice(i + 1, path.Length - i - 1);
            }
            return path;
        }
        public static ReadOnlySpan<char> GetFileNameWithoutExtension(this ReadOnlySpan<char> path)
        {
            ReadOnlySpan<char> fileName = GetFileName(path);
            int lastPeriod = fileName.LastIndexOf('.');
            return lastPeriod == -1 ?
                fileName : // No extension was found
                fileName.Slice(0, lastPeriod);
        }
        public static string GetUniqueFileName(this string v)
        {
            int i = 1;
            Regex regex = new Regex(" \\- [0-9]+");
            string t = Path.Combine(Path.GetDirectoryName(v),
                           regex.Split(Path.GetFileNameWithoutExtension(v), 2).First() + " - " + i.ToString().PadLeft(3, '0') +
                           Path.GetExtension(v));
            while (File.Exists(t))
            {
                i++;
                t = Path.Combine(Path.GetDirectoryName(v),
                    regex.Split(Path.GetFileNameWithoutExtension(v), 2).First() + " - " + i.ToString().PadLeft(3, '0') +
                    Path.GetExtension(v));
            }
            return t;
        }
        public static string GetValidFileName(this string v)
        {
            if (v == null) return null;
            // (Char -> Int) 1-31 Invalid;
            List<char> chars = new List<char>(v.Length);
            for (int i = 0; i < v.Length; i++)
            {
                if (InvalidFileNameChars.Contains(v[i]))
                {
                    chars.Add(' ');
                }
                else
                {
                    chars.Add(v[i]);
                }
            }
            return new string(chars.ToArray());
        }
        public static string GetValidFileName(this string value, char c)
        {
            var chars = Path.GetInvalidFileNameChars();
            return new string(value.Select<char, char>((i) =>
            {
                if (chars.Contains(i))
                    return c;
                return i;
            }).Take(125).ToArray());
        }
        public static bool IsDirectoryEmpty(this DirectoryInfo possiblyEmptyDirectory)
        {
            using (var enumerator = Directory.EnumerateFileSystemEntries(possiblyEmptyDirectory.FullName).GetEnumerator())
            {
                return !enumerator.MoveNext();
            }
        }
        [MethodImpl(MethodImplOptions.AggressiveInlining)]
        internal static bool IsDirectorySeparator(char c)
        {
            return c == DirectorySeparatorChar || c == AltDirectorySeparatorChar;
        }
        internal static string NormalizeDirectorySeparators(string path)
        {
            if (string.IsNullOrEmpty(path))
                return path;
            char current;
            // Make a pass to see if we need to normalize so we can potentially skip allocating
            bool normalized = true;
            for (int i = 0; i < path.Length; i++)
            {
                current = path[i];
                if (IsDirectorySeparator(current)
                    && (current != DirectorySeparatorChar
                        // Check for sequential separators past the first position (we need to keep initial two for UNC/extended)
                        || (i > 0 && i + 1 < path.Length && IsDirectorySeparator(path[i + 1]))))
                {
                    normalized = false;
                    break;
                }
            }
            if (normalized)
                return path;
            StringBuilder builder = new StringBuilder(path.Length);
            int start = 0;
            if (IsDirectorySeparator(path[start]))
            {
                start++;
                builder.Append(DirectorySeparatorChar);
            }
            for (int i = start; i < path.Length; i++)
            {
                current = path[i];
                // If we have a separator
                if (IsDirectorySeparator(current))
                {
                    // If the next is a separator, skip adding this
                    if (i + 1 < path.Length && IsDirectorySeparator(path[i + 1]))
                    {
                        continue;
                    }
                    // Ensure it is the primary separator
                    current = DirectorySeparatorChar;
                }
                builder.Append(current);
            }
            return builder.ToString();
        }
        public static string[] ReadAllLines(this string path)
        {
            string line;
            List<string> lines = new List<string>();
            using (StreamReader sr = new StreamReader(path, new UTF8Encoding(false)))
                while ((line = sr.ReadLine()) != null)
                    lines.Add(line);
            return lines.ToArray();
        }
        public static string ReadAllText(this string path)
        {
            var encoding = new UTF8Encoding(false);
            using (StreamReader sr = new StreamReader(path, encoding, true))
                return sr.ReadToEnd();
        }
    }
}
```

## GenericExtensions


```csharp

namespace Utils{using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
   public static  class GenericExtensions{        public static IEnumerable<TSource> AppendItems<TSource>(this IEnumerable<TSource> source, params TSource[] items)
        {
            return source.Concat(items);
        }
        public static bool AreContinuous<TSource>(this IEnumerable<TSource> source, IEnumerable<TSource> original)
        {
            int num = -1;
            List<TSource> list = System.Linq.Enumerable.ToList(original);
            foreach (TSource item in source)
            {
                int num2 = list.IndexOf(item);
                if (num > -1 && num2 != num + 1)
                {
                    return false;
                }
                num = num2;
            }
            return true;
        }
        public static IEnumerable<T> Except<T>(this IEnumerable<T> items, T itemToExclude)
        {
            if (items == null)
            {
                throw new ArgumentNullException("items");
            }
            return items.Except(new T[1]
            {
        itemToExclude
            });
        }
        public static int FindIndex<T>(this IEnumerable<T> items, Predicate<T> criterion)
        {
            int num = 0;
            foreach (T item in items)
            {
                if (criterion(item))
                {
                    return num;
                }
                num++;
            }
            return -1;
        }
        public static void ForEach<T>(this IEnumerable<T> items, Action<T, int> action)
        {
            int num = 0;
            foreach (T item in items)
            {
                action(item, num);
                num++;
            }
        }
        public static void ForEach<T>(this IEnumerable<T> items, Action<T> action)
        {
            foreach (T item in items)
            {
                action(item);
            }
        }
        public static TSource SingleOrNull<TSource>(this IEnumerable<TSource> source) where TSource : class
        {
            if (source == null)
            {
                return null;
            }
            IList<TSource> list = source as IList<TSource>;
            if (list != null)
            {
                if (list.Count == 1)
                {
                    return list[0];
                }
            }
            else
            {
                using (IEnumerator<TSource> enumerator = source.GetEnumerator())
                {
                    if (!enumerator.MoveNext())
                    {
                        return null;
                    }
                    TSource current = enumerator.Current;
                    if (!enumerator.MoveNext())
                    {
                        return current;
                    }
                }
            }
            return null;
        }
}}
```

## MathExtensions


```csharp

namespace Utils{using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
   public static  class MathExtensions{        public static double Round(this double value)
        {
            double temp = Math.Round(value);
            return (value - temp == 0.5) ? temp + 1 : temp;
        }
}}
```

## RegexExtensions


```csharp

namespace Utils
{
    using System.Text.RegularExpressions;
    public static class RegexExtensions
    {
        // Html
        public const string regJavaScript = @"(?<=&lt;script(?:\s.*?)?&gt;).+?(?=&lt;/script&gt;)";
        public const string regComment = @"&lt;!--.*?--&gt;";
        public const string regAspTag = @"&lt;%@.*?%&gt;|&lt;%=?|%&gt;|&lt;\?php|&lt;\?|\?&gt;";
        public const string regPhpCode = @"(?<=&lt;\?).*?(?=\?&gt;)|(?<=&lt;\?php).*?(?=\?&gt;)";
        public const string regAspCode = @"(?<=&lt;%=?).*?(?=%&gt;)";
        public const string regTagDelimiter = @"(?:&lt;/?!?\??(?!%)|(?<!%)/?&gt;)+";
        public const string regTagName = @"(?<=&lt;/?!?\??(?!%))[\w\.:-]+(?=.*&gt;)";
        public const string regAttributes = @"(?<=&lt;(?!%)/?!?\??[\w:-]+).*?(?=(?<!%)/?&gt;)";
        public const string regEntity = @"&amp;\w+;";
        public const string regAttributeMatch = @"(=?"".*?""|=?'.*?')|([\w:-]+)";
        public static string StripInvalidFileNameChars(this string value,string replace=" ")
        {
            const string re = @"[:*?/\""<>|\0\u0001\u0002\u0003\u0004\u0005\u0006\u0007\u0008\u0009\u000a\u000b\u000c\u000d\u000e\u000f\u0010\u0011\u0012\u0013\u0014\u0015\u0016\u0017\u0018\u0019\u001a\u001b\u001c\u001d\u001e\u001f]";
            return Regex.Replace(value,re,replace);
        }
        public static string StripComments(this string code)
        {
            const string re = @"(@(?:""[^""]*"")+|""(?:[^""\n\\]+|\\.)*""|'(?:[^'\n\\]+|\\.)*')|//.*|/\*(?s:.*?)\*/";
            return Regex.Replace(code, re, "$1");
        }
    }
}
```

## StringExtensions


```csharp

namespace Utils
{
    using System;
    using System.Globalization;
    using System.Runtime.InteropServices;
    using System.Text.RegularExpressions;
    using System.Text;
    public static class StringExtensions
    {
        static string EncodeNonAsciiCharacters(string value)
        {
            StringBuilder sb = new StringBuilder();
            foreach (char c in value)
            {
                if (c > 127)
                {
                    // This character is too big for ASCII
                    string encodedValue = "\\u" + ((int)c).ToString("x4");
                    sb.Append(encodedValue);
                }
                else
                {
                    sb.Append(c);
                }
            }
            return sb.ToString();
        }
       public static string EncodeCharacters(this string value)
        {
            StringBuilder sb = new StringBuilder();
            foreach (char c in value)
            {

                    string encodedValue = "\\u" + ((int)c).ToString("x4");
                    sb.Append(encodedValue);

            }
            return sb.ToString();

        }
        static string DecodeEncodedNonAsciiCharacters(string value)
        {
            return Regex.Replace(
                value,
                @"\\u(?<Value>[a-zA-Z0-9]{4})",
                m => {
                    return ((char)int.Parse(m.Groups["Value"].Value, NumberStyles.HexNumber)).ToString();
                });
        }
        private static readonly CompareInfo compareInfo = CultureInfo.InvariantCulture.CompareInfo;
        public static string Capitalize(this string value)
        {
            //  && char.IsLower(value[0])
            if (!string.IsNullOrEmpty(value))
            {
                return value.Substring(0, 1).ToUpper() + value.Substring(1);
            }
            return value;
        }
        public static string DeCapitalize(this string value)
        {
            //  && char.IsLower(value[0])
            if (!string.IsNullOrEmpty(value))
            {
                return value.Substring(0, 1).ToLower() + value.Substring(1);
            }
            return value;
        }
        public static bool IsEffectivelyEmpty(this ReadOnlySpan<char> path)
        {
            if (path.IsEmpty)
                return true;
            foreach (char c in path)
            {
                if (c != ' ')
                    return false;
            }
            return true;
        }
        public static bool IsReadable(this string value)
        {
            return !string.IsNullOrWhiteSpace(value);
        }
        public static bool IsVacuum(this string value)
        {
            if (value == null) return true;
            ReadOnlySpan<char> buffer = value.AsSpan();
            for (int i = 0; i < buffer.Length; i++)
            {
                if (!char.IsWhiteSpace(buffer[i])) return false;
            }
            return true;
        }
        public static bool IsWhiteSpace(this string value)
        {
            if (value == null) return true;
            for (int i = 0; i < value.Length; i++)
            {
                if (!char.IsWhiteSpace(value[i])) return false;
            }
            return true;
        }
        public static string RemoveNewLine(this string value)
        {
            return Regex.Replace(value, "[\r\n]+", "");
        }
        public static string ReplaceFirst(this string line, string str1, string str2)
        {
            int idx = line.IndexOf(str1, StringComparison.Ordinal);
            if (idx >= 0)
            {
                line = line.Remove(idx, str1.Length);
                line = line.Insert(idx, str2);
            }
            return line;
        }
        public static string SubstringAfter(this string value, char delimiter)
        {
            var index = value.IndexOf(delimiter);
            if (index == -1)
                return value;
            else
                return value.Substring(index + 1);
        }
        public static string SubstringAfter(this string s1, string s2)
        {
            if (s2.Length == 0) { return s1; }
            //int idx = collation.IndexOf(s1, s2);
            int idx = compareInfo.IndexOf(s1, s2, CompareOptions.Ordinal);
            return (idx < 0) ? string.Empty : s1.Substring(idx + s2.Length);
        }
        public static string SubstringAfterLast(this string value, char delimiter)
        {
            var index = value.LastIndexOf(delimiter);
            if (index == -1)
                return value;
            else
                return value.Substring(index + 1);
        }
        public static string SubstringAfterLast(this string value, string delimiter)
        {
            var index = value.LastIndexOf(delimiter);
            if (index == -1)
                return value;
            else
                return value.Substring(index + 1);
        }
        public static string SubstringBefore(this string value, char delimiter)
        {
            var index = value.IndexOf(delimiter);
            if (index == -1)
                return value;
            else
                return value.Substring(0, index);
        }
        public static string SubstringBefore(this string s1, string s2)
        {
            if (s2.Length == 0) { return s2; }
            //int idx = collation.IndexOf(s1, s2);
            int idx = compareInfo.IndexOf(s1, s2, CompareOptions.Ordinal);
            return (idx < 1) ? string.Empty : s1.Substring(0, idx);
        }
        public static string SubstringBeforeLast(this string value, char delimiter)
        {
            var s = value.AsSpan();
            var index = s.LastIndexOf(delimiter);
            if (index == -1)
                return value;
            else
            {
                var r = new string('\0', index);
                var writeable = MemoryMarshal.AsMemory(r.AsMemory()).Span;
                s.Slice(0, index).CopyTo(writeable);
                return r;
            }
        }
        public static string SubstringBeforeLast(this string value, string delimiter)
        {
            var index = value.LastIndexOf(delimiter);
            if (index == -1)
                return value;
            else
                return value.Substring(0, index);
        }
    }
}
```

## ZipExtensions


```csharp

namespace Utils{using System;
using System.Buffers;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.IO.Compression;
using System.Linq;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;
using System.Text;
using System.Text.RegularExpressions;
   public static  class ZipExtensions{
        public static void CreateZipFromAndroidProject(this string dir,
            CompressionLevel compressionLevel = CompressionLevel.Fastest)
        {
            var targetFile = dir + ".zip";
            var encoding = Encoding.GetEncoding("gbk");
            using (ZipArchive archive = ZipFile.Open(targetFile, ZipArchiveMode.Create, encoding))
            {
                bool directoryIsEmpty = true;
                var di = new DirectoryInfo(dir);
                var basePath = di.FullName;
                const int DefaultCapacity = 260;
                char[] entryNameBuffer = ArrayPool<char>.Shared.Rent(DefaultCapacity);
                try
                {
                    var gradleFiles = new DirectoryInfo(Path.Combine(basePath, "gradle")).GetFileSystemInfos("*", SearchOption.AllDirectories);
                    var appFiles = new DirectoryInfo(Path.Combine(basePath, "app")).GetFileSystemInfos("*");
                    var srcFiles = new DirectoryInfo(Path.Combine(basePath, "app\\src")).GetFileSystemInfos("*", SearchOption.AllDirectories);
                    var files = di.GetFileSystemInfos("*");
                    var list = files.Concat(gradleFiles).Concat(appFiles).Concat(srcFiles);
                    foreach (var file in list)
                    {
                        directoryIsEmpty = false;
                        int entryNameLength = file.FullName.Length - basePath.Length;
                        if (file is FileInfo)
                        {
                            string entryName = EntryFromPath(file.FullName, basePath.Length, entryNameLength, ref entryNameBuffer);
                            DoCreateEntryFromFile(archive, file.FullName, entryName, compressionLevel);
                        }
                        else
                        {
                            DirectoryInfo possiblyEmtpy = file as DirectoryInfo;
                            if (possiblyEmtpy != null && possiblyEmtpy.IsDirectoryEmpty())
                            {
                                string entryName = EntryFromPath(file.FullName, basePath.Length, entryNameLength, ref entryNameBuffer, true);
                                archive.CreateEntry(entryName);
                            }
                        }
                    }
                    //if (directoryIsEmpty)
                    //{
                    //    archive.CreateEntry(EntryFromPath(di.Name, 0, di.Name.Length, ref entryNameBuffer, true));
                    //}
                }
                finally
                {
                    ArrayPool<char>.Shared.Return(entryNameBuffer);
                }
            }
        }
        public static void CreateZipFromDirectory(this string dir)
        {
            var targetFile = Path.Combine(dir, Path.GetFileName(dir) + ".zip");
            ZipFile.CreateFromDirectory(dir, targetFile,
                CompressionLevel.Fastest,
                false,
                Encoding.GetEncoding("gbk"));
        }
        internal static ZipArchiveEntry DoCreateEntryFromFile(this ZipArchive destination,
                                                             string sourceFileName, string entryName, CompressionLevel? compressionLevel)
        {
            if (destination == null)
                throw new ArgumentNullException(nameof(destination));
            if (sourceFileName == null)
                throw new ArgumentNullException(nameof(sourceFileName));
            if (entryName == null)
                throw new ArgumentNullException(nameof(entryName));
            // Checking of compressionLevel is passed down to DeflateStream and the IDeflater implementation
            // as it is a pluggable component that completely encapsulates the meaning of compressionLevel.
            // Argument checking gets passed down to FileStream's ctor and CreateEntry
            using (Stream fs = new FileStream(sourceFileName, FileMode.Open, FileAccess.Read, FileShare.Read, bufferSize: 0x1000, useAsync: false))
            {
                ZipArchiveEntry entry = compressionLevel.HasValue
                                    ? destination.CreateEntry(entryName, compressionLevel.Value)
                                    : destination.CreateEntry(entryName);
                DateTime lastWrite = File.GetLastWriteTime(sourceFileName);
                // If file to be archived has an invalid last modified time, use the first datetime representable in the Zip timestamp format
                // (midnight on January 1, 1980):
                if (lastWrite.Year < 1980 || lastWrite.Year > 2107)
                    lastWrite = new DateTime(1980, 1, 1, 0, 0, 0);
                entry.LastWriteTime = lastWrite;
                using (Stream es = entry.Open())
                    fs.CopyTo(es);
                return entry;
            }
        }
        public static void EnsureCapacity(ref char[] buffer, int min)
        {
            if (buffer.Length < min)
            {
                int newCapacity = buffer.Length * 2;
                if (newCapacity < min)
                    newCapacity = min;
                ArrayPool<char>.Shared.Return(buffer);
                buffer = ArrayPool<char>.Shared.Rent(newCapacity);
            }
        }
        public static string EntryFromPath(this string entry, int offset, int length, ref char[] buffer, bool appendPathSeparator = false)
        {
            const char PathSeparator = '/';
            while (length > 0)
            {
                if (entry[offset] != Path.DirectorySeparatorChar &&
                    entry[offset] != Path.AltDirectorySeparatorChar)
                    break;
                offset++;
                length--;
            }
            if (length == 0)
                return appendPathSeparator ? PathSeparator.ToString() : string.Empty;
            int resultLength = appendPathSeparator ? length + 1 : length;
            EnsureCapacity(ref buffer, resultLength);
            entry.CopyTo(offset, buffer, 0, length);
            for (int i = 0; i < length; i++)
            {
                char ch = buffer[i];
                if (ch == Path.DirectorySeparatorChar || ch == Path.AltDirectorySeparatorChar)
                    buffer[i] = PathSeparator;
            }
            if (appendPathSeparator)
                buffer[length] = PathSeparator;
            return new string(buffer, 0, resultLength);
        }
}}
```


