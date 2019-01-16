


# Java: ExoPlayer offline

- [ActionFile](#actionfile)
- [DownloadAction](#downloadaction)
- [Downloader](#downloader)
- [DownloaderConstructorHelper](#downloaderconstructorhelper)
- [DownloadException](#downloadexception)
- [DownloadHelper](#downloadhelper)
- [DownloadManager](#downloadmanager)
- [DownloadService](#downloadservice)
- [FilterableManifest](#filterablemanifest)
- [FilteringManifestParser](#filteringmanifestparser)
- [ProgressiveDownloadAction](#progressivedownloadaction)
- [ProgressiveDownloader](#progressivedownloader)
- [ProgressiveDownloadHelper](#progressivedownloadhelper)
- [SegmentDownloadAction](#segmentdownloadaction)
- [SegmentDownloader](#segmentdownloader)
- [StreamKey](#streamkey)
- [TrackKey](#trackkey)

## ActionFile


```java

package com.google.android.exoplayer2.offline;
import com.google.android.exoplayer2.offline.DownloadAction.Deserializer;
import com.google.android.exoplayer2.util.AtomicFile;
import com.google.android.exoplayer2.util.Util;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
/**
 * Stores and loads {@link DownloadAction}s to/from a file.
 */
public final class ActionFile {
  /* package */ static final int VERSION = 0;
  private final AtomicFile atomicFile;
  private final File actionFile;
  /**
   * @param actionFile File to be used to store and load {@link DownloadAction}s.
   */
  public ActionFile(File actionFile) {
    this.actionFile = actionFile;
    atomicFile = new AtomicFile(actionFile);
  }
  /**
   * Loads {@link DownloadAction}s from file.
   *
   * @param deserializers {@link Deserializer}s to deserialize DownloadActions.
   * @return Loaded DownloadActions. If the action file doesn't exists returns an empty array.
   * @throws IOException If there is an error during loading.
   */
  public DownloadAction[] load(Deserializer... deserializers) throws IOException {
    if (!actionFile.exists()) {
      return new DownloadAction[0];
    }
    InputStream inputStream = null;
    try {
      inputStream = atomicFile.openRead();
      DataInputStream dataInputStream = new DataInputStream(inputStream);
      int version = dataInputStream.readInt();
      if (version > VERSION) {
        throw new IOException("Unsupported action file version: " + version);
      }
      int actionCount = dataInputStream.readInt();
      DownloadAction[] actions = new DownloadAction[actionCount];
      for (int i = 0; i < actionCount; i++) {
        actions[i] = DownloadAction.deserializeFromStream(deserializers, dataInputStream);
      }
      return actions;
    } finally {
      Util.closeQuietly(inputStream);
    }
  }
  /**
   * Stores {@link DownloadAction}s to file.
   *
   * @param downloadActions DownloadActions to store to file.
   * @throws IOException If there is an error during storing.
   */
  public void store(DownloadAction... downloadActions) throws IOException {
    DataOutputStream output = null;
    try {
      output = new DataOutputStream(atomicFile.startWrite());
      output.writeInt(VERSION);
      output.writeInt(downloadActions.length);
      for (DownloadAction action : downloadActions) {
        DownloadAction.serializeToStream(action, output);
      }
      atomicFile.endWrite(output);
      // Avoid calling close twice.
      output = null;
    } finally {
      Util.closeQuietly(output);
    }
  }
}
```

## DownloadAction


```java

package com.google.android.exoplayer2.offline;
import android.net.Uri;
import android.support.annotation.Nullable;
import com.google.android.exoplayer2.util.Assertions;
import com.google.android.exoplayer2.util.Util;
import java.io.ByteArrayOutputStream;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;
/** Contains the necessary parameters for a download or remove action. */
public abstract class DownloadAction {
  /** Used to deserialize {@link DownloadAction}s. */
  public abstract static class Deserializer {
    public final String type;
    public final int version;
    public Deserializer(String type, int version) {
      this.type = type;
      this.version = version;
    }
    /**
     * Deserializes an action from the {@code input}.
     *
     * @param version The version of the serialized action.
     * @param input The stream from which to read the action.
     * @see DownloadAction#writeToStream(DataOutputStream)
     */
    public abstract DownloadAction readFromStream(int version, DataInputStream input)
        throws IOException;
  }
  private static @Nullable Deserializer[] defaultDeserializers;
  /** Returns available default {@link Deserializer}s. */
  public static synchronized Deserializer[] getDefaultDeserializers() {
    if (defaultDeserializers != null) {
      return defaultDeserializers;
    }
    Deserializer[] deserializers = new Deserializer[4];
    int count = 0;
    deserializers[count++] = ProgressiveDownloadAction.DESERIALIZER;
    Class<?> clazz;
    // Full class names used for constructor args so the LINT rule triggers if any of them move.
    try {
      // LINT.IfChange
      clazz = Class.forName("com.google.android.exoplayer2.source.dash.offline.DashDownloadAction");
      // LINT.ThenChange(../../../../../../../../../dash/proguard-rules.txt)
      deserializers[count++] = getDeserializer(clazz);
    } catch (Exception e) {
      // Do nothing.
    }
    try {
      // LINT.IfChange
      clazz = Class.forName("com.google.android.exoplayer2.source.hls.offline.HlsDownloadAction");
      // LINT.ThenChange(../../../../../../../../../hls/proguard-rules.txt)
      deserializers[count++] = getDeserializer(clazz);
    } catch (Exception e) {
      // Do nothing.
    }
    try {
      // LINT.IfChange
      clazz =
          Class.forName(
              "com.google.android.exoplayer2.source.smoothstreaming.offline.SsDownloadAction");
      // LINT.ThenChange(../../../../../../../../../smoothstreaming/proguard-rules.txt)
      deserializers[count++] = getDeserializer(clazz);
    } catch (Exception e) {
      // Do nothing.
    }
    defaultDeserializers = Arrays.copyOf(Assertions.checkNotNull(deserializers), count);
    return defaultDeserializers;
  }
  /**
   * Deserializes one action that was serialized with {@link #serializeToStream(DownloadAction,
   * OutputStream)} from the {@code input}, using the {@link Deserializer}s that supports the
   * action's type.
   *
   * <p>The caller is responsible for closing the given {@link InputStream}.
   *
   * @param deserializers {@link Deserializer}s for supported actions.
   * @param input The stream from which to read the action.
   * @return The deserialized action.
   * @throws IOException If there is an IO error reading from {@code input}, or if the action type
   *     isn't supported by any of the {@code deserializers}.
   */
  public static DownloadAction deserializeFromStream(
      Deserializer[] deserializers, InputStream input) throws IOException {
    // Don't close the stream as it closes the underlying stream too.
    DataInputStream dataInputStream = new DataInputStream(input);
    String type = dataInputStream.readUTF();
    int version = dataInputStream.readInt();
    for (Deserializer deserializer : deserializers) {
      if (type.equals(deserializer.type) && deserializer.version >= version) {
        return deserializer.readFromStream(version, dataInputStream);
      }
    }
    throw new DownloadException("No deserializer found for:" + type + ", " + version);
  }
  /** Serializes {@code action} type and data into the {@code output}. */
  public static void serializeToStream(DownloadAction action, OutputStream output)
      throws IOException {
    // Don't close the stream as it closes the underlying stream too.
    DataOutputStream dataOutputStream = new DataOutputStream(output);
    dataOutputStream.writeUTF(action.type);
    dataOutputStream.writeInt(action.version);
    action.writeToStream(dataOutputStream);
    dataOutputStream.flush();
  }
  /** The type of the action. */
  public final String type;
  /** The action version. */
  public final int version;
  /** The uri being downloaded or removed. */
  public final Uri uri;
  /** Whether this is a remove action. If false, this is a download action. */
  public final boolean isRemoveAction;
  /** Custom data for this action. May be empty. */
  public final byte[] data;
  /**
   * @param type The type of the action.
   * @param version The action version.
   * @param uri The uri being downloaded or removed.
   * @param isRemoveAction Whether this is a remove action. If false, this is a download action.
   * @param data Optional custom data for this action.
   */
  protected DownloadAction(
      String type, int version, Uri uri, boolean isRemoveAction, @Nullable byte[] data) {
    this.type = type;
    this.version = version;
    this.uri = uri;
    this.isRemoveAction = isRemoveAction;
    this.data = data != null ? data : Util.EMPTY_BYTE_ARRAY;
  }
  /** Serializes itself into a byte array. */
  public final byte[] toByteArray() {
    ByteArrayOutputStream output = new ByteArrayOutputStream();
    try {
      serializeToStream(this, output);
    } catch (IOException e) {
      // ByteArrayOutputStream shouldn't throw IOException.
      throw new IllegalStateException();
    }
    return output.toByteArray();
  }
  /** Returns whether this is an action for the same media as the {@code other}. */
  public boolean isSameMedia(DownloadAction other) {
    return uri.equals(other.uri);
  }
  /** Returns keys of tracks to be downloaded. */
  public List<StreamKey> getKeys() {
    return Collections.emptyList();
  }
  /** Serializes itself into the {@code output}. */
  protected abstract void writeToStream(DataOutputStream output) throws IOException;
  /** Creates a {@link Downloader} with the given parameters. */
  public abstract Downloader createDownloader(
      DownloaderConstructorHelper downloaderConstructorHelper);
  @SuppressWarnings("EqualsGetClass")
  @Override
  public boolean equals(@Nullable Object o) {
    if (o == null || getClass() != o.getClass()) {
      return false;
    }
    DownloadAction that = (DownloadAction) o;
    return type.equals(that.type)
        && version == that.version
        && uri.equals(that.uri)
        && isRemoveAction == that.isRemoveAction
        && Arrays.equals(data, that.data);
  }
  @Override
  public int hashCode() {
    int result = uri.hashCode();
    result = 31 * result + (isRemoveAction ? 1 : 0);
    result = 31 * result + Arrays.hashCode(data);
    return result;
  }
  private static Deserializer getDeserializer(Class<?> clazz)
      throws NoSuchFieldException, IllegalAccessException {
    Object value = clazz.getDeclaredField("DESERIALIZER").get(null);
    return (Deserializer) Assertions.checkNotNull(value);
  }
}
```

## Downloader


```java

package com.google.android.exoplayer2.offline;
import com.google.android.exoplayer2.C;
import java.io.IOException;
/**
 * An interface for stream downloaders.
 */
public interface Downloader {
  /**
   * Downloads the media.
   *
   * @throws DownloadException Thrown if the media cannot be downloaded.
   * @throws InterruptedException If the thread has been interrupted.
   * @throws IOException Thrown when there is an io error while downloading.
   */
  void download() throws InterruptedException, IOException;
  /** Interrupts any current download operation and prevents future operations from running. */
  void cancel();
  /** Returns the total number of downloaded bytes. */
  long getDownloadedBytes();
  /**
   * Returns the estimated download percentage, or {@link C#PERCENTAGE_UNSET} if no estimate is
   * available.
   */
  float getDownloadPercentage();
  /**
   * Removes the media.
   *
   * @throws InterruptedException Thrown if the thread was interrupted.
   */
  void remove() throws InterruptedException;
}
```

## DownloaderConstructorHelper


```java

package com.google.android.exoplayer2.offline;
import android.support.annotation.Nullable;
import com.google.android.exoplayer2.C;
import com.google.android.exoplayer2.upstream.DataSink;
import com.google.android.exoplayer2.upstream.DataSource;
import com.google.android.exoplayer2.upstream.DataSource.Factory;
import com.google.android.exoplayer2.upstream.DummyDataSource;
import com.google.android.exoplayer2.upstream.FileDataSource;
import com.google.android.exoplayer2.upstream.PriorityDataSource;
import com.google.android.exoplayer2.upstream.cache.Cache;
import com.google.android.exoplayer2.upstream.cache.CacheDataSink;
import com.google.android.exoplayer2.upstream.cache.CacheDataSource;
import com.google.android.exoplayer2.util.Assertions;
import com.google.android.exoplayer2.util.PriorityTaskManager;
/** A helper class that holds necessary parameters for {@link Downloader} construction. */
public final class DownloaderConstructorHelper {
  private final Cache cache;
  private final Factory upstreamDataSourceFactory;
  private final Factory cacheReadDataSourceFactory;
  private final DataSink.Factory cacheWriteDataSinkFactory;
  private final PriorityTaskManager priorityTaskManager;
  /**
   * @param cache Cache instance to be used to store downloaded data.
   * @param upstreamDataSourceFactory A {@link Factory} for downloading data.
   */
  public DownloaderConstructorHelper(Cache cache, Factory upstreamDataSourceFactory) {
    this(cache, upstreamDataSourceFactory, null, null, null);
  }
  /**
   * @param cache Cache instance to be used to store downloaded data.
   * @param upstreamDataSourceFactory A {@link Factory} for downloading data.
   * @param cacheReadDataSourceFactory A {@link Factory} for reading data from the cache. If null
   *     then standard {@link FileDataSource} instances will be used.
   * @param cacheWriteDataSinkFactory A {@link DataSink.Factory} for writing data to the cache. If
   *     null then standard {@link CacheDataSink} instances will be used.
   * @param priorityTaskManager A {@link PriorityTaskManager} to use when downloading. If non-null,
   *     downloaders will register as tasks with priority {@link C#PRIORITY_DOWNLOAD} whilst
   *     downloading.
   */
  public DownloaderConstructorHelper(
      Cache cache,
      Factory upstreamDataSourceFactory,
      @Nullable Factory cacheReadDataSourceFactory,
      @Nullable DataSink.Factory cacheWriteDataSinkFactory,
      @Nullable PriorityTaskManager priorityTaskManager) {
    Assertions.checkNotNull(upstreamDataSourceFactory);
    this.cache = cache;
    this.upstreamDataSourceFactory = upstreamDataSourceFactory;
    this.cacheReadDataSourceFactory = cacheReadDataSourceFactory;
    this.cacheWriteDataSinkFactory = cacheWriteDataSinkFactory;
    this.priorityTaskManager = priorityTaskManager;
  }
  /** Returns the {@link Cache} instance. */
  public Cache getCache() {
    return cache;
  }
  /** Returns a {@link PriorityTaskManager} instance. */
  public PriorityTaskManager getPriorityTaskManager() {
    // Return a dummy PriorityTaskManager if none is provided. Create a new PriorityTaskManager
    // each time so clients don't affect each other over the dummy PriorityTaskManager instance.
    return priorityTaskManager != null ? priorityTaskManager : new PriorityTaskManager();
  }
  /**
   * Returns a new {@link CacheDataSource} instance. If {@code offline} is true, it can only read
   * data from the cache.
   */
  public CacheDataSource buildCacheDataSource(boolean offline) {
    DataSource cacheReadDataSource = cacheReadDataSourceFactory != null
        ? cacheReadDataSourceFactory.createDataSource() : new FileDataSource();
    if (offline) {
      return new CacheDataSource(cache, DummyDataSource.INSTANCE,
          cacheReadDataSource, null, CacheDataSource.FLAG_BLOCK_ON_CACHE, null);
    } else {
      DataSink cacheWriteDataSink = cacheWriteDataSinkFactory != null
          ? cacheWriteDataSinkFactory.createDataSink()
          : new CacheDataSink(cache, CacheDataSource.DEFAULT_MAX_CACHE_FILE_SIZE);
      DataSource upstream = upstreamDataSourceFactory.createDataSource();
      upstream = priorityTaskManager == null ? upstream
          : new PriorityDataSource(upstream, priorityTaskManager, C.PRIORITY_DOWNLOAD);
      return new CacheDataSource(cache, upstream, cacheReadDataSource,
          cacheWriteDataSink, CacheDataSource.FLAG_BLOCK_ON_CACHE, null);
    }
  }
}
```

## DownloadException


```java

package com.google.android.exoplayer2.offline;
import java.io.IOException;
/** Thrown on an error during downloading. */
public final class DownloadException extends IOException {
  /** @param message The message for the exception. */
  public DownloadException(String message) {
    super(message);
  }
  /** @param cause The cause for the exception. */
  public DownloadException(Throwable cause) {
    super(cause);
  }
}
```

## DownloadHelper


```java

package com.google.android.exoplayer2.offline;
import android.os.Handler;
import android.os.Looper;
import android.support.annotation.Nullable;
import com.google.android.exoplayer2.source.TrackGroupArray;
import java.io.IOException;
import java.util.List;
/** A helper for initializing and removing downloads. */
public abstract class DownloadHelper {
  /** A callback to be notified when the {@link DownloadHelper} is prepared. */
  public interface Callback {
    /**
     * Called when preparation completes.
     *
     * @param helper The reporting {@link DownloadHelper}.
     */
    void onPrepared(DownloadHelper helper);
    /**
     * Called when preparation fails.
     *
     * @param helper The reporting {@link DownloadHelper}.
     * @param e The error.
     */
    void onPrepareError(DownloadHelper helper, IOException e);
  }
  /**
   * Initializes the helper for starting a download.
   *
   * @param callback A callback to be notified when preparation completes or fails. The callback
   *     will be invoked on the calling thread unless that thread does not have an associated {@link
   *     Looper}, in which case it will be called on the application's main thread.
   */
  public void prepare(final Callback callback) {
    final Handler handler =
        new Handler(Looper.myLooper() != null ? Looper.myLooper() : Looper.getMainLooper());
    new Thread() {
      @Override
      public void run() {
        try {
          prepareInternal();
          handler.post(() -> callback.onPrepared(DownloadHelper.this));
        } catch (final IOException e) {
          handler.post(() -> callback.onPrepareError(DownloadHelper.this, e));
        }
      }
    }.start();
  }
  /**
   * Called on a background thread during preparation.
   *
   * @throws IOException If preparation fails.
   */
  protected abstract void prepareInternal() throws IOException;
  /**
   * Returns the number of periods for which media is available. Must not be called until after
   * preparation completes.
   */
  public abstract int getPeriodCount();
  /**
   * Returns the track groups for the given period. Must not be called until after preparation
   * completes.
   *
   * @param periodIndex The period index.
   * @return The track groups for the period. May be {@link TrackGroupArray#EMPTY} for single stream
   *     content.
   */
  public abstract TrackGroupArray getTrackGroups(int periodIndex);
  /**
   * Builds a {@link DownloadAction} for downloading the specified tracks. Must not be called until
   * after preparation completes.
   *
   * @param data Application provided data to store in {@link DownloadAction#data}.
   * @param trackKeys The selected tracks. If empty, all streams will be downloaded.
   * @return The built {@link DownloadAction}.
   */
  public abstract DownloadAction getDownloadAction(@Nullable byte[] data, List<TrackKey> trackKeys);
  /**
   * Builds a {@link DownloadAction} for removing the media. May be called in any state.
   *
   * @param data Application provided data to store in {@link DownloadAction#data}.
   * @return The built {@link DownloadAction}.
   */
  public abstract DownloadAction getRemoveAction(@Nullable byte[] data);
}
```

## DownloadManager


```java

package com.google.android.exoplayer2.offline;
import static com.google.android.exoplayer2.offline.DownloadManager.TaskState.STATE_CANCELED;
import static com.google.android.exoplayer2.offline.DownloadManager.TaskState.STATE_COMPLETED;
import static com.google.android.exoplayer2.offline.DownloadManager.TaskState.STATE_FAILED;
import static com.google.android.exoplayer2.offline.DownloadManager.TaskState.STATE_QUEUED;
import static com.google.android.exoplayer2.offline.DownloadManager.TaskState.STATE_STARTED;
import android.os.ConditionVariable;
import android.os.Handler;
import android.os.HandlerThread;
import android.os.Looper;
import android.support.annotation.IntDef;
import android.support.annotation.Nullable;
import com.google.android.exoplayer2.C;
import com.google.android.exoplayer2.offline.DownloadAction.Deserializer;
import com.google.android.exoplayer2.upstream.DataSource;
import com.google.android.exoplayer2.upstream.cache.Cache;
import com.google.android.exoplayer2.util.Assertions;
import com.google.android.exoplayer2.util.Log;
import com.google.android.exoplayer2.util.Util;
import java.io.ByteArrayInputStream;
import java.io.File;
import java.io.IOException;
import java.lang.annotation.Documented;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.CopyOnWriteArraySet;
/**
 * Manages multiple stream download and remove requests.
 *
 * <p>A download manager instance must be accessed only from the thread that created it, unless that
 * thread does not have a {@link Looper}. In that case, it must be accessed only from the
 * application's main thread. Registered listeners will be called on the same thread.
 */
public final class DownloadManager {
  /** Listener for {@link DownloadManager} events. */
  public interface Listener {
    /**
     * Called when all actions have been restored.
     *
     * @param downloadManager The reporting instance.
     */
    void onInitialized(DownloadManager downloadManager);
    /**
     * Called when the state of a task changes.
     *
     * @param downloadManager The reporting instance.
     * @param taskState The state of the task.
     */
    void onTaskStateChanged(DownloadManager downloadManager, TaskState taskState);
    /**
     * Called when there is no active task left.
     *
     * @param downloadManager The reporting instance.
     */
    void onIdle(DownloadManager downloadManager);
  }
  /** The default maximum number of simultaneous download tasks. */
  public static final int DEFAULT_MAX_SIMULTANEOUS_DOWNLOADS = 1;
  /** The default minimum number of times a task must be retried before failing. */
  public static final int DEFAULT_MIN_RETRY_COUNT = 5;
  private static final String TAG = "DownloadManager";
  private static final boolean DEBUG = false;
  private final DownloaderConstructorHelper downloaderConstructorHelper;
  private final int maxActiveDownloadTasks;
  private final int minRetryCount;
  private final ActionFile actionFile;
  private final DownloadAction.Deserializer[] deserializers;
  private final ArrayList<Task> tasks;
  private final ArrayList<Task> activeDownloadTasks;
  private final Handler handler;
  private final HandlerThread fileIOThread;
  private final Handler fileIOHandler;
  private final CopyOnWriteArraySet<Listener> listeners;
  private int nextTaskId;
  private boolean initialized;
  private boolean released;
  private boolean downloadsStopped;
  /**
   * Creates a {@link DownloadManager}.
   *
   * @param cache Cache instance to be used to store downloaded data.
   * @param upstreamDataSourceFactory A {@link DataSource.Factory} for creating data sources for
   *     downloading upstream data.
   * @param actionSaveFile File to save active actions.
   * @param deserializers Used to deserialize {@link DownloadAction}s. If empty, {@link
   *     DownloadAction#getDefaultDeserializers()} is used instead.
   */
  public DownloadManager(
      Cache cache,
      DataSource.Factory upstreamDataSourceFactory,
      File actionSaveFile,
      Deserializer... deserializers) {
    this(
        new DownloaderConstructorHelper(cache, upstreamDataSourceFactory),
        actionSaveFile,
        deserializers);
  }
  /**
   * Constructs a {@link DownloadManager}.
   *
   * @param constructorHelper A {@link DownloaderConstructorHelper} to create {@link Downloader}s
   *     for downloading data.
   * @param actionFile The file in which active actions are saved.
   * @param deserializers Used to deserialize {@link DownloadAction}s. If empty, {@link
   *     DownloadAction#getDefaultDeserializers()} is used instead.
   */
  public DownloadManager(
      DownloaderConstructorHelper constructorHelper,
      File actionFile,
      Deserializer... deserializers) {
    this(
        constructorHelper,
        DEFAULT_MAX_SIMULTANEOUS_DOWNLOADS,
        DEFAULT_MIN_RETRY_COUNT,
        actionFile,
        deserializers);
  }
  /**
   * Constructs a {@link DownloadManager}.
   *
   * @param constructorHelper A {@link DownloaderConstructorHelper} to create {@link Downloader}s
   *     for downloading data.
   * @param maxSimultaneousDownloads The maximum number of simultaneous download tasks.
   * @param minRetryCount The minimum number of times a task must be retried before failing.
   * @param actionFile The file in which active actions are saved.
   * @param deserializers Used to deserialize {@link DownloadAction}s. If empty, {@link
   *     DownloadAction#getDefaultDeserializers()} is used instead.
   */
  public DownloadManager(
      DownloaderConstructorHelper constructorHelper,
      int maxSimultaneousDownloads,
      int minRetryCount,
      File actionFile,
      Deserializer... deserializers) {
    this.downloaderConstructorHelper = constructorHelper;
    this.maxActiveDownloadTasks = maxSimultaneousDownloads;
    this.minRetryCount = minRetryCount;
    this.actionFile = new ActionFile(actionFile);
    this.deserializers =
        deserializers.length > 0 ? deserializers : DownloadAction.getDefaultDeserializers();
    this.downloadsStopped = true;
    tasks = new ArrayList<>();
    activeDownloadTasks = new ArrayList<>();
    Looper looper = Looper.myLooper();
    if (looper == null) {
      looper = Looper.getMainLooper();
    }
    handler = new Handler(looper);
    fileIOThread = new HandlerThread("DownloadManager file i/o");
    fileIOThread.start();
    fileIOHandler = new Handler(fileIOThread.getLooper());
    listeners = new CopyOnWriteArraySet<>();
    loadActions();
    logd("Created");
  }
  /**
   * Adds a {@link Listener}.
   *
   * @param listener The listener to be added.
   */
  public void addListener(Listener listener) {
    listeners.add(listener);
  }
  /**
   * Removes a {@link Listener}.
   *
   * @param listener The listener to be removed.
   */
  public void removeListener(Listener listener) {
    listeners.remove(listener);
  }
  /** Starts the download tasks. */
  public void startDownloads() {
    Assertions.checkState(!released);
    if (downloadsStopped) {
      downloadsStopped = false;
      maybeStartTasks();
      logd("Downloads are started");
    }
  }
  /** Stops all of the download tasks. Call {@link #startDownloads()} to restart tasks. */
  public void stopDownloads() {
    Assertions.checkState(!released);
    if (!downloadsStopped) {
      downloadsStopped = true;
      for (int i = 0; i < activeDownloadTasks.size(); i++) {
        activeDownloadTasks.get(i).stop();
      }
      logd("Downloads are stopping");
    }
  }
  /**
   * Deserializes an action from {@code actionData}, and calls {@link
   * #handleAction(DownloadAction)}.
   *
   * @param actionData Serialized version of the action to be executed.
   * @return The id of the newly created task.
   * @throws IOException If an error occurs deserializing the action.
   */
  public int handleAction(byte[] actionData) throws IOException {
    Assertions.checkState(!released);
    ByteArrayInputStream input = new ByteArrayInputStream(actionData);
    DownloadAction action = DownloadAction.deserializeFromStream(deserializers, input);
    return handleAction(action);
  }
  /**
   * Handles the given action. A task is created and added to the task queue. If it's a remove
   * action then any download tasks for the same media are immediately canceled.
   *
   * @param action The action to be executed.
   * @return The id of the newly created task.
   */
  public int handleAction(DownloadAction action) {
    Assertions.checkState(!released);
    Task task = addTaskForAction(action);
    if (initialized) {
      saveActions();
      maybeStartTasks();
      if (task.currentState == STATE_QUEUED) {
        // Task did not change out of its initial state, and so its initial state won't have been
        // reported to listeners. Do so now.
        notifyListenersTaskStateChange(task);
      }
    }
    return task.id;
  }
  /** Returns the number of tasks. */
  public int getTaskCount() {
    Assertions.checkState(!released);
    return tasks.size();
  }
  /** Returns the number of download tasks. */
  public int getDownloadCount() {
    int count = 0;
    for (int i = 0; i < tasks.size(); i++) {
      if (!tasks.get(i).action.isRemoveAction) {
        count++;
      }
    }
    return count;
  }
  /** Returns the state of a task, or null if no such task exists */
  public @Nullable TaskState getTaskState(int taskId) {
    Assertions.checkState(!released);
    for (int i = 0; i < tasks.size(); i++) {
      Task task = tasks.get(i);
      if (task.id == taskId) {
        return task.getDownloadState();
      }
    }
    return null;
  }
  /** Returns the states of all current tasks. */
  public TaskState[] getAllTaskStates() {
    Assertions.checkState(!released);
    TaskState[] states = new TaskState[tasks.size()];
    for (int i = 0; i < states.length; i++) {
      states[i] = tasks.get(i).getDownloadState();
    }
    return states;
  }
  /** Returns whether the manager has completed initialization. */
  public boolean isInitialized() {
    Assertions.checkState(!released);
    return initialized;
  }
  /** Returns whether there are no active tasks. */
  public boolean isIdle() {
    Assertions.checkState(!released);
    if (!initialized) {
      return false;
    }
    for (int i = 0; i < tasks.size(); i++) {
      if (tasks.get(i).isActive()) {
        return false;
      }
    }
    return true;
  }
  /**
   * Stops all of the tasks and releases resources. If the action file isn't up to date, waits for
   * the changes to be written. The manager must not be accessed after this method has been called.
   */
  public void release() {
    if (released) {
      return;
    }
    released = true;
    for (int i = 0; i < tasks.size(); i++) {
      tasks.get(i).stop();
    }
    final ConditionVariable fileIOFinishedCondition = new ConditionVariable();
    fileIOHandler.post(fileIOFinishedCondition::open);
    fileIOFinishedCondition.block();
    fileIOThread.quit();
    logd("Released");
  }
  private Task addTaskForAction(DownloadAction action) {
    Task task = new Task(nextTaskId++, this, action, minRetryCount);
    tasks.add(task);
    logd("Task is added", task);
    return task;
  }
  /**
   * Iterates through the task queue and starts any task if all of the following are true:
   *
   * <ul>
   *   <li>It hasn't started yet.
   *   <li>There are no preceding conflicting tasks.
   *   <li>If it's a download task then there are no preceding download tasks on hold and the
   *       maximum number of active downloads hasn't been reached.
   * </ul>
   *
   * If the task is a remove action then preceding conflicting tasks are canceled.
   */
  private void maybeStartTasks() {
    if (!initialized || released) {
      return;
    }
    boolean skipDownloadActions = downloadsStopped
        || activeDownloadTasks.size() == maxActiveDownloadTasks;
    for (int i = 0; i < tasks.size(); i++) {
      Task task = tasks.get(i);
      if (!task.canStart()) {
        continue;
      }
      DownloadAction action = task.action;
      boolean isRemoveAction = action.isRemoveAction;
      if (!isRemoveAction && skipDownloadActions) {
        continue;
      }
      boolean canStartTask = true;
      for (int j = 0; j < i; j++) {
        Task otherTask = tasks.get(j);
        if (otherTask.action.isSameMedia(action)) {
          if (isRemoveAction) {
            canStartTask = false;
            logd(task + " clashes with " + otherTask);
            otherTask.cancel();
            // Continue loop to cancel any other preceding clashing tasks.
          } else if (otherTask.action.isRemoveAction) {
            canStartTask = false;
            skipDownloadActions = true;
            break;
          }
        }
      }
      if (canStartTask) {
        task.start();
        if (!isRemoveAction) {
          activeDownloadTasks.add(task);
          skipDownloadActions = activeDownloadTasks.size() == maxActiveDownloadTasks;
        }
      }
    }
  }
  private void maybeNotifyListenersIdle() {
    if (!isIdle()) {
      return;
    }
    logd("Notify idle state");
    for (Listener listener : listeners) {
      listener.onIdle(this);
    }
  }
  private void onTaskStateChange(Task task) {
    if (released) {
      return;
    }
    boolean stopped = !task.isActive();
    if (stopped) {
      activeDownloadTasks.remove(task);
    }
    notifyListenersTaskStateChange(task);
    if (task.isFinished()) {
      tasks.remove(task);
      saveActions();
    }
    if (stopped) {
      maybeStartTasks();
      maybeNotifyListenersIdle();
    }
  }
  private void notifyListenersTaskStateChange(Task task) {
    logd("Task state is changed", task);
    TaskState taskState = task.getDownloadState();
    for (Listener listener : listeners) {
      listener.onTaskStateChanged(this, taskState);
    }
  }
  private void loadActions() {
    fileIOHandler.post(
        () -> {
          DownloadAction[] loadedActions;
          try {
            loadedActions = actionFile.load(DownloadManager.this.deserializers);
            logd("Action file is loaded.");
          } catch (Throwable e) {
            Log.e(TAG, "Action file loading failed.", e);
            loadedActions = new DownloadAction[0];
          }
          final DownloadAction[] actions = loadedActions;
          handler.post(
              () -> {
                if (released) {
                  return;
                }
                List<Task> pendingTasks = new ArrayList<>(tasks);
                tasks.clear();
                for (DownloadAction action : actions) {
                  addTaskForAction(action);
                }
                logd("Tasks are created.");
                initialized = true;
                for (Listener listener : listeners) {
                  listener.onInitialized(DownloadManager.this);
                }
                if (!pendingTasks.isEmpty()) {
                  tasks.addAll(pendingTasks);
                  saveActions();
                }
                maybeStartTasks();
                for (int i = 0; i < tasks.size(); i++) {
                  Task task = tasks.get(i);
                  if (task.currentState == STATE_QUEUED) {
                    // Task did not change out of its initial state, and so its initial state
                    // won't have been reported to listeners. Do so now.
                    notifyListenersTaskStateChange(task);
                  }
                }
              });
        });
  }
  private void saveActions() {
    if (released) {
      return;
    }
    final DownloadAction[] actions = new DownloadAction[tasks.size()];
    for (int i = 0; i < tasks.size(); i++) {
      actions[i] = tasks.get(i).action;
    }
    fileIOHandler.post(
        () -> {
          try {
            actionFile.store(actions);
            logd("Actions persisted.");
          } catch (IOException e) {
            Log.e(TAG, "Persisting actions failed.", e);
          }
        });
  }
  private static void logd(String message) {
    if (DEBUG) {
      Log.d(TAG, message);
    }
  }
  private static void logd(String message, Task task) {
    logd(message + ": " + task);
  }
  /** Represents state of a task. */
  public static final class TaskState {
    /**
     * Task states. One of {@link #STATE_QUEUED}, {@link #STATE_STARTED}, {@link #STATE_COMPLETED},
     * {@link #STATE_CANCELED} or {@link #STATE_FAILED}.
     *
     * <p>Transition diagram:
     *
     * <pre>
     *                    -&gt; canceled
     * queued &lt;-&gt; started -&gt; completed
     *                    -&gt; failed
     * </pre>
     */
    @Documented
    @Retention(RetentionPolicy.SOURCE)
    @IntDef({STATE_QUEUED, STATE_STARTED, STATE_COMPLETED, STATE_CANCELED, STATE_FAILED})
    public @interface State {}
    /** The task is waiting to be started. */
    public static final int STATE_QUEUED = 0;
    /** The task is currently started. */
    public static final int STATE_STARTED = 1;
    /** The task completed. */
    public static final int STATE_COMPLETED = 2;
    /** The task was canceled. */
    public static final int STATE_CANCELED = 3;
    /** The task failed. */
    public static final int STATE_FAILED = 4;
    /** Returns the state string for the given state value. */
    public static String getStateString(@State int state) {
      switch (state) {
        case STATE_QUEUED:
          return "QUEUED";
        case STATE_STARTED:
          return "STARTED";
        case STATE_COMPLETED:
          return "COMPLETED";
        case STATE_CANCELED:
          return "CANCELED";
        case STATE_FAILED:
          return "FAILED";
        default:
          throw new IllegalStateException();
      }
    }
    /** The unique task id. */
    public final int taskId;
    /** The action being executed. */
    public final DownloadAction action;
    /** The state of the task. */
    public final @State int state;
    /**
     * The estimated download percentage, or {@link C#PERCENTAGE_UNSET} if no estimate is available
     * or if this is a removal task.
     */
    public final float downloadPercentage;
    /** The total number of downloaded bytes. */
    public final long downloadedBytes;
    /** If {@link #state} is {@link #STATE_FAILED} then this is the cause, otherwise null. */
    public final Throwable error;
    private TaskState(
        int taskId,
        DownloadAction action,
        @State int state,
        float downloadPercentage,
        long downloadedBytes,
        Throwable error) {
      this.taskId = taskId;
      this.action = action;
      this.state = state;
      this.downloadPercentage = downloadPercentage;
      this.downloadedBytes = downloadedBytes;
      this.error = error;
    }
  }
  private static final class Task implements Runnable {
    /**
     * Task states. One of {@link TaskState#STATE_QUEUED}, {@link TaskState#STATE_STARTED}, {@link
     * TaskState#STATE_COMPLETED}, {@link TaskState#STATE_CANCELED}, {@link TaskState#STATE_FAILED},
     * {@link #STATE_QUEUED_CANCELING}, {@link #STATE_STARTED_CANCELING} or {@link
     * #STATE_STARTED_STOPPING}.
     *
     * <p>Transition map (vertical states are source states):
     *
     * <pre>
     *             +------+-------+---------+-----------+-----------+--------+--------+------+
     *             |queued|started|completed|q_canceling|s_canceling|canceled|stopping|failed|
     * +-----------+------+-------+---------+-----------+-----------+--------+--------+------+
     * |queued     |      |   X   |         |     X     |           |        |        |      |
     * |started    |      |       |    X    |           |     X     |        |   X    |   X  |
     * |q_canceling|      |       |         |           |           |   X    |        |      |
     * |s_canceling|      |       |         |           |           |   X    |        |      |
     * |stopping   |   X  |       |         |           |           |        |        |      |
     * +-----------+------+-------+---------+-----------+-----------+--------+--------+------+
     * </pre>
     */
    @Documented
    @Retention(RetentionPolicy.SOURCE)
    @IntDef({
      STATE_QUEUED,
      STATE_STARTED,
      STATE_COMPLETED,
      STATE_CANCELED,
      STATE_FAILED,
      STATE_QUEUED_CANCELING,
      STATE_STARTED_CANCELING,
      STATE_STARTED_STOPPING
    })
    public @interface InternalState {}
    /** The task is about to be canceled. */
    public static final int STATE_QUEUED_CANCELING = 5;
    /** The task is about to be canceled. */
    public static final int STATE_STARTED_CANCELING = 6;
    /** The task is about to be stopped. */
    public static final int STATE_STARTED_STOPPING = 7;
    private final int id;
    private final DownloadManager downloadManager;
    private final DownloadAction action;
    private final int minRetryCount;
    private volatile @InternalState int currentState;
    private volatile Downloader downloader;
    private Thread thread;
    private Throwable error;
    private Task(
        int id, DownloadManager downloadManager, DownloadAction action, int minRetryCount) {
      this.id = id;
      this.downloadManager = downloadManager;
      this.action = action;
      this.currentState = STATE_QUEUED;
      this.minRetryCount = minRetryCount;
    }
    public TaskState getDownloadState() {
      int externalState = getExternalState();
      return new TaskState(
          id, action, externalState, getDownloadPercentage(), getDownloadedBytes(), error);
    }
    /** Returns whether the task is finished. */
    public boolean isFinished() {
      return currentState == STATE_FAILED
          || currentState == STATE_COMPLETED
          || currentState == STATE_CANCELED;
    }
    /** Returns whether the task is started. */
    public boolean isActive() {
      return currentState == STATE_QUEUED_CANCELING
          || currentState == STATE_STARTED
          || currentState == STATE_STARTED_STOPPING
          || currentState == STATE_STARTED_CANCELING;
    }
    /**
     * Returns the estimated download percentage, or {@link C#PERCENTAGE_UNSET} if no estimate is
     * available.
     */
    public float getDownloadPercentage() {
      return downloader != null ? downloader.getDownloadPercentage() : C.PERCENTAGE_UNSET;
    }
    /** Returns the total number of downloaded bytes. */
    public long getDownloadedBytes() {
      return downloader != null ? downloader.getDownloadedBytes() : 0;
    }
    @Override
    public String toString() {
      if (!DEBUG) {
        return super.toString();
      }
      return action.type
          + ' '
          + (action.isRemoveAction ? "remove" : "download")
          + ' '
          + toString(action.data)
          + ' '
          + getStateString();
    }
    private static String toString(byte[] data) {
      if (data.length > 100) {
        return "<data is too long>";
      } else {
        return '\'' + Util.fromUtf8Bytes(data) + '\'';
      }
    }
    private String getStateString() {
      switch (currentState) {
        case STATE_QUEUED_CANCELING:
        case STATE_STARTED_CANCELING:
          return "CANCELING";
        case STATE_STARTED_STOPPING:
          return "STOPPING";
        case STATE_QUEUED:
        case STATE_STARTED:
        case STATE_COMPLETED:
        case STATE_CANCELED:
        case STATE_FAILED:
        default:
          return TaskState.getStateString(currentState);
      }
    }
    private int getExternalState() {
      switch (currentState) {
        case STATE_QUEUED_CANCELING:
          return STATE_QUEUED;
        case STATE_STARTED_CANCELING:
        case STATE_STARTED_STOPPING:
          return STATE_STARTED;
        case STATE_QUEUED:
        case STATE_STARTED:
        case STATE_COMPLETED:
        case STATE_CANCELED:
        case STATE_FAILED:
        default:
          return currentState;
      }
    }
    private void start() {
      if (changeStateAndNotify(STATE_QUEUED, STATE_STARTED)) {
        thread = new Thread(this);
        thread.start();
      }
    }
    private boolean canStart() {
      return currentState == STATE_QUEUED;
    }
    private void cancel() {
      if (changeStateAndNotify(STATE_QUEUED, STATE_QUEUED_CANCELING)) {
        downloadManager.handler.post(
            () -> changeStateAndNotify(STATE_QUEUED_CANCELING, STATE_CANCELED));
      } else if (changeStateAndNotify(STATE_STARTED, STATE_STARTED_CANCELING)) {
        cancelDownload();
      }
    }
    private void stop() {
      if (changeStateAndNotify(STATE_STARTED, STATE_STARTED_STOPPING)) {
        logd("Stopping", this);
        cancelDownload();
      }
    }
    private boolean changeStateAndNotify(@InternalState int oldState, @InternalState int newState) {
      return changeStateAndNotify(oldState, newState, null);
    }
    private boolean changeStateAndNotify(
        @InternalState int oldState, @InternalState int newState, Throwable error) {
      if (currentState != oldState) {
        return false;
      }
      currentState = newState;
      this.error = error;
      boolean isInternalState = currentState != getExternalState();
      if (!isInternalState) {
        downloadManager.onTaskStateChange(this);
      }
      return true;
    }
    private void cancelDownload() {
      if (downloader != null) {
        downloader.cancel();
      }
      thread.interrupt();
    }
    // Methods running on download thread.
    @Override
    public void run() {
      logd("Task is started", this);
      Throwable error = null;
      try {
        downloader = action.createDownloader(downloadManager.downloaderConstructorHelper);
        if (action.isRemoveAction) {
          downloader.remove();
        } else {
          int errorCount = 0;
          long errorPosition = C.LENGTH_UNSET;
          while (!Thread.interrupted()) {
            try {
              downloader.download();
              break;
            } catch (IOException e) {
              long downloadedBytes = downloader.getDownloadedBytes();
              if (downloadedBytes != errorPosition) {
                logd("Reset error count. downloadedBytes = " + downloadedBytes, this);
                errorPosition = downloadedBytes;
                errorCount = 0;
              }
              if (currentState != STATE_STARTED || ++errorCount > minRetryCount) {
                throw e;
              }
              logd("Download error. Retry " + errorCount, this);
              Thread.sleep(getRetryDelayMillis(errorCount));
            }
          }
        }
      } catch (Throwable e){
        error = e;
      }
      final Throwable finalError = error;
      downloadManager.handler.post(
          () -> {
            if (changeStateAndNotify(
                    STATE_STARTED, finalError != null ? STATE_FAILED : STATE_COMPLETED, finalError)
                || changeStateAndNotify(STATE_STARTED_CANCELING, STATE_CANCELED)
                || changeStateAndNotify(STATE_STARTED_STOPPING, STATE_QUEUED)) {
              return;
            }
            throw new IllegalStateException();
          });
    }
    private int getRetryDelayMillis(int errorCount) {
      return Math.min((errorCount - 1) * 1000, 5000);
    }
  }
}
```

## DownloadService


```java

package com.google.android.exoplayer2.offline;
import android.app.Notification;
import android.app.Service;
import android.content.Context;
import android.content.Intent;
import android.os.Handler;
import android.os.IBinder;
import android.os.Looper;
import android.support.annotation.Nullable;
import android.support.annotation.StringRes;
import com.google.android.exoplayer2.offline.DownloadManager.TaskState;
import com.google.android.exoplayer2.scheduler.Requirements;
import com.google.android.exoplayer2.scheduler.RequirementsWatcher;
import com.google.android.exoplayer2.scheduler.Scheduler;
import com.google.android.exoplayer2.util.Log;
import com.google.android.exoplayer2.util.NotificationUtil;
import com.google.android.exoplayer2.util.Util;
import java.io.IOException;
import java.util.HashMap;
/** A {@link Service} for downloading media. */
public abstract class DownloadService extends Service {
  /** Starts a download service without adding a new {@link DownloadAction}. */
  public static final String ACTION_INIT =
      "com.google.android.exoplayer.downloadService.action.INIT";
  /** Starts a download service, adding a new {@link DownloadAction} to be executed. */
  public static final String ACTION_ADD = "com.google.android.exoplayer.downloadService.action.ADD";
  /** Reloads the download requirements. */
  public static final String ACTION_RELOAD_REQUIREMENTS =
      "com.google.android.exoplayer.downloadService.action.RELOAD_REQUIREMENTS";
  /** Like {@link #ACTION_INIT}, but with {@link #KEY_FOREGROUND} implicitly set to true. */
  private static final String ACTION_RESTART =
      "com.google.android.exoplayer.downloadService.action.RESTART";
  /** Key for the {@link DownloadAction} in an {@link #ACTION_ADD} intent. */
  public static final String KEY_DOWNLOAD_ACTION = "download_action";
  /** Invalid foreground notification id which can be used to run the service in the background. */
  public static final int FOREGROUND_NOTIFICATION_ID_NONE = 0;
  /**
   * Key for a boolean flag in any intent to indicate whether the service was started in the
   * foreground. If set, the service is guaranteed to call {@link #startForeground(int,
   * Notification)}.
   */
  public static final String KEY_FOREGROUND = "foreground";
  /** Default foreground notification update interval in milliseconds. */
  public static final long DEFAULT_FOREGROUND_NOTIFICATION_UPDATE_INTERVAL = 1000;
  private static final String TAG = "DownloadService";
  private static final boolean DEBUG = false;
  // Keep the requirements helper for each DownloadService as long as there are tasks (and the
  // process is running). This allows tasks to resume when there's no scheduler. It may also allow
  // tasks the resume more quickly than when relying on the scheduler alone.
  private static final HashMap<Class<? extends DownloadService>, RequirementsHelper>
      requirementsHelpers = new HashMap<>();
  private static final Requirements DEFAULT_REQUIREMENTS =
      new Requirements(Requirements.NETWORK_TYPE_ANY, false, false);
  private final @Nullable ForegroundNotificationUpdater foregroundNotificationUpdater;
  private final @Nullable String channelId;
  private final @StringRes int channelName;
  private DownloadManager downloadManager;
  private DownloadManagerListener downloadManagerListener;
  private int lastStartId;
  private boolean startedInForeground;
  private boolean taskRemoved;
  /**
   * Creates a DownloadService.
   *
   * <p>If {@code foregroundNotificationId} is {@link #FOREGROUND_NOTIFICATION_ID_NONE} (value
   * {@value #FOREGROUND_NOTIFICATION_ID_NONE}) then the service runs in the background. No
   * foreground notification is displayed and {@link #getScheduler()} isn't called.
   *
   * <p>If {@code foregroundNotificationId} isn't {@link #FOREGROUND_NOTIFICATION_ID_NONE} (value
   * {@value #FOREGROUND_NOTIFICATION_ID_NONE}) the service runs in the foreground with {@link
   * #DEFAULT_FOREGROUND_NOTIFICATION_UPDATE_INTERVAL}. In that case {@link
   * #getForegroundNotification(TaskState[])} should be overridden in the subclass.
   *
   * @param foregroundNotificationId The notification id for the foreground notification, or {@link
   *     #FOREGROUND_NOTIFICATION_ID_NONE} (value {@value #FOREGROUND_NOTIFICATION_ID_NONE})
   */
  protected DownloadService(int foregroundNotificationId) {
    this(foregroundNotificationId, DEFAULT_FOREGROUND_NOTIFICATION_UPDATE_INTERVAL);
  }
  /**
   * Creates a DownloadService which will run in the foreground. {@link
   * #getForegroundNotification(TaskState[])} should be overridden in the subclass.
   *
   * @param foregroundNotificationId The notification id for the foreground notification, must not
   *     be 0.
   * @param foregroundNotificationUpdateInterval The maximum interval to update foreground
   *     notification, in milliseconds.
   */
  protected DownloadService(
      int foregroundNotificationId, long foregroundNotificationUpdateInterval) {
    this(
        foregroundNotificationId,
        foregroundNotificationUpdateInterval,
        /* channelId= */ null,
        /* channelName= */ 0);
  }
  /**
   * Creates a DownloadService which will run in the foreground. {@link
   * #getForegroundNotification(TaskState[])} should be overridden in the subclass.
   *
   * @param foregroundNotificationId The notification id for the foreground notification. Must not
   *     be 0.
   * @param foregroundNotificationUpdateInterval The maximum interval between updates to the
   *     foreground notification, in milliseconds.
   * @param channelId An id for a low priority notification channel to create, or {@code null} if
   *     the app will take care of creating a notification channel if needed. If specified, must be
   *     unique per package and the value may be truncated if it is too long.
   * @param channelName A string resource identifier for the user visible name of the channel, if
   *     {@code channelId} is specified. The recommended maximum length is 40 characters; the value
   *     may be truncated if it is too long.
   */
  protected DownloadService(
      int foregroundNotificationId,
      long foregroundNotificationUpdateInterval,
      @Nullable String channelId,
      @StringRes int channelName) {
    foregroundNotificationUpdater =
        foregroundNotificationId == 0
            ? null
            : new ForegroundNotificationUpdater(
                foregroundNotificationId, foregroundNotificationUpdateInterval);
    this.channelId = channelId;
    this.channelName = channelName;
  }
  /**
   * Builds an {@link Intent} for adding an action to be executed by the service.
   *
   * @param context A {@link Context}.
   * @param clazz The concrete download service being targeted by the intent.
   * @param downloadAction The action to be executed.
   * @param foreground Whether this intent will be used to start the service in the foreground.
   * @return Created Intent.
   */
  public static Intent buildAddActionIntent(
      Context context,
      Class<? extends DownloadService> clazz,
      DownloadAction downloadAction,
      boolean foreground) {
    return getIntent(context, clazz, ACTION_ADD)
        .putExtra(KEY_DOWNLOAD_ACTION, downloadAction.toByteArray())
        .putExtra(KEY_FOREGROUND, foreground);
  }
  /**
   * Starts the service, adding an action to be executed.
   *
   * @param context A {@link Context}.
   * @param clazz The concrete download service to be started.
   * @param downloadAction The action to be executed.
   * @param foreground Whether the service is started in the foreground.
   */
  public static void startWithAction(
      Context context,
      Class<? extends DownloadService> clazz,
      DownloadAction downloadAction,
      boolean foreground) {
    Intent intent = buildAddActionIntent(context, clazz, downloadAction, foreground);
    if (foreground) {
      Util.startForegroundService(context, intent);
    } else {
      context.startService(intent);
    }
  }
  /**
   * Starts the service without adding a new action. If there are any not finished actions and the
   * requirements are met, the service resumes executing actions. Otherwise it stops immediately.
   *
   * @param context A {@link Context}.
   * @param clazz The concrete download service to be started.
   * @see #startForeground(Context, Class)
   */
  public static void start(Context context, Class<? extends DownloadService> clazz) {
    context.startService(getIntent(context, clazz, ACTION_INIT));
  }
  /**
   * Starts the service in the foreground without adding a new action. If there are any not finished
   * actions and the requirements are met, the service resumes executing actions. Otherwise it stops
   * immediately.
   *
   * @param context A {@link Context}.
   * @param clazz The concrete download service to be started.
   * @see #start(Context, Class)
   */
  public static void startForeground(Context context, Class<? extends DownloadService> clazz) {
    Intent intent = getIntent(context, clazz, ACTION_INIT).putExtra(KEY_FOREGROUND, true);
    Util.startForegroundService(context, intent);
  }
  @Override
  public void onCreate() {
    logd("onCreate");
    if (channelId != null) {
      NotificationUtil.createNotificationChannel(
          this, channelId, channelName, NotificationUtil.IMPORTANCE_LOW);
    }
    downloadManager = getDownloadManager();
    downloadManagerListener = new DownloadManagerListener();
    downloadManager.addListener(downloadManagerListener);
  }
  @Override
  public int onStartCommand(Intent intent, int flags, int startId) {
    lastStartId = startId;
    taskRemoved = false;
    String intentAction = null;
    if (intent != null) {
      intentAction = intent.getAction();
      startedInForeground |=
          intent.getBooleanExtra(KEY_FOREGROUND, false) || ACTION_RESTART.equals(intentAction);
    }
    // intentAction is null if the service is restarted or no action is specified.
    if (intentAction == null) {
      intentAction = ACTION_INIT;
    }
    logd("onStartCommand action: " + intentAction + " startId: " + startId);
    switch (intentAction) {
      case ACTION_INIT:
      case ACTION_RESTART:
        // Do nothing.
        break;
      case ACTION_ADD:
        byte[] actionData = intent.getByteArrayExtra(KEY_DOWNLOAD_ACTION);
        if (actionData == null) {
          Log.e(TAG, "Ignoring ADD action with no action data");
        } else {
          try {
            downloadManager.handleAction(actionData);
          } catch (IOException e) {
            Log.e(TAG, "Failed to handle ADD action", e);
          }
        }
        break;
      case ACTION_RELOAD_REQUIREMENTS:
        stopWatchingRequirements();
        break;
      default:
        Log.e(TAG, "Ignoring unrecognized action: " + intentAction);
        break;
    }
    Requirements requirements = getRequirements();
    if (requirements.checkRequirements(this)) {
      downloadManager.startDownloads();
    } else {
      downloadManager.stopDownloads();
    }
    maybeStartWatchingRequirements(requirements);
    if (downloadManager.isIdle()) {
      stop();
    }
    return START_STICKY;
  }
  @Override
  public void onTaskRemoved(Intent rootIntent) {
    logd("onTaskRemoved rootIntent: " + rootIntent);
    taskRemoved = true;
  }
  @Override
  public void onDestroy() {
    logd("onDestroy");
    if (foregroundNotificationUpdater != null) {
      foregroundNotificationUpdater.stopPeriodicUpdates();
    }
    downloadManager.removeListener(downloadManagerListener);
    maybeStopWatchingRequirements();
  }
  @Nullable
  @Override
  public IBinder onBind(Intent intent) {
    return null;
  }
  /**
   * Returns a {@link DownloadManager} to be used to downloaded content. Called only once in the
   * life cycle of the service. The service will call {@link DownloadManager#startDownloads()} and
   * {@link DownloadManager#stopDownloads} as necessary when requirements returned by {@link
   * #getRequirements()} are met or stop being met.
   */
  protected abstract DownloadManager getDownloadManager();
  /**
   * Returns a {@link Scheduler} to restart the service when requirements allowing downloads to take
   * place are met. If {@code null}, the service will only be restarted if the process is still in
   * memory when the requirements are met.
   */
  protected abstract @Nullable Scheduler getScheduler();
  /**
   * Returns requirements for downloads to take place. By default the only requirement is that the
   * device has network connectivity.
   */
  protected Requirements getRequirements() {
    return DEFAULT_REQUIREMENTS;
  }
  /**
   * Should be overridden in the subclass if the service will be run in the foreground.
   *
   * <p>Returns a notification to be displayed when this service running in the foreground.
   *
   * <p>This method is called when there is a task state change and periodically while there are
   * active tasks. The periodic update interval can be set using {@link #DownloadService(int,
   * long)}.
   *
   * <p>On API level 26 and above, this method may also be called just before the service stops,
   * with an empty {@code taskStates} array. The returned notification is used to satisfy system
   * requirements for foreground services.
   *
   * @param taskStates The states of all current tasks.
   * @return The foreground notification to display.
   */
  protected Notification getForegroundNotification(TaskState[] taskStates) {
    throw new IllegalStateException(
        getClass().getName()
            + " is started in the foreground but getForegroundNotification() is not implemented.");
  }
  /**
   * Called when the state of a task changes.
   *
   * @param taskState The state of the task.
   */
  protected void onTaskStateChanged(TaskState taskState) {
    // Do nothing.
  }
  private void maybeStartWatchingRequirements(Requirements requirements) {
    if (downloadManager.getDownloadCount() == 0) {
      return;
    }
    Class<? extends DownloadService> clazz = getClass();
    RequirementsHelper requirementsHelper = requirementsHelpers.get(clazz);
    if (requirementsHelper == null) {
      requirementsHelper = new RequirementsHelper(this, requirements, getScheduler(), clazz);
      requirementsHelpers.put(clazz, requirementsHelper);
      requirementsHelper.start();
      logd("started watching requirements");
    }
  }
  private void maybeStopWatchingRequirements() {
    if (downloadManager.getDownloadCount() > 0) {
      return;
    }
    stopWatchingRequirements();
  }
  private void stopWatchingRequirements() {
    RequirementsHelper requirementsHelper = requirementsHelpers.remove(getClass());
    if (requirementsHelper != null) {
      requirementsHelper.stop();
      logd("stopped watching requirements");
    }
  }
  private void stop() {
    if (foregroundNotificationUpdater != null) {
      foregroundNotificationUpdater.stopPeriodicUpdates();
      // Make sure startForeground is called before stopping. Workaround for [Internal: b/69424260].
      if (startedInForeground && Util.SDK_INT >= 26) {
        foregroundNotificationUpdater.showNotificationIfNotAlready();
      }
    }
    if (Util.SDK_INT < 28 && taskRemoved) { // See [Internal: b/74248644].
      stopSelf();
      logd("stopSelf()");
    } else {
      boolean stopSelfResult = stopSelfResult(lastStartId);
      logd("stopSelf(" + lastStartId + ") result: " + stopSelfResult);
    }
  }
  private void logd(String message) {
    if (DEBUG) {
      Log.d(TAG, message);
    }
  }
  private static Intent getIntent(
      Context context, Class<? extends DownloadService> clazz, String action) {
    return new Intent(context, clazz).setAction(action);
  }
  private final class DownloadManagerListener implements DownloadManager.Listener {
    @Override
    public void onInitialized(DownloadManager downloadManager) {
      maybeStartWatchingRequirements(getRequirements());
    }
    @Override
    public void onTaskStateChanged(DownloadManager downloadManager, TaskState taskState) {
      DownloadService.this.onTaskStateChanged(taskState);
      if (foregroundNotificationUpdater != null) {
        if (taskState.state == TaskState.STATE_STARTED) {
          foregroundNotificationUpdater.startPeriodicUpdates();
        } else {
          foregroundNotificationUpdater.update();
        }
      }
    }
    @Override
    public final void onIdle(DownloadManager downloadManager) {
      stop();
    }
  }
  private final class ForegroundNotificationUpdater implements Runnable {
    private final int notificationId;
    private final long updateInterval;
    private final Handler handler;
    private boolean periodicUpdatesStarted;
    private boolean notificationDisplayed;
    public ForegroundNotificationUpdater(int notificationId, long updateInterval) {
      this.notificationId = notificationId;
      this.updateInterval = updateInterval;
      this.handler = new Handler(Looper.getMainLooper());
    }
    public void startPeriodicUpdates() {
      periodicUpdatesStarted = true;
      update();
    }
    public void stopPeriodicUpdates() {
      periodicUpdatesStarted = false;
      handler.removeCallbacks(this);
    }
    public void update() {
      TaskState[] taskStates = downloadManager.getAllTaskStates();
      startForeground(notificationId, getForegroundNotification(taskStates));
      notificationDisplayed = true;
      if (periodicUpdatesStarted) {
        handler.removeCallbacks(this);
        handler.postDelayed(this, updateInterval);
      }
    }
    public void showNotificationIfNotAlready() {
      if (!notificationDisplayed) {
        update();
      }
    }
    @Override
    public void run() {
      update();
    }
  }
  private static final class RequirementsHelper implements RequirementsWatcher.Listener {
    private final Context context;
    private final Requirements requirements;
    private final @Nullable Scheduler scheduler;
    private final Class<? extends DownloadService> serviceClass;
    private final RequirementsWatcher requirementsWatcher;
    private RequirementsHelper(
        Context context,
        Requirements requirements,
        @Nullable Scheduler scheduler,
        Class<? extends DownloadService> serviceClass) {
      this.context = context;
      this.requirements = requirements;
      this.scheduler = scheduler;
      this.serviceClass = serviceClass;
      requirementsWatcher = new RequirementsWatcher(context, this, requirements);
    }
    public void start() {
      requirementsWatcher.start();
    }
    public void stop() {
      requirementsWatcher.stop();
      if (scheduler != null) {
        scheduler.cancel();
      }
    }
    @Override
    public void requirementsMet(RequirementsWatcher requirementsWatcher) {
      try {
        notifyService();
      } catch (Exception e) {
        /* If we can't notify the service, don't stop the scheduler. */
        return;
      }
      if (scheduler != null) {
        scheduler.cancel();
      }
    }
    @Override
    public void requirementsNotMet(RequirementsWatcher requirementsWatcher) {
      try {
        notifyService();
      } catch (Exception e) {
        /* Do nothing. The service isn't running anyway. */
      }
      if (scheduler != null) {
        String servicePackage = context.getPackageName();
        boolean success = scheduler.schedule(requirements, servicePackage, ACTION_RESTART);
        if (!success) {
          Log.e(TAG, "Scheduling downloads failed.");
        }
      }
    }
    private void notifyService() throws Exception {
      Intent intent = getIntent(context, serviceClass, DownloadService.ACTION_INIT);
      try {
        context.startService(intent);
      } catch (IllegalStateException e) {
        /* startService will fail if the app is in the background and the service isn't running. */
        throw new Exception(e);
      }
    }
  }
}
```

## FilterableManifest


```java

package com.google.android.exoplayer2.offline;
import java.util.List;
/**
 * A manifest that can generate copies of itself including only the streams specified by the given
 * keys.
 *
 * @param <T> The manifest type.
 */
public interface FilterableManifest<T> {
  /**
   * Returns a copy of the manifest including only the streams specified by the given keys. If the
   * manifest is unchanged then the instance may return itself.
   *
   * @param streamKeys A non-empty list of stream keys.
   * @return The filtered manifest.
   */
  T copy(List<StreamKey> streamKeys);
}
```

## FilteringManifestParser


```java

package com.google.android.exoplayer2.offline;
import android.net.Uri;
import com.google.android.exoplayer2.upstream.ParsingLoadable.Parser;
import java.io.IOException;
import java.io.InputStream;
import java.util.List;
/** A manifest parser that includes only the streams identified by the given stream keys. */
public final class FilteringManifestParser<T extends FilterableManifest<T>> implements Parser<T> {
  private final Parser<T> parser;
  private final List<StreamKey> streamKeys;
  /**
   * @param parser A parser for the manifest that will be filtered.
   * @param streamKeys The stream keys. If null or empty then filtering will not occur.
   */
  public FilteringManifestParser(Parser<T> parser, List<StreamKey> streamKeys) {
    this.parser = parser;
    this.streamKeys = streamKeys;
  }
  @Override
  public T parse(Uri uri, InputStream inputStream) throws IOException {
    T manifest = parser.parse(uri, inputStream);
    return streamKeys == null || streamKeys.isEmpty() ? manifest : manifest.copy(streamKeys);
  }
}
```

## ProgressiveDownloadAction


```java

package com.google.android.exoplayer2.offline;
import android.net.Uri;
import android.support.annotation.Nullable;
import com.google.android.exoplayer2.upstream.cache.CacheUtil;
import com.google.android.exoplayer2.util.Util;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
/** An action to download or remove downloaded progressive streams. */
public final class ProgressiveDownloadAction extends DownloadAction {
  private static final String TYPE = "progressive";
  private static final int VERSION = 0;
  public static final Deserializer DESERIALIZER =
      new Deserializer(TYPE, VERSION) {
        @Override
        public ProgressiveDownloadAction readFromStream(int version, DataInputStream input)
            throws IOException {
          Uri uri = Uri.parse(input.readUTF());
          boolean isRemoveAction = input.readBoolean();
          int dataLength = input.readInt();
          byte[] data = new byte[dataLength];
          input.readFully(data);
          String customCacheKey = input.readBoolean() ? input.readUTF() : null;
          return new ProgressiveDownloadAction(uri, isRemoveAction, data, customCacheKey);
        }
      };
  private final @Nullable String customCacheKey;
  /**
   * Creates a progressive stream download action.
   *
   * @param uri Uri of the data to be downloaded.
   * @param data Optional custom data for this action.
   * @param customCacheKey A custom key that uniquely identifies the original stream. If not null it
   *     is used for cache indexing.
   */
  public static ProgressiveDownloadAction createDownloadAction(
      Uri uri, @Nullable byte[] data, @Nullable String customCacheKey) {
    return new ProgressiveDownloadAction(uri, /* isRemoveAction= */ false, data, customCacheKey);
  }
  /**
   * Creates a progressive stream remove action.
   *
   * @param uri Uri of the data to be removed.
   * @param data Optional custom data for this action.
   * @param customCacheKey A custom key that uniquely identifies the original stream. If not null it
   *     is used for cache indexing.
   */
  public static ProgressiveDownloadAction createRemoveAction(
      Uri uri, @Nullable byte[] data, @Nullable String customCacheKey) {
    return new ProgressiveDownloadAction(uri, /* isRemoveAction= */ true, data, customCacheKey);
  }
  /**
   * @param uri Uri of the data to be downloaded.
   * @param isRemoveAction Whether this is a remove action. If false, this is a download action.
   * @param data Optional custom data for this action.
   * @param customCacheKey A custom key that uniquely identifies the original stream. If not null it
   *     is used for cache indexing.
   * @deprecated Use {@link #createDownloadAction(Uri, byte[], String)} or {@link
   *     #createRemoveAction(Uri, byte[], String)}.
   */
  @Deprecated
  public ProgressiveDownloadAction(
      Uri uri, boolean isRemoveAction, @Nullable byte[] data, @Nullable String customCacheKey) {
    super(TYPE, VERSION, uri, isRemoveAction, data);
    this.customCacheKey = customCacheKey;
  }
  @Override
  public ProgressiveDownloader createDownloader(DownloaderConstructorHelper constructorHelper) {
    return new ProgressiveDownloader(uri, customCacheKey, constructorHelper);
  }
  @Override
  protected void writeToStream(DataOutputStream output) throws IOException {
    output.writeUTF(uri.toString());
    output.writeBoolean(isRemoveAction);
    output.writeInt(data.length);
    output.write(data);
    boolean customCacheKeySet = customCacheKey != null;
    output.writeBoolean(customCacheKeySet);
    if (customCacheKeySet) {
      output.writeUTF(customCacheKey);
    }
  }
  @Override
  public boolean isSameMedia(DownloadAction other) {
    return ((other instanceof ProgressiveDownloadAction)
        && getCacheKey().equals(((ProgressiveDownloadAction) other).getCacheKey()));
  }
  @Override
  public boolean equals(@Nullable Object o) {
    if (this == o) {
      return true;
    }
    if (!super.equals(o)) {
      return false;
    }
    ProgressiveDownloadAction that = (ProgressiveDownloadAction) o;
    return Util.areEqual(customCacheKey, that.customCacheKey);
  }
  @Override
  public int hashCode() {
    int result = super.hashCode();
    result = 31 * result + (customCacheKey != null ? customCacheKey.hashCode() : 0);
    return result;
  }
  private String getCacheKey() {
    return customCacheKey != null ? customCacheKey : CacheUtil.generateKey(uri);
  }
}
```

## ProgressiveDownloader


```java

package com.google.android.exoplayer2.offline;
import android.net.Uri;
import com.google.android.exoplayer2.C;
import com.google.android.exoplayer2.upstream.DataSpec;
import com.google.android.exoplayer2.upstream.cache.Cache;
import com.google.android.exoplayer2.upstream.cache.CacheDataSource;
import com.google.android.exoplayer2.upstream.cache.CacheUtil;
import com.google.android.exoplayer2.upstream.cache.CacheUtil.CachingCounters;
import com.google.android.exoplayer2.util.PriorityTaskManager;
import java.io.IOException;
import java.util.concurrent.atomic.AtomicBoolean;
/**
 * A downloader for progressive media streams.
 */
public final class ProgressiveDownloader implements Downloader {
  private static final int BUFFER_SIZE_BYTES = 128 * 1024;
  private final DataSpec dataSpec;
  private final Cache cache;
  private final CacheDataSource dataSource;
  private final PriorityTaskManager priorityTaskManager;
  private final CacheUtil.CachingCounters cachingCounters;
  private final AtomicBoolean isCanceled;
  /**
   * @param uri Uri of the data to be downloaded.
   * @param customCacheKey A custom key that uniquely identifies the original stream. Used for cache
   *     indexing. May be null.
   * @param constructorHelper A {@link DownloaderConstructorHelper} instance.
   */
  public ProgressiveDownloader(
      Uri uri, String customCacheKey, DownloaderConstructorHelper constructorHelper) {
    this.dataSpec = new DataSpec(uri, 0, C.LENGTH_UNSET, customCacheKey, 0);
    this.cache = constructorHelper.getCache();
    this.dataSource = constructorHelper.buildCacheDataSource(false);
    this.priorityTaskManager = constructorHelper.getPriorityTaskManager();
    cachingCounters = new CachingCounters();
    isCanceled = new AtomicBoolean();
  }
  @Override
  public void download() throws InterruptedException, IOException {
    priorityTaskManager.add(C.PRIORITY_DOWNLOAD);
    try {
      CacheUtil.cache(
          dataSpec,
          cache,
          dataSource,
          new byte[BUFFER_SIZE_BYTES],
          priorityTaskManager,
          C.PRIORITY_DOWNLOAD,
          cachingCounters,
          isCanceled,
          /* enableEOFException= */ true);
    } finally {
      priorityTaskManager.remove(C.PRIORITY_DOWNLOAD);
    }
  }
  @Override
  public void cancel() {
    isCanceled.set(true);
  }
  @Override
  public long getDownloadedBytes() {
    return cachingCounters.totalCachedBytes();
  }
  @Override
  public float getDownloadPercentage() {
    long contentLength = cachingCounters.contentLength;
    return contentLength == C.LENGTH_UNSET
        ? C.PERCENTAGE_UNSET
        : ((cachingCounters.totalCachedBytes() * 100f) / contentLength);
  }
  @Override
  public void remove() {
    CacheUtil.remove(cache, CacheUtil.getKey(dataSpec));
  }
}
```

## ProgressiveDownloadHelper


```java

package com.google.android.exoplayer2.offline;
import android.net.Uri;
import android.support.annotation.Nullable;
import com.google.android.exoplayer2.source.TrackGroupArray;
import java.util.List;
/** A {@link DownloadHelper} for progressive streams. */
public final class ProgressiveDownloadHelper extends DownloadHelper {
  private final Uri uri;
  private final @Nullable String customCacheKey;
  public ProgressiveDownloadHelper(Uri uri) {
    this(uri, null);
  }
  public ProgressiveDownloadHelper(Uri uri, @Nullable String customCacheKey) {
    this.uri = uri;
    this.customCacheKey = customCacheKey;
  }
  @Override
  protected void prepareInternal() {
    // Do nothing.
  }
  @Override
  public int getPeriodCount() {
    return 1;
  }
  @Override
  public TrackGroupArray getTrackGroups(int periodIndex) {
    return TrackGroupArray.EMPTY;
  }
  @Override
  public ProgressiveDownloadAction getDownloadAction(
      @Nullable byte[] data, List<TrackKey> trackKeys) {
    return ProgressiveDownloadAction.createDownloadAction(uri, data, customCacheKey);
  }
  @Override
  public ProgressiveDownloadAction getRemoveAction(@Nullable byte[] data) {
    return ProgressiveDownloadAction.createRemoveAction(uri, data, customCacheKey);
  }
}
```

## SegmentDownloadAction


```java

package com.google.android.exoplayer2.offline;
import android.net.Uri;
import android.support.annotation.Nullable;
import com.google.android.exoplayer2.util.Assertions;
import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
/** {@link DownloadAction} for {@link SegmentDownloader}s. */
public abstract class SegmentDownloadAction extends DownloadAction {
  /** Base class for {@link SegmentDownloadAction} {@link Deserializer}s. */
  protected abstract static class SegmentDownloadActionDeserializer extends Deserializer {
    public SegmentDownloadActionDeserializer(String type, int version) {
      super(type, version);
    }
    @Override
    public final DownloadAction readFromStream(int version, DataInputStream input)
        throws IOException {
      Uri uri = Uri.parse(input.readUTF());
      boolean isRemoveAction = input.readBoolean();
      int dataLength = input.readInt();
      byte[] data = new byte[dataLength];
      input.readFully(data);
      int keyCount = input.readInt();
      List<StreamKey> keys = new ArrayList<>();
      for (int i = 0; i < keyCount; i++) {
        keys.add(readKey(version, input));
      }
      return createDownloadAction(uri, isRemoveAction, data, keys);
    }
    /** Deserializes a key from the {@code input}. */
    protected StreamKey readKey(int version, DataInputStream input) throws IOException {
      int periodIndex = input.readInt();
      int groupIndex = input.readInt();
      int trackIndex = input.readInt();
      return new StreamKey(periodIndex, groupIndex, trackIndex);
    }
    /** Returns a {@link DownloadAction}. */
    protected abstract DownloadAction createDownloadAction(
        Uri manifestUri, boolean isRemoveAction, byte[] data, List<StreamKey> keys);
  }
  public final List<StreamKey> keys;
  /**
   * @param type The type of the action.
   * @param version The action version.
   * @param uri The URI of the media being downloaded.
   * @param isRemoveAction Whether the data will be removed. If {@code false} it will be downloaded.
   * @param data Optional custom data for this action. If {@code null} an empty array will be used.
   * @param keys Keys of tracks to be downloaded. If empty, all tracks will be downloaded. If {@code
   *     removeAction} is true, {@code keys} must be empty.
   */
  protected SegmentDownloadAction(
      String type,
      int version,
      Uri uri,
      boolean isRemoveAction,
      @Nullable byte[] data,
      List<StreamKey> keys) {
    super(type, version, uri, isRemoveAction, data);
    if (isRemoveAction) {
      Assertions.checkArgument(keys.isEmpty());
      this.keys = Collections.emptyList();
    } else {
      ArrayList<StreamKey> mutableKeys = new ArrayList<>(keys);
      Collections.sort(mutableKeys);
      this.keys = Collections.unmodifiableList(mutableKeys);
    }
  }
  @Override
  public List<StreamKey> getKeys() {
    return keys;
  }
  @Override
  public final void writeToStream(DataOutputStream output) throws IOException {
    output.writeUTF(uri.toString());
    output.writeBoolean(isRemoveAction);
    output.writeInt(data.length);
    output.write(data);
    output.writeInt(keys.size());
    for (int i = 0; i < keys.size(); i++) {
      writeKey(output, keys.get(i));
    }
  }
  @Override
  public boolean equals(@Nullable Object o) {
    if (this == o) {
      return true;
    }
    if (!super.equals(o)) {
      return false;
    }
    SegmentDownloadAction that = (SegmentDownloadAction) o;
    return keys.equals(that.keys);
  }
  @Override
  public int hashCode() {
    int result = super.hashCode();
    result = 31 * result + keys.hashCode();
    return result;
  }
  /** Serializes the {@code key} into the {@code output}. */
  private void writeKey(DataOutputStream output, StreamKey key) throws IOException {
    output.writeInt(key.periodIndex);
    output.writeInt(key.groupIndex);
    output.writeInt(key.trackIndex);
  }
}
```

## SegmentDownloader


```java

package com.google.android.exoplayer2.offline;
import android.net.Uri;
import android.support.annotation.NonNull;
import com.google.android.exoplayer2.C;
import com.google.android.exoplayer2.upstream.DataSource;
import com.google.android.exoplayer2.upstream.DataSpec;
import com.google.android.exoplayer2.upstream.cache.Cache;
import com.google.android.exoplayer2.upstream.cache.CacheDataSource;
import com.google.android.exoplayer2.upstream.cache.CacheUtil;
import com.google.android.exoplayer2.upstream.cache.CacheUtil.CachingCounters;
import com.google.android.exoplayer2.util.PriorityTaskManager;
import com.google.android.exoplayer2.util.Util;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.concurrent.atomic.AtomicBoolean;
/**
 * Base class for multi segment stream downloaders.
 *
 * @param <M> The type of the manifest object.
 */
public abstract class SegmentDownloader<M extends FilterableManifest<M>> implements Downloader {
  /** Smallest unit of content to be downloaded. */
  protected static class Segment implements Comparable<Segment> {
    /** The start time of the segment in microseconds. */
    public final long startTimeUs;
    /** The {@link DataSpec} of the segment. */
    public final DataSpec dataSpec;
    /** Constructs a Segment. */
    public Segment(long startTimeUs, DataSpec dataSpec) {
      this.startTimeUs = startTimeUs;
      this.dataSpec = dataSpec;
    }
    @Override
    public int compareTo(@NonNull Segment other) {
      return Util.compareLong(startTimeUs, other.startTimeUs);
    }
  }
  private static final int BUFFER_SIZE_BYTES = 128 * 1024;
  private final Uri manifestUri;
  private final PriorityTaskManager priorityTaskManager;
  private final Cache cache;
  private final CacheDataSource dataSource;
  private final CacheDataSource offlineDataSource;
  private final ArrayList<StreamKey> streamKeys;
  private final AtomicBoolean isCanceled;
  private volatile int totalSegments;
  private volatile int downloadedSegments;
  private volatile long downloadedBytes;
  /**
   * @param manifestUri The {@link Uri} of the manifest to be downloaded.
   * @param streamKeys Keys defining which streams in the manifest should be selected for download.
   *     If empty, all streams are downloaded.
   * @param constructorHelper A {@link DownloaderConstructorHelper} instance.
   */
  public SegmentDownloader(
      Uri manifestUri, List<StreamKey> streamKeys, DownloaderConstructorHelper constructorHelper) {
    this.manifestUri = manifestUri;
    this.streamKeys = new ArrayList<>(streamKeys);
    this.cache = constructorHelper.getCache();
    this.dataSource = constructorHelper.buildCacheDataSource(false);
    this.offlineDataSource = constructorHelper.buildCacheDataSource(true);
    this.priorityTaskManager = constructorHelper.getPriorityTaskManager();
    totalSegments = C.LENGTH_UNSET;
    isCanceled = new AtomicBoolean();
  }
  /**
   * Downloads the selected streams in the media. If multiple streams are selected, they are
   * downloaded in sync with one another.
   *
   * @throws IOException Thrown when there is an error downloading.
   * @throws InterruptedException If the thread has been interrupted.
   */
  // downloadedSegments and downloadedBytes are only written from this method, and this method
  // should not be called from more than one thread. Hence non-atomic updates are valid.
  @SuppressWarnings("NonAtomicVolatileUpdate")
  @Override
  public final void download() throws IOException, InterruptedException {
    priorityTaskManager.add(C.PRIORITY_DOWNLOAD);
    try {
      List<Segment> segments = initDownload();
      Collections.sort(segments);
      byte[] buffer = new byte[BUFFER_SIZE_BYTES];
      CachingCounters cachingCounters = new CachingCounters();
      for (int i = 0; i < segments.size(); i++) {
        try {
          CacheUtil.cache(
              segments.get(i).dataSpec,
              cache,
              dataSource,
              buffer,
              priorityTaskManager,
              C.PRIORITY_DOWNLOAD,
              cachingCounters,
              isCanceled,
              true);
          downloadedSegments++;
        } finally {
          downloadedBytes += cachingCounters.newlyCachedBytes;
        }
      }
    } finally {
      priorityTaskManager.remove(C.PRIORITY_DOWNLOAD);
    }
  }
  @Override
  public void cancel() {
    isCanceled.set(true);
  }
  @Override
  public final long getDownloadedBytes() {
    return downloadedBytes;
  }
  @Override
  public final float getDownloadPercentage() {
    // Take local snapshot of the volatile fields
    int totalSegments = this.totalSegments;
    int downloadedSegments = this.downloadedSegments;
    if (totalSegments == C.LENGTH_UNSET || downloadedSegments == C.LENGTH_UNSET) {
      return C.PERCENTAGE_UNSET;
    }
    return totalSegments == 0 ? 100f : (downloadedSegments * 100f) / totalSegments;
  }
  @Override
  public final void remove() throws InterruptedException {
    try {
      M manifest = getManifest(offlineDataSource, manifestUri);
      List<Segment> segments = getSegments(offlineDataSource, manifest, true);
      for (int i = 0; i < segments.size(); i++) {
        removeUri(segments.get(i).dataSpec.uri);
      }
    } catch (IOException e) {
      // Ignore exceptions when removing.
    } finally {
      // Always attempt to remove the manifest.
      removeUri(manifestUri);
    }
  }
  // Internal methods.
  /**
   * Loads and parses the manifest.
   *
   * @param dataSource The {@link DataSource} through which to load.
   * @param uri The manifest uri.
   * @return The manifest.
   * @throws IOException If an error occurs reading data.
   */
  protected abstract M getManifest(DataSource dataSource, Uri uri) throws IOException;
  /**
   * Returns a list of all downloadable {@link Segment}s for a given manifest.
   *
   * @param dataSource The {@link DataSource} through which to load any required data.
   * @param manifest The manifest containing the segments.
   * @param allowIncompleteList Whether to continue in the case that a load error prevents all
   *     segments from being listed. If true then a partial segment list will be returned. If false
   *     an {@link IOException} will be thrown.
   * @throws InterruptedException Thrown if the thread was interrupted.
   * @throws IOException Thrown if {@code allowPartialIndex} is false and a load error occurs, or if
   *     the media is not in a form that allows for its segments to be listed.
   * @return The list of downloadable {@link Segment}s.
   */
  protected abstract List<Segment> getSegments(
      DataSource dataSource, M manifest, boolean allowIncompleteList)
      throws InterruptedException, IOException;
  /** Initializes the download, returning a list of {@link Segment}s that need to be downloaded. */
  // Writes to downloadedSegments and downloadedBytes are safe. See the comment on download().
  @SuppressWarnings("NonAtomicVolatileUpdate")
  private List<Segment> initDownload() throws IOException, InterruptedException {
    M manifest = getManifest(dataSource, manifestUri);
    if (!streamKeys.isEmpty()) {
      manifest = manifest.copy(streamKeys);
    }
    List<Segment> segments = getSegments(dataSource, manifest, /* allowIncompleteList= */ false);
    CachingCounters cachingCounters = new CachingCounters();
    totalSegments = segments.size();
    downloadedSegments = 0;
    downloadedBytes = 0;
    for (int i = segments.size() - 1; i >= 0; i--) {
      Segment segment = segments.get(i);
      CacheUtil.getCached(segment.dataSpec, cache, cachingCounters);
      downloadedBytes += cachingCounters.alreadyCachedBytes;
      if (cachingCounters.alreadyCachedBytes == cachingCounters.contentLength) {
        // The segment is fully downloaded.
        downloadedSegments++;
        segments.remove(i);
      }
    }
    return segments;
  }
  private void removeUri(Uri uri) {
    CacheUtil.remove(cache, CacheUtil.generateKey(uri));
  }
}
```

## StreamKey


```java

package com.google.android.exoplayer2.offline;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
/**
 * Identifies a given track by the index of the containing period, the index of the containing group
 * within the period, and the index of the track within the group.
 */
public final class StreamKey implements Comparable<StreamKey> {
  /** The period index. */
  public final int periodIndex;
  /** The group index. */
  public final int groupIndex;
  /** The track index. */
  public final int trackIndex;
  /**
   * @param groupIndex The group index.
   * @param trackIndex The track index.
   */
  public StreamKey(int groupIndex, int trackIndex) {
    this(0, groupIndex, trackIndex);
  }
  /**
   * @param periodIndex The period index.
   * @param groupIndex The group index.
   * @param trackIndex The track index.
   */
  public StreamKey(int periodIndex, int groupIndex, int trackIndex) {
    this.periodIndex = periodIndex;
    this.groupIndex = groupIndex;
    this.trackIndex = trackIndex;
  }
  @Override
  public String toString() {
    return periodIndex + "." + groupIndex + "." + trackIndex;
  }
  @Override
  public boolean equals(@Nullable Object o) {
    if (this == o) {
      return true;
    }
    if (o == null || getClass() != o.getClass()) {
      return false;
    }
    StreamKey that = (StreamKey) o;
    return periodIndex == that.periodIndex
        && groupIndex == that.groupIndex
        && trackIndex == that.trackIndex;
  }
  @Override
  public int hashCode() {
    int result = periodIndex;
    result = 31 * result + groupIndex;
    result = 31 * result + trackIndex;
    return result;
  }
  // Comparable implementation.
  @Override
  public int compareTo(@NonNull StreamKey o) {
    int result = periodIndex - o.periodIndex;
    if (result == 0) {
      result = groupIndex - o.groupIndex;
      if (result == 0) {
        result = trackIndex - o.trackIndex;
      }
    }
    return result;
  }
}
```

## TrackKey


```java

package com.google.android.exoplayer2.offline;
/**
 * Identifies a given track by the index of the containing period, the index of the containing group
 * within the period, and the index of the track within the group.
 */
public final class TrackKey {
  /** The period index. */
  public final int periodIndex;
  /** The group index. */
  public final int groupIndex;
  /** The track index. */
  public final int trackIndex;
  /**
   * @param periodIndex The period index.
   * @param groupIndex The group index.
   * @param trackIndex The track index.
   */
  public TrackKey(int periodIndex, int groupIndex, int trackIndex) {
    this.periodIndex = periodIndex;
    this.groupIndex = groupIndex;
    this.trackIndex = trackIndex;
  }
}
```


