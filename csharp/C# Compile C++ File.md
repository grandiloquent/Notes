# C# Compile C++ File

```csharp
namespace CompileCpp
{
	using System;
	using System.Diagnostics;
	using System.IO;
	using System.Linq;
	using System.Runtime.InteropServices;
	using System.Text;
	using System.Text.RegularExpressions;
	using System.Threading;
	using System.Windows.Forms;
	class Program
	{
		public const int WM_HOTKEY = 0x0312;
		[StructLayout(LayoutKind.Sequential)]
		[Serializable]
		public struct MSG
		{
			public IntPtr hwnd;
			public int message;
			public IntPtr wParam;
			public IntPtr lParam;
			public int time;
			// pt was a by-value POINT structure
			public int pt_x;
			public int pt_y;
		}
		//https://referencesource.microsoft.com/#System.Windows.Forms/winforms/Managed/System/WinForms/UnsafeNativeMethods.cs,bf9e9bdf5c11080c
		[DllImport("User32", ExactSpelling = true, CharSet = CharSet.Unicode)]
		public static extern bool GetMessageW([In, Out] ref MSG msg, HandleRef hWnd, int uMsgFilterMin, int uMsgFilterMax);
		[DllImport("User32", CharSet = CharSet.Unicode, SetLastError = true)]
		public static extern bool RegisterHotKey(IntPtr hwnd, int atom, int fsModifiers, int vk);
  
		public static void CompileCpp(string f)
		{
			
//			var dir = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.Desktop), "output");
//			if (!Directory.Exists(dir)) {
//				Directory.CreateDirectory(dir);
//			}
			var dir = Path.Combine(Path.GetDirectoryName(f), "bin");
			if (!Directory.Exists(dir))
				Directory.CreateDirectory(dir);
			var arg = "";
			var argLines = File.ReadLines(f, new UTF8Encoding(false));
			foreach (var element in argLines) {
				if (string.IsNullOrWhiteSpace(element))
					continue;
				if (element.StartsWith("// ")) {
					arg += element.Substring(3) + " ";
				} else
					break;
			}
			var exe = Path.GetFileNameWithoutExtension(f) + ".exe";
				
			try {
			 
				var ps =	Process.GetProcesses().Where(i => i.ProcessName == Path.GetFileNameWithoutExtension(f) || i.ProcessName == "cmd");
				if (ps.Any()) {
					foreach (var p in ps) {
						p.Kill();
					}
				}
			} catch (Exception e) {
				Console.WriteLine("ERROR:" + e.Message);
			}
			// -finput-charset=UTF-8 -fexec-charset=GBK -lstdc++fs  -std=c++17  
			//var cmd = string.Format("/K gcc -Wall -g -finput-charset=GBK -fexec-charset=GBK \"{0}\" -o \"{1}\\{3}\" {2} && \"{1}\\{3}\" ", f, dir, arg, exe);
			var cmd = string.Format("/K gcc -Wall -g \"{0}\" -o \"{1}\\{3}\" {2} && \"{1}\\{3}\" ", f, dir, arg, exe);
		
			Process.Start(new ProcessStartInfo() {
				FileName = "cmd",
				Arguments = cmd,
				WorkingDirectory = dir
			});
				
			
		}
		static string GetClipboardText()
		{
			string res = "";
			Thread staThread = new Thread(x => {
				try {
					res = Clipboard.GetText();
				} catch (Exception ex) {
					res = ex.Message;            
				}
			});
			staThread.SetApartmentState(ApartmentState.STA);
			staThread.Start();
			staThread.Join();
			return res;
		}
		public static void Main(string[] args)
		{
			
			var result =	RegisterHotKey(IntPtr.Zero, 0, 0, 0x78);// 0x78 F9
			if (result)
				Console.WriteLine("Successfully registered hotkey F9.");
			
			var handleRef = new HandleRef(null, IntPtr.Zero);
			MSG msg = new MSG();
			while (GetMessageW(ref msg, handleRef, 0, 0)) {
				if (msg.message == WM_HOTKEY) {
					 
					try {
						var fileName = GetClipboardText();
						 
						if (File.Exists(fileName) && Regex.IsMatch(fileName, "\\.(?:c|cpp)$")) {
							
							CompileCpp(fileName);
						} else {
						 
						}
					} catch (Exception e) {
						Console.WriteLine("ERROR: " + e.Message);
					}
					
					 
				}
			}
		}
	}
}
```


