# Java: utils

## App


```java

package euphoria.psycho.utils;
import android.app.Application;
public class App extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        ViewUtils.initialize(this);
    }
}
```

## BaseAppCompatActivity


```java

package euphoria.psycho.utils;
import android.content.SharedPreferences;
import android.content.pm.PackageManager;
import android.os.Build;
import android.os.Bundle;
import android.preference.PreferenceManager;
import android.view.Menu;
import android.widget.Toast;
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;
import androidx.appcompat.widget.Toolbar;
import euphoria.psycho.player.R;
public abstract class BaseAppCompatActivity extends AppCompatActivity {
    public static final int REQUEST_CODE_PERMISSIONS = 11;
    public SharedPreferences mPreferences;
    private Toolbar mToolbar;
    public void bindViews() {
    }
    protected abstract int getLayoutId();
    protected abstract String[] getNeedPermissions();
    protected abstract int getOptionsMenu();
    public Toolbar getToolbar() {
        return mToolbar;
    }
    public void initView() {
    }
    public void initialize() {
        mPreferences = PreferenceManager.getDefaultSharedPreferences(this);
        setContentView(getLayoutId());
        setToolbar();
        bindViews();
        initView();
    }
    public void setToolbar() {
        mToolbar = findViewById(R.id.toolbar);
        if (mToolbar != null) {
            setSupportActionBar(mToolbar);
            getSupportActionBar().setDisplayHomeAsUpEnabled(true);
        }
    }
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        String[] permissions = getNeedPermissions();
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M
                && permissions != null
                && permissions.length > 0) {
            requestPermissions(permissions, REQUEST_CODE_PERMISSIONS);
        } else {
            initialize();
        }
    }
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        if (getOptionsMenu() > 0) {
            getMenuInflater().inflate(getOptionsMenu(), menu);
        }
        return super.onCreateOptionsMenu(menu);
    }
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        for (int i = 0; i < grantResults.length; i++) {
            if (grantResults[i] != PackageManager.PERMISSION_GRANTED) {
                Toast.makeText(this, "Missing necessary permission: " + permissions[i], Toast.LENGTH_LONG).show();
                return;
            }
        }
        initialize();
    }
}
```

## BaseService


```java

package euphoria.psycho.utils;
import android.app.Notification;
import android.app.NotificationChannel;
import android.app.NotificationManager;
import android.app.Service;
import android.graphics.Color;
public abstract class BaseService extends Service {
    protected static final String PRIMARY_CHANNEL = "default";
    private static final String PRIMARY_CHANNEL_STRING = "Primary Channel";
    protected NotificationManager mNotificationManager;
    public static final String EXTRA_COMMAND = "command";
    protected void createChannel() {
        if (android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.O) {
            NotificationChannel channel =
                    new NotificationChannel(PRIMARY_CHANNEL,
                            PRIMARY_CHANNEL_STRING,
                            NotificationManager.IMPORTANCE_LOW);
            // public int getLightColor ()
            // Returns the notification light color for notifications
            // posted to this channel. Irrelevant unless shouldShowLights().
            channel.setLightColor(Color.GREEN);
            // public void setLockscreenVisibility (int lockscreenVisibility)
            // Sets whether notifications posted to this channel appear
            // on the lockscreen or not, and if so, whether they appear
            // in a redacted form. See e.g. Notification.VISIBILITY_SECRET.
            // Only modifiable by the system and notification ranker.
            // public static final int VISIBILITY_PRIVATE
            // Notification visibility: Show this notification on
            // all lockscreens, but conceal sensitive or private information
            // on secure lockscreens.
            channel.setLockscreenVisibility(Notification.VISIBILITY_PRIVATE);
            mNotificationManager.createNotificationChannel(channel);
        }
    }
}
```

## BitmapUtils


```java

package euphoria.psycho.utils;
import android.graphics.Bitmap;
import android.graphics.Bitmap.CompressFormat;
import android.graphics.BitmapFactory;
import android.graphics.Canvas;
import android.graphics.Matrix;
import android.graphics.Paint;
import android.os.Build;
import android.util.Log;
import java.io.ByteArrayOutputStream;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import euphoria.psycho.common.Shared;
import static euphoria.psycho.common.CheckUtils.assertTrue;
public class BitmapUtils {
    private static final String TAG = "BitmapUtils";
    private static final int DEFAULT_JPEG_QUALITY = 90;
    public static final int UNCONSTRAINED = -1;
    private BitmapUtils() {
    }
    /*
     * Compute the sample size as a function of minSideLength
     * and maxNumOfPixels.
     * minSideLength is used to specify that minimal width or height of a
     * bitmap.
     * maxNumOfPixels is used to specify the maximal size in pixels that is
     * tolerable in terms of memory usage.
     *
     * The function returns a sample size based on the constraints.
     * Both size and minSideLength can be passed in as UNCONSTRAINED,
     * which indicates no care of the corresponding constraint.
     * The functions prefers returning a sample size that
     * generates a smaller bitmap, unless minSideLength = UNCONSTRAINED.
     *
     * Also, the function rounds up the sample size to a power of 2 or multiple
     * of 8 because BitmapFactory only honors sample size this way.
     * For example, BitmapFactory downsamples an image by 2 even though the
     * request is 3. So we round up the sample size to avoid OOM.
     */
    public static int computeSampleSize(int width, int height,
                                        int minSideLength, int maxNumOfPixels) {
        int initialSize = computeInitialSampleSize(
                width, height, minSideLength, maxNumOfPixels);
        return initialSize <= 8
                ? Shared.nextPowerOf2(initialSize)
                : (initialSize + 7) / 8 * 8;
    }
    private static int computeInitialSampleSize(int w, int h,
                                                int minSideLength, int maxNumOfPixels) {
        if (maxNumOfPixels == UNCONSTRAINED
                && minSideLength == UNCONSTRAINED) return 1;
        int lowerBound = (maxNumOfPixels == UNCONSTRAINED) ? 1 :
                (int) Math.ceil(Math.sqrt((double) (w * h) / maxNumOfPixels));
        if (minSideLength == UNCONSTRAINED) {
            return lowerBound;
        } else {
            int sampleSize = Math.min(w / minSideLength, h / minSideLength);
            return Math.max(sampleSize, lowerBound);
        }
    }
    // This computes a sample size which makes the longer side at least
    // minSideLength long. If that's not possible, return 1.
    public static int computeSampleSizeLarger(int w, int h,
                                              int minSideLength) {
        int initialSize = Math.max(w / minSideLength, h / minSideLength);
        if (initialSize <= 1) return 1;
        return initialSize <= 8
                ? Shared.prevPowerOf2(initialSize)
                : initialSize / 8 * 8;
    }
    // Find the min x that 1 / x >= scale
    public static int computeSampleSizeLarger(float scale) {
        int initialSize = (int) Math.floor(1d / scale);
        if (initialSize <= 1) return 1;
        return initialSize <= 8
                ? Shared.prevPowerOf2(initialSize)
                : initialSize / 8 * 8;
    }
    // Find the max x that 1 / x <= scale.
    public static int computeSampleSize(float scale) {
        assertTrue(scale > 0);
        int initialSize = Math.max(1, (int) Math.ceil(1 / scale));
        return initialSize <= 8
                ? Shared.nextPowerOf2(initialSize)
                : (initialSize + 7) / 8 * 8;
    }
    public static Bitmap resizeBitmapByScale(
            Bitmap bitmap, float scale, boolean recycle) {
        int width = Math.round(bitmap.getWidth() * scale);
        int height = Math.round(bitmap.getHeight() * scale);
        if (width == bitmap.getWidth()
                && height == bitmap.getHeight()) return bitmap;
        Bitmap target = Bitmap.createBitmap(width, height, getConfig(bitmap));
        Canvas canvas = new Canvas(target);
        canvas.scale(scale, scale);
        Paint paint = new Paint(Paint.FILTER_BITMAP_FLAG | Paint.DITHER_FLAG);
        canvas.drawBitmap(bitmap, 0, 0, paint);
        if (recycle) bitmap.recycle();
        return target;
    }
    private static Bitmap.Config getConfig(Bitmap bitmap) {
        Bitmap.Config config = bitmap.getConfig();
        if (config == null) {
            config = Bitmap.Config.ARGB_8888;
        }
        return config;
    }
    public static Bitmap resizeDownBySideLength(
            Bitmap bitmap, int maxLength, boolean recycle) {
        int srcWidth = bitmap.getWidth();
        int srcHeight = bitmap.getHeight();
        float scale = Math.min(
                (float) maxLength / srcWidth, (float) maxLength / srcHeight);
        if (scale >= 1.0f) return bitmap;
        return resizeBitmapByScale(bitmap, scale, recycle);
    }
    public static Bitmap resizeAndCropCenter(Bitmap bitmap, int size, boolean recycle) {
        int w = bitmap.getWidth();
        int h = bitmap.getHeight();
        if (w == size && h == size) return bitmap;
        // scale the image so that the shorter side equals to the target;
        // the longer side will be center-cropped.
        float scale = (float) size / Math.min(w, h);
        Bitmap target = Bitmap.createBitmap(size, size, getConfig(bitmap));
        int width = Math.round(scale * bitmap.getWidth());
        int height = Math.round(scale * bitmap.getHeight());
        Canvas canvas = new Canvas(target);
        canvas.translate((size - width) / 2f, (size - height) / 2f);
        canvas.scale(scale, scale);
        Paint paint = new Paint(Paint.FILTER_BITMAP_FLAG | Paint.DITHER_FLAG);
        canvas.drawBitmap(bitmap, 0, 0, paint);
        if (recycle) bitmap.recycle();
        return target;
    }
    public static void recycleSilently(Bitmap bitmap) {
        if (bitmap == null) return;
        try {
            bitmap.recycle();
        } catch (Throwable t) {
            Log.w(TAG, "unable recycle bitmap", t);
        }
    }
    public static Bitmap rotateBitmap(Bitmap source, int rotation, boolean recycle) {
        if (rotation == 0) return source;
        int w = source.getWidth();
        int h = source.getHeight();
        Matrix m = new Matrix();
        m.postRotate(rotation);
        Bitmap bitmap = Bitmap.createBitmap(source, 0, 0, w, h, m, true);
        if (recycle) source.recycle();
        return bitmap;
    }
    public static Bitmap createVideoThumbnail(String filePath) {
        // MediaMetadataRetriever is available on API Level 8
        // but is hidden until API Level 10
        Class<?> clazz = null;
        Object instance = null;
        try {
            clazz = Class.forName("android.media.MediaMetadataRetriever");
            instance = clazz.newInstance();
            Method method = clazz.getMethod("setDataSource", String.class);
            method.invoke(instance, filePath);
            // The method name changes between API Level 9 and 10.
            if (Build.VERSION.SDK_INT <= 9) {
                return (Bitmap) clazz.getMethod("captureFrame").invoke(instance);
            } else {
                byte[] data = (byte[]) clazz.getMethod("getEmbeddedPicture").invoke(instance);
                if (data != null) {
                    Bitmap bitmap = BitmapFactory.decodeByteArray(data, 0, data.length);
                    if (bitmap != null) return bitmap;
                }
                return (Bitmap) clazz.getMethod("getFrameAtTime").invoke(instance);
            }
        } catch (IllegalArgumentException ex) {
            // Assume this is a corrupt video file
        } catch (RuntimeException ex) {
            // Assume this is a corrupt video file.
        } catch (InstantiationException e) {
            Log.e(TAG, "createVideoThumbnail", e);
        } catch (InvocationTargetException e) {
            Log.e(TAG, "createVideoThumbnail", e);
        } catch (ClassNotFoundException e) {
            Log.e(TAG, "createVideoThumbnail", e);
        } catch (NoSuchMethodException e) {
            Log.e(TAG, "createVideoThumbnail", e);
        } catch (IllegalAccessException e) {
            Log.e(TAG, "createVideoThumbnail", e);
        } finally {
            try {
                if (instance != null) {
                    clazz.getMethod("release").invoke(instance);
                }
            } catch (Exception ignored) {
            }
        }
        return null;
    }
    public static byte[] compressToBytes(Bitmap bitmap) {
        return compressToBytes(bitmap, DEFAULT_JPEG_QUALITY);
    }
    public static byte[] compressToBytes(Bitmap bitmap, int quality) {
        ByteArrayOutputStream baos = new ByteArrayOutputStream(65536);
        bitmap.compress(CompressFormat.JPEG, quality, baos);
        return baos.toByteArray();
    }
    public static boolean isSupportedByRegionDecoder(String mimeType) {
        if (mimeType == null) return false;
        mimeType = mimeType.toLowerCase();
        return mimeType.startsWith("image/") &&
                (!mimeType.equals("image/gif") && !mimeType.endsWith("bmp"));
    }
    public static boolean isRotationSupported(String mimeType) {
        if (mimeType == null) return false;
        mimeType = mimeType.toLowerCase();
        return mimeType.equals("image/jpeg");
    }
}
```

## FileUtils


```java

package euphoria.psycho.utils;
import android.content.Context;
import android.content.DialogInterface;
import android.net.Uri;
import android.os.Build;
import android.os.Environment;
import android.provider.DocumentsContract;
import android.util.Log;
import android.view.WindowManager;
import android.widget.EditText;
import java.io.Closeable;
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.Reader;
import java.nio.file.Files;
import java.nio.file.Path;
import java.text.Collator;
import java.text.DecimalFormat;
import java.util.Arrays;
import java.util.List;
import java.util.Locale;
import java.util.regex.Pattern;
import androidx.annotation.RequiresApi;
import androidx.appcompat.app.AlertDialog;
import androidx.documentfile.provider.DocumentFile;
import euphoria.psycho.common.AlgorithmUtils;
import euphoria.psycho.common.Shared;
import static euphoria.psycho.common.AlgorithmUtils.linearSearch;
import static euphoria.psycho.common.CheckUtils.isEmpty;
public class FileUtils {
    private static final int BUFFER_SIZE = 8192;
    private static final String[] sAudioExtensions = new String[]{
            ".m4a",
            ".aac",
            ".flac",
            ".gsm",
            ".mid",
            ".xmf",
            ".mxmf",
            ".rtttl",
            ".rtx",
            ".ota",
            ".imy",
            ".mp3",
            ".wav",
            ".ogg"
    };
    private static String sRemovablePath = null;
    private static final String[] sVideoExtensions = new String[]{
            ".3gp",
            ".mp4",
            ".ts",
            ".webm",
            ".mkv",
    };
    public static String changeExtension(String path, String extension) {
        if (path != null) {
            String s = path;
            int length = path.length();
            for (int i = length; --i >= 0; ) {
                char ch = path.charAt(i);
                if (ch == '.') {
                    s = path.substring(0, i);
                    break;
                }
                if (ch == File.separatorChar)
                    break;
            }
            if (extension != null && path.length() != 0) {
                if (extension.length() == 0 || extension.charAt(0) != '.') {
                    s = s + ".";
                }
                s = s + extension;
            }
            return s;
        }
        return null;
    }
    public static String changeFileName(String path, String fileName) {
        return getDirectoryName(path) + File.separatorChar + fileName;
    }
    public static void closeSilently(Closeable c) {
        if (c == null) return;
        try {
            c.close();
        } catch (IOException t) {
        }
    }
    public static int copyAllBytes(InputStream in, OutputStream out) throws IOException {
        int byteCount = 0;
        byte[] buffer = new byte[BUFFER_SIZE];
        while (true) {
            int read = in.read(buffer);
            if (read == -1) {
                break;
            }
            out.write(buffer, 0, read);
            byteCount += read;
        }
        return byteCount;
    }
    public static String formatSize(long size) {
        if (size <= 0)
            return "0 B";
        String[] units = new String[]{"B", "kB", "MB", "GB", "TB"};
        double digitGroups = (int) (Math.log10((double) size) / Math.log10(1024.0));
        return new DecimalFormat("#,##0.#").format(size / Math.pow(1024.0, digitGroups))
                + " " + units[(int) digitGroups];
    }
    public static int getDirectoryChildCount(File dir) {
        File[] files = dir.listFiles();
        if (files == null) return 0;
        else return files.length;
    }
    public static String getDirectoryName(String path) {
        int length = path.length();
        char ch = File.separatorChar;
        for (int i = length; --i >= 0; ) {
            if (path.charAt(i) == ch) {
                return path.substring(0, i);
            }
        }
        return "";
    }
    public static DocumentFile getDocumentFile(Context context, String pathName, Uri treeUri) {
        if (treeUri != null) {
            File[] sdcardPath = new File("/storage").listFiles();
            if (sdcardPath != null && sdcardPath.length >= 2) {
                String extSdcardPath = sdcardPath[1].getAbsolutePath();
                String truePath = pathName.substring(extSdcardPath.length() + 1, pathName.length());//除去root之外的
                String rootDocumentId = getRootDocumentId(context, treeUri);
                String documentId = rootDocumentId + truePath;
                //这里存在两种方式 一种会生成singleDocumentFile 另一种生成TreeDocumentFile
//            DocumentFile documentFile = DocumentFile.fromTreeUri(context, DocumentsContract.buildTreeDocumentUri(treeUri.getAuthority(), documentId));
                DocumentFile documentFile = null;
                if (android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.LOLLIPOP) {
                    documentFile = DocumentFile.fromSingleUri(context, DocumentsContract.buildDocumentUriUsingTree(treeUri, documentId));
                }
                return documentFile;
            }
        }
        return null;
    }
    public static String getExtension(String fileName) {
        if (fileName == null) return null;
        int length = fileName.length();
        for (int i = length; --i >= 0; ) {
            char ch = fileName.charAt(i);
            if (ch == '.') {
                if (i != length - 1)
                    return fileName.substring(i);
                else
                    return null;
            }
            if (ch == File.separatorChar)
                break;
        }
        return null;
    }
    public static File getExternalStorageFile(String fileName) {
        return new File(Environment.getExternalStorageDirectory(), fileName);
    }
    public static String getFileName(String path) {
        if (path != null) {
            int length = path.length();
            for (int i = length; --i >= 0; ) {
                char ch = path.charAt(i);
                if (ch == File.separatorChar)
                    return path.substring(i + 1);
            }
        }
        return path;
    }
    @RequiresApi(api = Build.VERSION_CODES.O)
    public static String getFileName(Path filePath) {
        String path = filePath.getFileName().toString();
        if (path != null) {
            int length = path.length();
            for (int i = length; --i >= 0; ) {
                char ch = path.charAt(i);
                if (ch == File.separatorChar)
                    return path.substring(i + 1);
            }
        }
        return path;
    }
    public static String getRemovableStoragePath() {
        if (sRemovablePath != null) return sRemovablePath;
        File fileList[] = new File("/storage/").listFiles();
        for (File file : fileList) {
            if (!file.getAbsolutePath().equalsIgnoreCase(Environment.getExternalStorageDirectory().getAbsolutePath()) && file.isDirectory() && file.canRead())
                sRemovablePath = file.getAbsolutePath();
        }
        return sRemovablePath;
    }
    public static String getRootDocumentId(Context context, Uri treeUri) {
        if (treeUri != null) {
            DocumentFile documentFile = DocumentFile.fromTreeUri(context, treeUri);
            return DocumentsContract.getDocumentId(documentFile.getUri());
        }
        return null;
    }
    public static boolean isAudio(File file) {
        String ext = getExtension(file.getName());
        if (ext == null) return false;
        return AlgorithmUtils.linearSearch(sAudioExtensions, ext) != -1;
    }
    public static boolean isAudio(String file) {
        String ext = getExtension(file);
        if (ext == null) return false;
        return AlgorithmUtils.linearSearch(sAudioExtensions, ext) != -1;
    }
    @RequiresApi(api = Build.VERSION_CODES.O)
    public static boolean isVideo(Path path) {
        if (!Files.isRegularFile(path)) return false;
        final Pattern pattern = Pattern.compile("\\.(?:mp4|3gp)$");
        return pattern.matcher(path.getFileName().toString()).find();
    }
    public static boolean isVideo(File file) {
        String ext = getExtension(file.getName());
        if (ext == null) return false;
        return Arrays.binarySearch(sVideoExtensions, ext) != -1;
    }
    public static boolean isVideo(String file) {
        String ext = getExtension(file);
        if (ext == null) return false;
        return linearSearch(sVideoExtensions, ext) != -1;
    }
    public static File[] listFiles(File dir, final String[] extensions) {
        File[] files;
        if (isEmpty(extensions)) {
            files = dir.listFiles();
        } else {
            files = dir.listFiles(file -> {
                if (file.isFile()) {
                    String extension = getExtension(file.getName());
                    for (String e : extensions) {
                        if (e.equals(extension)) return true;
                    }
                    return false;
                }
                return true;
            });
        }
        if (isEmpty(files)) return null;
        final Collator collator = Collator.getInstance(Locale.CHINA);
//
//        int sortBy = 1;
//        boolean ascending = false;
//
//
//        switch (sortBy) {
//            case 1: {
//                if (ascending) {
//
//                } else {
//                    Arrays.sort(files, new Comparator<File>() {
//                        @Override
//                        public int compare(File o1, File o2) {
//                            boolean a = o1.isDirectory();
//                            boolean b = o2.isDirectory();
//
//                            if ((a && b) || (!a && !b)) {
//                                return collator.compare(o1.getName(), o2.getName());
//                            } else if (!a) {
//                                return -1;
//                            }
//                            return 0;
//                        }
//                    });
//                }
//            }
//            break;
//        }
        return files;
    }
    public static String readAllText(Reader reader) throws IOException {
        char[] buffer = new char[BUFFER_SIZE / 2];
        StringBuilder builder = new StringBuilder();
        while (true) {
            int read = reader.read(buffer);
            if (read == -1) {
                break;
            }
            builder.append(buffer, 0, read);
        }
        return builder.toString();
    }
    public static boolean renameFile(String source, String destination) {
        if (source.equals(destination)) return false;
        File sourceFile = new File(source);
        if (!sourceFile.isFile()) return false;
        File dstFile = new File(destination);
        if (dstFile.isFile()) return false;
        return sourceFile.renameTo(dstFile);
    }
    public static void showFileDialog(Context context,
                                      String content,
                                      String title,
                                      DialogListener listener) {
        EditText editText = new EditText(context);
        editText.setMaxLines(1);
        editText.setText(content);
        if (content != null) {
            int pos = content.lastIndexOf('.');
            if (pos > -1) {
                editText.setSelection(0, pos);
            }
        }
        AlertDialog dialog = new AlertDialog.Builder(context)
                .setView(editText)
                .setTitle(title)
                .setNegativeButton(android.R.string.cancel, (dialog1, which) -> {
                    dialog1.dismiss();
                    listener.onDismiss();
                })
                .setPositiveButton(android.R.string.ok, (dialog1, which) -> {
                    listener.onConfirmed(editText.getText());
                    dialog1.dismiss();
                }).create();
        //  Show the input keyboard for user
        dialog.getWindow().setSoftInputMode(WindowManager.LayoutParams.SOFT_INPUT_STATE_VISIBLE);
        dialog.show();
    }
    public interface DialogListener {
        void onConfirmed(CharSequence value);
        void onDismiss();
    }
}
```

## GenericUtils


```java

package euphoria.psycho.utils;
import android.annotation.TargetApi;
import android.os.Build;
import java.util.AbstractMap;
import java.util.Arrays;
import java.util.Iterator;
import java.util.Map;
import java.util.Objects;
import java.util.Optional;
import java.util.function.BiConsumer;
import java.util.function.BiFunction;
import java.util.function.BinaryOperator;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Predicate;
import java.util.stream.Collector;
import java.util.stream.Stream;
import java.util.stream.StreamSupport;
import static java.util.function.Function.identity;
import static java.util.stream.Collectors.counting;
import static java.util.stream.Collectors.groupingBy;
import static java.util.stream.Collectors.toMap;
import static java.util.Arrays.asList;
@TargetApi(Build.VERSION_CODES.N)
public class GenericUtils {
    /*
    Map<Integer,Long> map = Stream.of(1, 1, 1, 3, 3, 6)
    .collect(NCollectors.countingOccurrences());
    // {1=3, 3=2, 6=1}
     */
    public static <T> Collector<T, ?, Map<T, Long>> countingOccurrences() {
        return groupingBy(identity(), counting());
    }
    public static Predicate<String> endsWith(String suffix) {
        return GenericUtils.<String>notNull().and(s -> s.endsWith(suffix));
    }
    public static <T> Predicate<T> is(T value) {
        return x -> x == value;
    }
    public static <T> Predicate<T> isNull() {
        return x -> x == null;
    }
    public static <T> Predicate<T> not(Predicate<T> predicate) {
        return x -> !predicate.test(x);
    }
    public static <T> Predicate<T> not(T value) {
        return not(is(value));
    }
    public static <T> Predicate<T> notNull() {
        return not(isNull());
    }
}
```

## HttpUtils


```java

package euphoria.psycho.utils;
import java.io.IOException;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.List;
import java.util.Map;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
public class HttpUtils {
    public static String dumpHeaders(HttpURLConnection conn) {
        StringBuilder sb = new StringBuilder();
        Map<String, List<String>> headers = conn.getHeaderFields();
        for (Map.Entry<String, List<String>> entry : headers.entrySet()) {
            sb.append(entry.getKey())
                    .append(": ");
            List<String> value = entry.getValue();
            for (String v : value) {
                sb.append(v).append(';');
            }
            sb.append('\n');
        }
        return sb.toString();
    }
    public static String getFileNameFromUrl(String url) {
        int p = url.indexOf('?');
        if (p != -1) {
            url = url.substring(0, p);
        }
        p = url.lastIndexOf('/');
        if (p != -1) {
            url = url.substring(p + 1);
        }
        return url;
    }
    public static boolean isValidUrl(String value) {
        URL url;
        try {
            url = new URL(value);
            url.openConnection();
            return true;
        } catch (IOException e) {
            return false;
        }
    }
}
```

## Preconditions


```java

package euphoria.psycho.utils;
public class Preconditions {
    // Throws AssertionError if the input is false.
    public static void assertTrue(boolean cond) {
        if (!cond) {
            throw new AssertionError();
        }
    }
    // Throws NullPointerException if the input is null.
    public static <T> T checkNotNull(T object) {
        if (object == null) throw new NullPointerException();
        return object;
    }
    public static String ensureNotNull(String value) {
        return value == null ? "" : value;
    }
    public static void fail(String message, Object... args) {
        throw new AssertionError(
                args.length == 0 ? message : String.format(message, args));
    }
}
```

## PriorityThreadFactory


```java

package euphoria.psycho.utils;
import android.os.Process;
import java.util.concurrent.ThreadFactory;
import java.util.concurrent.atomic.AtomicInteger;
/**
 * A thread factory that creates threads with a given thread priority.
 */
public class PriorityThreadFactory implements ThreadFactory {
    private final int mPriority;
    private final AtomicInteger mNumber = new AtomicInteger();
    private final String mName;
    public PriorityThreadFactory(String name, int priority) {
        mName = name;
        mPriority = priority;
    }
    @Override
    public Thread newThread(Runnable r) {
        return new Thread(r, mName + '-' + mNumber.getAndIncrement()) {
            @Override
            public void run() {
                Process.setThreadPriority(mPriority);
                super.run();
            }
        };
    }
}
```

## StringUtils


```java

package euphoria.psycho.utils;
import android.content.Context;
public class StringUtils {
    public static String formatDuration(final Context context, int duration) {
        int h = duration / 3600;
        int m = (duration - h * 3600) / 60;
        int s = duration - (h * 3600 + m * 60);
        String durationValue;
        if (h == 0) {
            durationValue = String.format("%1$02d:%2$02d", m, s);
        } else {
            durationValue = String.format("%1$d:%2$02d:%3$02d", h, m, s);
        }
        return durationValue;
    }
    public static String getString(CharSequence charSequence) {
        if (charSequence == null) return "";
        return charSequence.toString();
    }
    public static boolean isNullOrEmpty(String str) {
        return str == null || str.length() == 0;
    }
    public static boolean isNullOrWhiteSpace(String str) {
        return str == null || str.length() == 0 || str.trim().length() == 0;
    }
}
```

## Utils


```java

package euphoria.psycho.utils;
import android.content.Context;
import android.os.Environment;
import android.os.StatFs;
import android.text.TextUtils;
import android.util.DisplayMetrics;
import android.util.Log;
import android.view.WindowManager;
import java.io.Closeable;
import java.io.IOException;
import java.util.Objects;
import androidx.core.util.Preconditions;
public class Utils {
    public static final boolean DEBUG = true;
    public static final String TAG = "_tag_";
    private static final long INITIALCRC = 0xFFFFFFFFFFFFFFFFL;
    private static final long POLY64REV = 0x95AC9329AC4BC9B5L;
    private static long[] sCrcTable = new long[256];
    static {
        // http://bioinf.cs.ucl.ac.uk/downloads/crc64/crc64.c
        long part;
        for (int i = 0; i < 256; i++) {
            part = i;
            for (int j = 0; j < 8; j++) {
                long x = ((int) part & 1) != 0 ? POLY64REV : 0;
                part = (part >> 1) ^ x;
            }
            sCrcTable[i] = part;
        }
    }
    // Returns the input value x clamped to the range [min, max].
    public static int clamp(int x, int min, int max) {
        if (x > max) return max;
        if (x < min) return min;
        return x;
    }
    // Returns the input value x clamped to the range [min, max].
    public static float clamp(float x, float min, float max) {
        if (x > max) return max;
        if (x < min) return min;
        return x;
    }
    // Returns the input value x clamped to the range [min, max].
    public static long clamp(long x, long min, long max) {
        if (x > max) return max;
        if (x < min) return min;
        return x;
    }
    public static final long crc64Long(String in) {
        if (in == null || in.length() == 0) {
            return 0;
        }
        return crc64Long(getBytes(in));
    }
    public static final long crc64Long(byte[] buffer) {
        long crc = INITIALCRC;
        for (int k = 0, n = buffer.length; k < n; ++k) {
            crc = sCrcTable[(((int) crc) ^ buffer[k]) & 0xff] ^ (crc >> 8);
        }
        return crc;
    }
    // Returns true if two input Object are both null or equal
    // to each other.
    public static boolean equals(Object a, Object b) {
        return (a == b) || (a == null ? false : a.equals(b));
    }
    public static byte[] getBytes(String in) {
        byte[] result = new byte[in.length() * 2];
        int output = 0;
        for (char ch : in.toCharArray()) {
            result[output++] = (byte) (ch & 0xFF);
            result[output++] = (byte) (ch >> 8);
        }
        return result;
    }
    public static boolean hasSpaceForSize(long size) {
        String state = Environment.getExternalStorageState();
        if (!Environment.MEDIA_MOUNTED.equals(state)) {
            return false;
        }
        String path = Environment.getExternalStorageDirectory().getPath();
        try {
            StatFs stat = new StatFs(path);
            return stat.getAvailableBlocksLong() * stat.getBlockSizeLong() > size;
        } catch (Exception e) {
            Log.i(TAG, "Fail to access external storage", e);
        }
        return false;
    }
    public static <T> boolean isSame(T[] t1, T[] t2) {
        if (t1 == null && t2 == null) return true;
        if ((t1 == null || t2 == null) || (t1.length != t2.length)) return false;
        for (int i = 0; i < t1.length; i++) {
            if (t1[i] != t2[i]) return false;
        }
        return true;
    }
    public static <T extends Comparable<T>> int linearSearch(T[] array, T value) {
        for (int i = 0; i < array.length; i++) {
            if (array[i].compareTo(value) == 0) {
                return i;
            }
        }
        return -1;
    }
    // Returns the next power of two.
    // Returns the input if it is already power of 2.
    // Throws IllegalArgumentException if the input is <= 0 or
    // the answer overflows.
    public static int nextPowerOf2(int n) {
        if (n <= 0 || n > (1 << 30)) throw new IllegalArgumentException("n is invalid: " + n);
        n -= 1;
        n |= n >> 16;
        n |= n >> 8;
        n |= n >> 4;
        n |= n >> 2;
        n |= n >> 1;
        return n + 1;
    }
    public static float parseFloatSafely(String content, float defaultValue) {
        if (content == null) return defaultValue;
        try {
            return Float.parseFloat(content);
        } catch (NumberFormatException e) {
            return defaultValue;
        }
    }
    public static int parseIntSafely(String content, int defaultValue) {
        if (content == null) return defaultValue;
        try {
            return Integer.parseInt(content);
        } catch (NumberFormatException e) {
            return defaultValue;
        }
    }
    // Returns the previous power of two.
    // Returns the input if it is already power of 2.
    // Throws IllegalArgumentException if the input is <= 0
    public static int prevPowerOf2(int n) {
        if (n <= 0) throw new IllegalArgumentException();
        return Integer.highestOneBit(n);
    }
    public static final double toMile(double meter) {
        return meter / 1609;
    }
}
```

## ViewUtils


```java

package euphoria.psycho.utils;
import android.content.Context;
import android.util.DisplayMetrics;
import android.view.WindowManager;
public class ViewUtils {
    private static float sPixelDensity = -1f;
    public static float dpToPixel(float dp) {
        return sPixelDensity * dp;
    }
    public static int dpToPixel(int dp) {
        return Math.round(dpToPixel((float) dp));
    }
    public static void initialize(Context context) {
        DisplayMetrics metrics = new DisplayMetrics();
        WindowManager wm = (WindowManager)
                context.getSystemService(Context.WINDOW_SERVICE);
        wm.getDefaultDisplay().getMetrics(metrics);
        sPixelDensity = metrics.density;
    }
    public static int meterToPixel(float meter) {
        // 1 meter = 39.37 inches, 1 inch = 160 dp.
        return Math.round(dpToPixel(meter * 39.37f * 160));
    }
}
```


