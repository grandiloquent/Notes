

# Java: SettingsLib lifecycle

- [Lifecycle](#lifecycle)
- [LifecycleObserver](#lifecycleobserver)
- [ObservableActivity](#observableactivity)
- [ObservableDialogFragment](#observabledialogfragment)
- [ObservableFragment](#observablefragment)
- [ObservablePreferenceFragment](#observablepreferencefragment)
- [OnAttach](#onattach)
- [OnCreate](#oncreate)
- [OnCreateOptionsMenu](#oncreateoptionsmenu)
- [OnDestroy](#ondestroy)
- [OnOptionsItemSelected](#onoptionsitemselected)
- [OnPause](#onpause)
- [OnPrepareOptionsMenu](#onprepareoptionsmenu)
- [OnResume](#onresume)
- [OnSaveInstanceState](#onsaveinstancestate)
- [OnStart](#onstart)
- [OnStop](#onstop)
- [SetPreferenceScreen](#setpreferencescreen)


https://android.googlesource.com/platform/frameworks/base/+/master/packages/SettingsLib

## Lifecycle


```java

package com.android.settingslib.core.lifecycle;
import static androidx.lifecycle.Lifecycle.Event.ON_ANY;
import android.annotation.UiThread;
import androidx.lifecycle.LifecycleOwner;
import androidx.lifecycle.LifecycleRegistry;
import androidx.lifecycle.OnLifecycleEvent;
import android.content.Context;
import android.os.Bundle;
import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.preference.PreferenceScreen;
import android.util.Log;
import android.view.Menu;
import android.view.MenuInflater;
import android.view.MenuItem;
import com.android.settingslib.core.lifecycle.events.OnAttach;
import com.android.settingslib.core.lifecycle.events.OnCreate;
import com.android.settingslib.core.lifecycle.events.OnCreateOptionsMenu;
import com.android.settingslib.core.lifecycle.events.OnDestroy;
import com.android.settingslib.core.lifecycle.events.OnOptionsItemSelected;
import com.android.settingslib.core.lifecycle.events.OnPause;
import com.android.settingslib.core.lifecycle.events.OnPrepareOptionsMenu;
import com.android.settingslib.core.lifecycle.events.OnResume;
import com.android.settingslib.core.lifecycle.events.OnSaveInstanceState;
import com.android.settingslib.core.lifecycle.events.OnStart;
import com.android.settingslib.core.lifecycle.events.OnStop;
import com.android.settingslib.core.lifecycle.events.SetPreferenceScreen;
import com.android.settingslib.utils.ThreadUtils;
import java.util.ArrayList;
import java.util.List;
/**
 * Dispatcher for lifecycle events.
 */
public class Lifecycle extends LifecycleRegistry {
    private static final String TAG = "LifecycleObserver";
    private final List<LifecycleObserver> mObservers = new ArrayList<>();
    private final LifecycleProxy mProxy = new LifecycleProxy();
    /**
     * Creates a new LifecycleRegistry for the given provider.
     * <p>
     * You should usually create this inside your LifecycleOwner class's constructor and hold
     * onto the same instance.
     *
     * @param provider The owner LifecycleOwner
     */
    public Lifecycle(@NonNull LifecycleOwner provider) {
        super(provider);
        addObserver(mProxy);
    }
    /**
     * Registers a new observer of lifecycle events.
     */
    @UiThread
    @Override
    public void addObserver(androidx.lifecycle.LifecycleObserver observer) {
        ThreadUtils.ensureMainThread();
        super.addObserver(observer);
        if (observer instanceof LifecycleObserver) {
            mObservers.add((LifecycleObserver) observer);
        }
    }
    @UiThread
    @Override
    public void removeObserver(androidx.lifecycle.LifecycleObserver observer) {
        ThreadUtils.ensureMainThread();
        super.removeObserver(observer);
        if (observer instanceof LifecycleObserver) {
            mObservers.remove(observer);
        }
    }
    public void onAttach(Context context) {
        for (int i = 0, size = mObservers.size(); i < size; i++) {
            final LifecycleObserver observer = mObservers.get(i);
            if (observer instanceof OnAttach) {
                ((OnAttach) observer).onAttach(context);
            }
        }
    }
    // This method is not called from the proxy because it does not have access to the
    // savedInstanceState
    public void onCreate(Bundle savedInstanceState) {
        for (int i = 0, size = mObservers.size(); i < size; i++) {
            final LifecycleObserver observer = mObservers.get(i);
            if (observer instanceof OnCreate) {
                ((OnCreate) observer).onCreate(savedInstanceState);
            }
        }
    }
    private void onStart() {
        for (int i = 0, size = mObservers.size(); i < size; i++) {
            final LifecycleObserver observer = mObservers.get(i);
            if (observer instanceof OnStart) {
                ((OnStart) observer).onStart();
            }
        }
    }
    public void setPreferenceScreen(PreferenceScreen preferenceScreen) {
        for (int i = 0, size = mObservers.size(); i < size; i++) {
            final LifecycleObserver observer = mObservers.get(i);
            if (observer instanceof SetPreferenceScreen) {
                ((SetPreferenceScreen) observer).setPreferenceScreen(preferenceScreen);
            }
        }
    }
    private void onResume() {
        for (int i = 0, size = mObservers.size(); i < size; i++) {
            final LifecycleObserver observer = mObservers.get(i);
            if (observer instanceof OnResume) {
                ((OnResume) observer).onResume();
            }
        }
    }
    private void onPause() {
        for (int i = 0, size = mObservers.size(); i < size; i++) {
            final LifecycleObserver observer = mObservers.get(i);
            if (observer instanceof OnPause) {
                ((OnPause) observer).onPause();
            }
        }
    }
    public void onSaveInstanceState(Bundle outState) {
        for (int i = 0, size = mObservers.size(); i < size; i++) {
            final LifecycleObserver observer = mObservers.get(i);
            if (observer instanceof OnSaveInstanceState) {
                ((OnSaveInstanceState) observer).onSaveInstanceState(outState);
            }
        }
    }
    private void onStop() {
        for (int i = 0, size = mObservers.size(); i < size; i++) {
            final LifecycleObserver observer = mObservers.get(i);
            if (observer instanceof OnStop) {
                ((OnStop) observer).onStop();
            }
        }
    }
    private void onDestroy() {
        for (int i = 0, size = mObservers.size(); i < size; i++) {
            final LifecycleObserver observer = mObservers.get(i);
            if (observer instanceof OnDestroy) {
                ((OnDestroy) observer).onDestroy();
            }
        }
    }
    public void onCreateOptionsMenu(final Menu menu, final @Nullable MenuInflater inflater) {
        for (int i = 0, size = mObservers.size(); i < size; i++) {
            final LifecycleObserver observer = mObservers.get(i);
            if (observer instanceof OnCreateOptionsMenu) {
                ((OnCreateOptionsMenu) observer).onCreateOptionsMenu(menu, inflater);
            }
        }
    }
    public void onPrepareOptionsMenu(final Menu menu) {
        for (int i = 0, size = mObservers.size(); i < size; i++) {
            final LifecycleObserver observer = mObservers.get(i);
            if (observer instanceof OnPrepareOptionsMenu) {
                ((OnPrepareOptionsMenu) observer).onPrepareOptionsMenu(menu);
            }
        }
    }
    public boolean onOptionsItemSelected(final MenuItem menuItem) {
        for (int i = 0, size = mObservers.size(); i < size; i++) {
            final LifecycleObserver observer = mObservers.get(i);
            if (observer instanceof OnOptionsItemSelected) {
                if (((OnOptionsItemSelected) observer).onOptionsItemSelected(menuItem)) {
                    return true;
                }
            }
        }
        return false;
    }
    private class LifecycleProxy
            implements androidx.lifecycle.LifecycleObserver {
        @OnLifecycleEvent(ON_ANY)
        public void onLifecycleEvent(LifecycleOwner owner, Event event) {
            switch (event) {
                case ON_CREATE:
                    // onCreate is called directly since we don't have savedInstanceState here
                    break;
                case ON_START:
                    onStart();
                    break;
                case ON_RESUME:
                    onResume();
                    break;
                case ON_PAUSE:
                    onPause();
                    break;
                case ON_STOP:
                    onStop();
                    break;
                case ON_DESTROY:
                    onDestroy();
                    break;
                case ON_ANY:
                    Log.wtf(TAG, "Should not receive an 'ANY' event!");
                    break;
            }
        }
    }
}
```

## LifecycleObserver


```java

package com.android.settingslib.core.lifecycle;
/**
 * Observer of lifecycle events.
 * @deprecated use {@link androidx.lifecycle.LifecycleObserver} instead
 */
@Deprecated
public interface LifecycleObserver extends
        androidx.lifecycle.LifecycleObserver {
}
```

## ObservableActivity


```java

package com.android.settingslib.core.lifecycle;
import static androidx.lifecycle.Lifecycle.Event.ON_CREATE;
import static androidx.lifecycle.Lifecycle.Event.ON_DESTROY;
import static androidx.lifecycle.Lifecycle.Event.ON_PAUSE;
import static androidx.lifecycle.Lifecycle.Event.ON_RESUME;
import static androidx.lifecycle.Lifecycle.Event.ON_START;
import static androidx.lifecycle.Lifecycle.Event.ON_STOP;
import android.annotation.Nullable;
import android.app.Activity;
import androidx.lifecycle.LifecycleOwner;
import android.os.Bundle;
import android.os.PersistableBundle;
import android.view.Menu;
import android.view.MenuItem;
/**
 * {@link Activity} that has hooks to observe activity lifecycle events.
 */
public class ObservableActivity extends Activity implements LifecycleOwner {
    private final Lifecycle mLifecycle = new Lifecycle(this);
    public Lifecycle getLifecycle() {
        return mLifecycle;
    }
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        mLifecycle.onAttach(this);
        mLifecycle.onCreate(savedInstanceState);
        mLifecycle.handleLifecycleEvent(ON_CREATE);
        super.onCreate(savedInstanceState);
    }
    @Override
    public void onCreate(@Nullable Bundle savedInstanceState,
            @Nullable PersistableBundle persistentState) {
        mLifecycle.onAttach(this);
        mLifecycle.onCreate(savedInstanceState);
        mLifecycle.handleLifecycleEvent(ON_CREATE);
        super.onCreate(savedInstanceState, persistentState);
    }
    @Override
    protected void onStart() {
        mLifecycle.handleLifecycleEvent(ON_START);
        super.onStart();
    }
    @Override
    protected void onResume() {
        mLifecycle.handleLifecycleEvent(ON_RESUME);
        super.onResume();
    }
    @Override
    protected void onPause() {
        mLifecycle.handleLifecycleEvent(ON_PAUSE);
        super.onPause();
    }
    @Override
    protected void onStop() {
        mLifecycle.handleLifecycleEvent(ON_STOP);
        super.onStop();
    }
    @Override
    protected void onDestroy() {
        mLifecycle.handleLifecycleEvent(ON_DESTROY);
        super.onDestroy();
    }
    @Override
    public boolean onCreateOptionsMenu(final Menu menu) {
        if (super.onCreateOptionsMenu(menu)) {
            mLifecycle.onCreateOptionsMenu(menu, null);
            return true;
        }
        return false;
    }
    @Override
    public boolean onPrepareOptionsMenu(final Menu menu) {
        if (super.onPrepareOptionsMenu(menu)) {
            mLifecycle.onPrepareOptionsMenu(menu);
            return true;
        }
        return false;
    }
    @Override
    public boolean onOptionsItemSelected(final MenuItem menuItem) {
        boolean lifecycleHandled = mLifecycle.onOptionsItemSelected(menuItem);
        if (!lifecycleHandled) {
            return super.onOptionsItemSelected(menuItem);
        }
        return lifecycleHandled;
    }
}
```

## ObservableDialogFragment


```java

package com.android.settingslib.core.lifecycle;
import static androidx.lifecycle.Lifecycle.Event.ON_CREATE;
import static androidx.lifecycle.Lifecycle.Event.ON_DESTROY;
import static androidx.lifecycle.Lifecycle.Event.ON_PAUSE;
import static androidx.lifecycle.Lifecycle.Event.ON_RESUME;
import static androidx.lifecycle.Lifecycle.Event.ON_START;
import static androidx.lifecycle.Lifecycle.Event.ON_STOP;
import android.app.DialogFragment;
import androidx.lifecycle.LifecycleOwner;
import android.content.Context;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuInflater;
import android.view.MenuItem;
/**
 * {@link DialogFragment} that has hooks to observe fragment lifecycle events.
 */
public class ObservableDialogFragment extends DialogFragment implements LifecycleOwner {
    protected final Lifecycle mLifecycle = new Lifecycle(this);
    @Override
    public void onAttach(Context context) {
        super.onAttach(context);
        mLifecycle.onAttach(context);
    }
    @Override
    public void onCreate(Bundle savedInstanceState) {
        mLifecycle.onCreate(savedInstanceState);
        mLifecycle.handleLifecycleEvent(ON_CREATE);
        super.onCreate(savedInstanceState);
    }
    @Override
    public void onStart() {
        mLifecycle.handleLifecycleEvent(ON_START);
        super.onStart();
    }
    @Override
    public void onResume() {
        mLifecycle.handleLifecycleEvent(ON_RESUME);
        super.onResume();
    }
    @Override
    public void onPause() {
        mLifecycle.handleLifecycleEvent(ON_PAUSE);
        super.onPause();
    }
    @Override
    public void onStop() {
        mLifecycle.handleLifecycleEvent(ON_STOP);
        super.onStop();
    }
    @Override
    public void onDestroy() {
        mLifecycle.handleLifecycleEvent(ON_DESTROY);
        super.onDestroy();
    }
    @Override
    public void onCreateOptionsMenu(final Menu menu, final MenuInflater inflater) {
        mLifecycle.onCreateOptionsMenu(menu, inflater);
        super.onCreateOptionsMenu(menu, inflater);
    }
    @Override
    public void onPrepareOptionsMenu(final Menu menu) {
        mLifecycle.onPrepareOptionsMenu(menu);
        super.onPrepareOptionsMenu(menu);
    }
    @Override
    public boolean onOptionsItemSelected(final MenuItem menuItem) {
        boolean lifecycleHandled = mLifecycle.onOptionsItemSelected(menuItem);
        if (!lifecycleHandled) {
            return super.onOptionsItemSelected(menuItem);
        }
        return lifecycleHandled;
    }
    @Override
    public Lifecycle getLifecycle() {
        return mLifecycle;
    }
}
```

## ObservableFragment


```java

package com.android.settingslib.core.lifecycle;
import static androidx.lifecycle.Lifecycle.Event.ON_CREATE;
import static androidx.lifecycle.Lifecycle.Event.ON_DESTROY;
import static androidx.lifecycle.Lifecycle.Event.ON_PAUSE;
import static androidx.lifecycle.Lifecycle.Event.ON_RESUME;
import static androidx.lifecycle.Lifecycle.Event.ON_START;
import static androidx.lifecycle.Lifecycle.Event.ON_STOP;
import android.annotation.CallSuper;
import android.app.Fragment;
import androidx.lifecycle.LifecycleOwner;
import android.content.Context;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuInflater;
import android.view.MenuItem;
public class ObservableFragment extends Fragment implements LifecycleOwner {
    private final Lifecycle mLifecycle = new Lifecycle(this);
    public Lifecycle getLifecycle() {
        return mLifecycle;
    }
    @CallSuper
    @Override
    public void onAttach(Context context) {
        super.onAttach(context);
        mLifecycle.onAttach(context);
    }
    @CallSuper
    @Override
    public void onCreate(Bundle savedInstanceState) {
        mLifecycle.onCreate(savedInstanceState);
        mLifecycle.handleLifecycleEvent(ON_CREATE);
        super.onCreate(savedInstanceState);
    }
    @CallSuper
    @Override
    public void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        mLifecycle.onSaveInstanceState(outState);
    }
    @CallSuper
    @Override
    public void onStart() {
        mLifecycle.handleLifecycleEvent(ON_START);
        super.onStart();
    }
    @CallSuper
    @Override
    public void onResume() {
        mLifecycle.handleLifecycleEvent(ON_RESUME);
        super.onResume();
    }
    @CallSuper
    @Override
    public void onPause() {
        mLifecycle.handleLifecycleEvent(ON_PAUSE);
        super.onPause();
    }
    @CallSuper
    @Override
    public void onStop() {
        mLifecycle.handleLifecycleEvent(ON_STOP);
        super.onStop();
    }
    @CallSuper
    @Override
    public void onDestroy() {
        mLifecycle.handleLifecycleEvent(ON_DESTROY);
        super.onDestroy();
    }
    @CallSuper
    @Override
    public void onCreateOptionsMenu(final Menu menu, final MenuInflater inflater) {
        mLifecycle.onCreateOptionsMenu(menu, inflater);
        super.onCreateOptionsMenu(menu, inflater);
    }
    @CallSuper
    @Override
    public void onPrepareOptionsMenu(final Menu menu) {
        mLifecycle.onPrepareOptionsMenu(menu);
        super.onPrepareOptionsMenu(menu);
    }
    @CallSuper
    @Override
    public boolean onOptionsItemSelected(final MenuItem menuItem) {
        boolean lifecycleHandled = mLifecycle.onOptionsItemSelected(menuItem);
        if (!lifecycleHandled) {
            return super.onOptionsItemSelected(menuItem);
        }
        return lifecycleHandled;
    }
}
```

## ObservablePreferenceFragment


```java

package com.android.settingslib.core.lifecycle;
import static androidx.lifecycle.Lifecycle.Event.ON_CREATE;
import static androidx.lifecycle.Lifecycle.Event.ON_DESTROY;
import static androidx.lifecycle.Lifecycle.Event.ON_PAUSE;
import static androidx.lifecycle.Lifecycle.Event.ON_RESUME;
import static androidx.lifecycle.Lifecycle.Event.ON_START;
import static androidx.lifecycle.Lifecycle.Event.ON_STOP;
import android.annotation.CallSuper;
import androidx.lifecycle.LifecycleOwner;
import android.content.Context;
import android.os.Bundle;
import androidx.preference.PreferenceFragment;
import androidx.preference.PreferenceScreen;
import android.view.Menu;
import android.view.MenuInflater;
import android.view.MenuItem;
/**
 * {@link PreferenceFragment} that has hooks to observe fragment lifecycle events.
 */
public abstract class ObservablePreferenceFragment extends PreferenceFragment
        implements LifecycleOwner {
    private final Lifecycle mLifecycle = new Lifecycle(this);
    public Lifecycle getLifecycle() {
        return mLifecycle;
    }
    @CallSuper
    @Override
    public void onAttach(Context context) {
        super.onAttach(context);
        mLifecycle.onAttach(context);
    }
    @CallSuper
    @Override
    public void onCreate(Bundle savedInstanceState) {
        mLifecycle.onCreate(savedInstanceState);
        mLifecycle.handleLifecycleEvent(ON_CREATE);
        super.onCreate(savedInstanceState);
    }
    @Override
    public void setPreferenceScreen(PreferenceScreen preferenceScreen) {
        mLifecycle.setPreferenceScreen(preferenceScreen);
        super.setPreferenceScreen(preferenceScreen);
    }
    @CallSuper
    @Override
    public void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        mLifecycle.onSaveInstanceState(outState);
    }
    @CallSuper
    @Override
    public void onStart() {
        mLifecycle.handleLifecycleEvent(ON_START);
        super.onStart();
    }
    @CallSuper
    @Override
    public void onResume() {
        mLifecycle.handleLifecycleEvent(ON_RESUME);
        super.onResume();
    }
    @CallSuper
    @Override
    public void onPause() {
        mLifecycle.handleLifecycleEvent(ON_PAUSE);
        super.onPause();
    }
    @CallSuper
    @Override
    public void onStop() {
        mLifecycle.handleLifecycleEvent(ON_STOP);
        super.onStop();
    }
    @CallSuper
    @Override
    public void onDestroy() {
        mLifecycle.handleLifecycleEvent(ON_DESTROY);
        super.onDestroy();
    }
    @CallSuper
    @Override
    public void onCreateOptionsMenu(final Menu menu, final MenuInflater inflater) {
        mLifecycle.onCreateOptionsMenu(menu, inflater);
        super.onCreateOptionsMenu(menu, inflater);
    }
    @CallSuper
    @Override
    public void onPrepareOptionsMenu(final Menu menu) {
        mLifecycle.onPrepareOptionsMenu(menu);
        super.onPrepareOptionsMenu(menu);
    }
    @CallSuper
    @Override
    public boolean onOptionsItemSelected(final MenuItem menuItem) {
        boolean lifecycleHandled = mLifecycle.onOptionsItemSelected(menuItem);
        if (!lifecycleHandled) {
            return super.onOptionsItemSelected(menuItem);
        }
        return lifecycleHandled;
    }
}
```

## OnAttach


```java

package com.android.settingslib.core.lifecycle.events;
import android.content.Context;
/**
 * @deprecated pass {@link Context} in constructor instead
 */
@Deprecated
public interface OnAttach {
    void onAttach(Context context);
}
```

## OnCreate


```java

package com.android.settingslib.core.lifecycle.events;
import androidx.lifecycle.Lifecycle;
import androidx.lifecycle.OnLifecycleEvent;
import android.os.Bundle;
/**
 * @deprecated use {@link OnLifecycleEvent(Lifecycle.Event) }
 */
@Deprecated
public interface OnCreate {
    void onCreate(Bundle savedInstanceState);
}
```

## OnCreateOptionsMenu


```java

package com.android.settingslib.core.lifecycle.events;
import android.view.Menu;
import android.view.MenuInflater;
public interface OnCreateOptionsMenu {
    void onCreateOptionsMenu(Menu menu, MenuInflater inflater);
}
```

## OnDestroy


```java

package com.android.settingslib.core.lifecycle.events;
import androidx.lifecycle.Lifecycle;
import androidx.lifecycle.OnLifecycleEvent;
/**
 * @deprecated use {@link OnLifecycleEvent(Lifecycle.Event) }
 */
@Deprecated
public interface OnDestroy {
    void onDestroy();
}
```

## OnOptionsItemSelected


```java

package com.android.settingslib.core.lifecycle.events;
import android.view.MenuItem;
public interface OnOptionsItemSelected {
    boolean onOptionsItemSelected(MenuItem menuItem);
}
```

## OnPause


```java

package com.android.settingslib.core.lifecycle.events;
import androidx.lifecycle.Lifecycle;
import androidx.lifecycle.OnLifecycleEvent;
/**
 * @deprecated use {@link OnLifecycleEvent(Lifecycle.Event) }
 */
@Deprecated
public interface OnPause {
    void onPause();
}
```

## OnPrepareOptionsMenu


```java

package com.android.settingslib.core.lifecycle.events;
import android.view.Menu;
import android.view.MenuInflater;
public interface OnPrepareOptionsMenu {
    void onPrepareOptionsMenu(Menu menu);
}
```

## OnResume


```java

package com.android.settingslib.core.lifecycle.events;
import androidx.lifecycle.Lifecycle;
import androidx.lifecycle.OnLifecycleEvent;
/**
 * @deprecated use {@link OnLifecycleEvent(Lifecycle.Event)}
 */
@Deprecated
public interface OnResume {
    void onResume();
}
```

## OnSaveInstanceState


```java

package com.android.settingslib.core.lifecycle.events;
import android.os.Bundle;
public interface OnSaveInstanceState {
    void onSaveInstanceState(Bundle outState);
}
```

## OnStart


```java

package com.android.settingslib.core.lifecycle.events;
import androidx.lifecycle.Lifecycle;
import androidx.lifecycle.OnLifecycleEvent;
/**
 * @deprecated use {@link OnLifecycleEvent(Lifecycle.Event) }
 */
@Deprecated
public interface OnStart {
    void onStart();
}
```

## OnStop


```java

package com.android.settingslib.core.lifecycle.events;
import androidx.lifecycle.Lifecycle;
import androidx.lifecycle.OnLifecycleEvent;
/**
 * @deprecated use {@link OnLifecycleEvent(Lifecycle.Event) }
 */
@Deprecated
public interface OnStop {
    void onStop();
}
```

## SetPreferenceScreen


```java

package com.android.settingslib.core.lifecycle.events;
import androidx.preference.PreferenceScreen;
public interface SetPreferenceScreen {
    void setPreferenceScreen(PreferenceScreen preferenceScreen);
}
```


