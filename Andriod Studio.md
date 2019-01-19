# Andriod Studio

- [快捷键](#快捷键)
- [配置](#配置)
- [模板](#模板)
	- [Android](#android)
	- [AndroidComments](#androidcomments)
	- [AndroidLog](#androidlog)
	- [AndroidParcelable](#androidparcelable)
	- [AndroidXML](#androidxml)
	- [iterations](#iterations)
	- [other](#other)
	- [plain](#plain)
- [扩展](#扩展)

## 快捷键

|快捷键|菜单|
|---|---|
||Refactor > Extract > Method|
||Refactor > Find and Replace Code Duplicates|
|Alt + 6|View > Tool Windows > Logcat|
|Control + F12|在 Studio 内导航和搜索 > 打开文件结构弹出式菜单|
|Control + Shift + U|Toggle Case|
|Control + D|Compare with CLipboard|
|F1|Code > Reformat Code|
|F5|Run > Run|

## 配置

* File > Settings > Editor > Code Style > Java > Imports > Insert imports for inner classes
* 工具栏 > app > Run/Debug Configurations > Debugger > Debug type > Java

## 模板

* `braces`: Surround with {}
* `callable`: Surround with Callable
* `geti`: Inserts singleton method getInstance
* `htmlorjsp`: Surround with &lt;tag&gt;&lt;/tag&gt; in HTML/JSP
* `inst`: Checks object type with instanceof and down-casts it
* `itar`: Iterate elements of array
* `itco`: Iterate elements of java.util.Collection
* `iten`: Iterate java.util.Enumeration
* `iter`: Iterate Iterable | Array in J2SDK 5.0 syntax
* `itit`: Iterate java.util.Iterator
* `itli`: Iterate elements of java.util.List
* `itover`: Iterate over an Iterable or Array selection in J2SDK 5.0 syntax
* `ittok`: Iterate tokens from String
* `itve`: Iterate elements of java.util.Vector
* `lazy`: Performs lazy initialization
* `lock`: Surround with ReadWriteLock.readLock
* `lock`: Surround with ReadWriteLock.writeLock
* `lst`: Fetches last element of an array
* `mn`: Sets lesser value to a variable
* `mx`: Sets greater value to a variable
* `null`: Inserts &#39;&#39;if not null&#39;&#39; statement
* `null`: Inserts &#39;&#39;if null&#39;&#39; statement
* `pair`: Tag pair
* `parens`: Surround with ()
* `psf`: public static final
* `psfi`: public static final int
* `psfs`: public static final String
* `psvm`: main() method declaration
* `ritar`: Iterate elements of array in reverse order
* `serr`: Prints a string to System.err
* `souf`: Prints a formatted string to System.out
* `sout`: Prints a string to System.out
* `soutm`: Prints current class and method names to System.out
* `soutp`: Prints method parameter names and values to System.out
* `soutv`: Prints a value to System.out
* `st`: String
* `tag`: Surround with &lt;tag&gt;&lt;/tag&gt;
* `thr`: throw new
* `toar`: Stores elements of java.util.Collection into array
* `xmlorhtmlorjsp`: Surround with CDATA section



### Android

* `c`

		private static final int $end$ = $value$;

* `cs`

		private static final String $name$ = "$value$";

* `d`

		Log.d(TAG,"$method$: ");

* `dp`

		Log.d(TAG,"$method$: "+$value$);

* `t`

		try {

		} catch (Exception e) {
		    Log.e(TAG, e.getMessage());
		}

* `tag`

		private static final String TAG = "TAG/"+$className$.class.getCanonicalName();

* `use`

		[Left,Top,Right,Bottom];

### AndroidComments

* `ccode`

		<code>$code$</code>

* `cfalse`

		<code>false</code>

* `clink`

		{@link $classToLink$}

* `ctrue`

		<code>true</code>

* `fixme`

		// FIXME: $date$ $todo$

* `noop`

		/* no-op */

* `stopship`

		// STOPSHIP: $date$ $todo$

* `todo`

		// TODO: $date$ $todo$

### AndroidLog

* `logd`

		android.util.Log.d(TAG, "$METHOD_NAME$: $content$");

* `loge`

		android.util.Log.e(TAG, "$METHOD_NAME$: $content$", $exception$);

* `logi`

		android.util.Log.i(TAG, "$METHOD_NAME$: $content$");

* `logm`

		android.util.Log.d(TAG, $content$);

* `logr`

		android.util.Log.d(TAG, "$METHOD_NAME$() returned: " +  $result$);

* `logt`

		private static final String TAG = "$className$";

* `logw`

		android.util.Log.w(TAG, "$METHOD_NAME$: $content$", $exception$);

* `wtf`

		android.util.Log.wtf(TAG, "$METHOD_NAME$: $content$", $exception$);

### AndroidParcelable

* `Parcelable`

		protected $className$(android.os.Parcel in) {

		}

		@Override
		public int describeContents() {
		  return 0;
		}

		@Override
		public void writeToParcel(@android.support.annotation.NonNull android.os.Parcel dest, int flags) {

		}

		public static final android.os.Parcelable.Creator<$className$> CREATOR = new Parcelable.Creator<$className$>() {
		  @Override
		  public $className$ createFromParcel(Parcel in) {
		    return new $className$(in);
		  }

		  @Override
		  public $className$[] newArray(int size) {
		    return new $className$[size];
		  }
		};

* `ParcelableEnum`

		@Override
		public int describeContents() {
		  return 0;
		}

		@Override
		public void writeToParcel(android.os.Parcel dest, int flags) {
		  dest.writeInt(this.ordinal());
		}

		public static final android.os.Parcelable.Creator<$className$> CREATOR = new Parcelable.Creator<$className$>() {
		  @Override
		  public $className$ createFromParcel(Parcel in) {
		    return $className$.values()[in.readInt()];
		  }

		  @Override
		  public $className$[] newArray(int size) {
		    return new $className$[size];
		  }
		};

* `ParcelableEnumTest`

		@Test
		public void testDescribeContents() throws Exception {
		  for ($className$ value : $className$.values()) {
		    assertEquals(0, value.describeContents());
		  }
		}

		@Test
		public void testWriteToParcel() throws Exception {
		  Parcel parcel;
		  for ($className$ value : $className$.values()) {
		    parcel = Parcel.obtain();

		    Parcel parceled$className$ = ParcelableHelper.writeToParcelAndResetDataPosition(value, 0);
		    value.writeToParcel(parcel, 0);
		    parcel.setDataPosition(0);

		    $className$ unparceled$className$ = $className$.CREATOR.createFromParcel(parceled$className$);
		    assertEquals(value, unparceled$className$);
		  }
		}

		@Test
		public void testArrayParcelable() throws Exception {
		  $className$[] values = $className$.CREATOR.newArray($className$.values().length);
		  assertEquals($className$.values().length, values.length);
		}

* `UnparcelIntArray`

		int $read$[] = in.createIntArray();
		in.readIntArray($read$);

* `UnparcelStringArray`

		String $read$[] = in.createStringArray();
		in.readStringArray($read$);

### AndroidXML

* `appNs`

		xmlns:app="http://schemas.android.com/apk/res-auto"

* `lh`

		android:layout_height="$height$"

* `lhm`

		android:layout_height="match_parent"

* `lhw`

		android:layout_height="wrap_content"

* `lw`

		android:layout_width="$width$"

* `lwm`

		android:layout_width="match_parent"

* `lww`

		android:layout_width="wrap_content"

* `toolsNs`

		xmlns:tools="http://schemas.android.com/tools"

### iterations

* `fori`

		for(int $INDEX$ = 0; $INDEX$ < $LIMIT$; $INDEX$++) {
		  $END$
		}

### other

* `ifn`

		if ($VAR$ == null) {
		$END$
		}

### plain

* `thr`

		throw new


## 扩展

* https://romannurik.github.io/AndroidAssetStudio

## 源代码

* https://github.com/JetBrains/intellij-community
* https://android.googlesource.com/platform/tools/idea/


