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

### Android

* `c`

		private static final int $end$ = $value$;

* `cs`

		private static final String $name$ = "$value$";

* `t`

		try {

		} catch (Exception e) {
		    Log.e(TAG, e.getMessage());
		}

* `dp`

		Log.d(TAG,"$method$: "+$value$);

* `use`

		[Left,Top,Right,Bottom];

* `tag`

		private static final String TAG = "TAG/"+$className$.class.getCanonicalName();

* `d`

		Log.d(TAG,"$method$: ");

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

* `logw`

		android.util.Log.w(TAG, "$METHOD_NAME$: $content$", $exception$);

* `wtf`

		android.util.Log.wtf(TAG, "$METHOD_NAME$: $content$", $exception$);

* `logt`

		private static final String TAG = "$className$";

* `logm`

		android.util.Log.d(TAG, $content$);

* `logr`

		android.util.Log.d(TAG, "$METHOD_NAME$() returned: " +  $result$);

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

* `toolsNs`

		xmlns:tools="http://schemas.android.com/tools"

* `lww`

		android:layout_width="wrap_content"

* `lw`

		android:layout_width="$width$"

* `lwm`

		android:layout_width="match_parent"

* `lhw`

		android:layout_height="wrap_content"

* `lhm`

		android:layout_height="match_parent"

* `lh`

		android:layout_height="$height$"

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
