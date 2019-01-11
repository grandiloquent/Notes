# Java: Lifecycle

## BindingFragment


```java

package euphoria.psycho.rxlifecycle;
import android.app.Activity;
import android.app.Fragment;
import android.content.Context;
import android.os.Build;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import androidx.annotation.Nullable;
import androidx.annotation.RequiresApi;
import io.reactivex.subjects.BehaviorSubject;
/**
 * @author nekocode (nekocode.cn@gmail.com)
 */
@RequiresApi(api = Build.VERSION_CODES.HONEYCOMB)
public class BindingFragment extends Fragment {
    private final BehaviorSubject<LifecycleEvent> mLifecycleBehavior = BehaviorSubject.create();
    public BindingFragment() {
    }
    public BehaviorSubject<LifecycleEvent> getLifecycleBehavior() {
        return mLifecycleBehavior;
    }
    @Override
    public void onAttach(Context context) {
        super.onAttach(context);
        mLifecycleBehavior.onNext(LifecycleEvent.ATTACH);
    }
    @Override
    public void onAttach(Activity activity) {
        super.onAttach(activity);
        mLifecycleBehavior.onNext(LifecycleEvent.ATTACH);
    }
    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        mLifecycleBehavior.onNext(LifecycleEvent.CREATE);
    }
    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        mLifecycleBehavior.onNext(LifecycleEvent.CREATE_VIEW);
        return null;
    }
    @Override
    public void onStart() {
        super.onStart();
        mLifecycleBehavior.onNext(LifecycleEvent.START);
    }
    @Override
    public void onResume() {
        super.onResume();
        mLifecycleBehavior.onNext(LifecycleEvent.RESUME);
    }
    @Override
    public void onPause() {
        super.onPause();
        mLifecycleBehavior.onNext(LifecycleEvent.PAUSE);
    }
    @Override
    public void onStop() {
        super.onStop();
        mLifecycleBehavior.onNext(LifecycleEvent.STOP);
    }
    @Override
    public void onDestroyView() {
        super.onDestroyView();
        mLifecycleBehavior.onNext(LifecycleEvent.DESTROY_VIEW);
    }
    @Override
    public void onDestroy() {
        super.onDestroy();
        mLifecycleBehavior.onNext(LifecycleEvent.DESTROY);
    }
    @Override
    public void onDetach() {
        super.onDetach();
        mLifecycleBehavior.onNext(LifecycleEvent.DETACH);
    }
}
```

## LifecycleEvent


```java

package euphoria.psycho.rxlifecycle;
/**
 * @author nekocode (nekocode.cn@gmail.com)
 */
public enum LifecycleEvent {
    ATTACH(COUNTER.getAndIncrease()),
    CREATE(COUNTER.getAndIncrease()),
    CREATE_VIEW(COUNTER.getAndIncrease()),
    START(COUNTER.getAndIncrease()),
    RESUME(COUNTER.getAndIncrease()),
    PAUSE(COUNTER.getAndIncrease()),
    STOP(COUNTER.getAndIncrease()),
    DESTROY_VIEW(COUNTER.getAndIncrease()),
    DESTROY(COUNTER.getAndIncrease()),
    DETACH(COUNTER.getAndIncrease()),
    DISPOSE(COUNTER.getAndIncrease()),
    ;
    private static class COUNTER {
        static int i = 0;
        static int getAndIncrease() {
            return i++;
        }
    }
    private final int i;
    LifecycleEvent(int i) {
        this.i = i;
    }
    public int compare(LifecycleEvent other) {
        return i -  other.i;
    }
}
```

## RxLifecycle


```java

package euphoria.psycho.rxlifecycle;
import android.app.Activity;
import android.app.Fragment;
import android.app.FragmentManager;
import android.app.FragmentTransaction;
import android.os.Build;
import androidx.annotation.NonNull;
import androidx.annotation.RequiresApi;
import euphoria.psycho.rxlifecycle.transformer.BindLifecycleCompletableTransformer;
import euphoria.psycho.rxlifecycle.transformer.BindLifecycleFlowableTransformer;
import euphoria.psycho.rxlifecycle.transformer.BindLifecycleMaybeTransformer;
import euphoria.psycho.rxlifecycle.transformer.BindLifecycleObservableTransformer;
import euphoria.psycho.rxlifecycle.transformer.BindLifecycleSingleTransformer;
import io.reactivex.BackpressureStrategy;
import io.reactivex.CompletableTransformer;
import io.reactivex.Flowable;
import io.reactivex.FlowableTransformer;
import io.reactivex.MaybeTransformer;
import io.reactivex.Observable;
import io.reactivex.ObservableTransformer;
import io.reactivex.SingleTransformer;
/**
 * @author nekocode (nekocode.cn@gmail.com)
 */
public class RxLifecycle {
    private static final String FRAGMENT_TAG = "_BINDING_FRAGMENT_";
    private final Observable<LifecycleEvent> mLifecycleObservable;
    public static RxLifecycle bind(@NonNull Observable<LifecycleEvent> lifecycleObservable) {
        return new RxLifecycle(lifecycleObservable);
    }
    @RequiresApi(api = Build.VERSION_CODES.HONEYCOMB)
    public static RxLifecycle bind(@NonNull Activity targetActivity) {
        return bind(targetActivity.getFragmentManager());
    }
    @RequiresApi(api = Build.VERSION_CODES.JELLY_BEAN_MR1)
    public static RxLifecycle bind(@NonNull Fragment targetFragment) {
        return bind(targetFragment.getChildFragmentManager());
    }
    @RequiresApi(api = Build.VERSION_CODES.HONEYCOMB)
    public static RxLifecycle bind(@NonNull FragmentManager fragmentManager) {
        BindingFragment fragment = (BindingFragment) fragmentManager.findFragmentByTag(FRAGMENT_TAG);
        if (fragment == null) {
            fragment = new BindingFragment();
            final FragmentTransaction transaction = fragmentManager.beginTransaction();
            transaction.add(fragment, FRAGMENT_TAG);
            transaction.commit();
        } else if (Build.VERSION.SDK_INT >= 13 && fragment.isDetached()) {
            final FragmentTransaction transaction = fragmentManager.beginTransaction();
            transaction.attach(fragment);
            transaction.commit();
        }
        return bind(fragment.getLifecycleBehavior());
    }
    private RxLifecycle(Observable<LifecycleEvent> lifecycleObservable) {
        this.mLifecycleObservable = lifecycleObservable;
    }
    public Flowable<LifecycleEvent> toFlowable(BackpressureStrategy strategy) {
        return mLifecycleObservable.toFlowable(strategy);
    }
    public Observable<LifecycleEvent> toObservable() {
        return mLifecycleObservable;
    }
    public <T> FlowableTransformer<T, T> cancelFlowableWhen(LifecycleEvent event) {
        return new BindLifecycleFlowableTransformer<T>(mLifecycleObservable, event);
    }
    public <T> ObservableTransformer<T, T> disposeObservableWhen(LifecycleEvent event) {
        return new BindLifecycleObservableTransformer<T>(mLifecycleObservable, event);
    }
    public CompletableTransformer disposeCompletableWhen(LifecycleEvent event) {
        return new BindLifecycleCompletableTransformer(mLifecycleObservable, event);
    }
    public <T> SingleTransformer<T, T> disposeSingleWhen(LifecycleEvent event) {
        return new BindLifecycleSingleTransformer<T>(mLifecycleObservable, event);
    }
    public <T> MaybeTransformer<T, T> disposeMaybeWhen(LifecycleEvent event) {
        return new BindLifecycleMaybeTransformer<T>(mLifecycleObservable, event);
    }
}
```

## AbstractBindLifecycleTransformer


```java

package euphoria.psycho.rxlifecycle.transformer;
import androidx.annotation.NonNull;
import euphoria.psycho.rxlifecycle.LifecycleEvent;
import io.reactivex.Completable;
import io.reactivex.CompletableSource;
import io.reactivex.Observable;
import io.reactivex.functions.Function;
import io.reactivex.functions.Predicate;
/**
 * @author nekocode (nekocode.cn@gmail.com)
 */
abstract class AbstractBindLifecycleTransformer {
    private final Observable<LifecycleEvent> mLifecycleObservable;
    private final LifecycleEvent mEvent;
    AbstractBindLifecycleTransformer(
            @NonNull Observable<LifecycleEvent> lifecycleObservable,
            @NonNull LifecycleEvent event) {
        this.mLifecycleObservable = lifecycleObservable;
        this.mEvent = event;
    }
    Completable receiveEventCompletable() {
        return mLifecycleObservable
                .filter(new Predicate<LifecycleEvent>() {
                    @Override
                    public boolean test(LifecycleEvent e) throws Exception {
                        return e.compare(mEvent) >= 0;
                    }
                })
                .take(1)
                .flatMapCompletable(new Function<LifecycleEvent, CompletableSource>() {
                    @Override
                    public CompletableSource apply(LifecycleEvent event) throws Exception {
                        return Completable.complete();
                    }
                });
    }
}
```

## BindLifecycleCompletableTransformer


```java

package euphoria.psycho.rxlifecycle.transformer;
import androidx.annotation.NonNull;
import euphoria.psycho.rxlifecycle.LifecycleEvent;
import io.reactivex.Completable;
import io.reactivex.CompletableObserver;
import io.reactivex.CompletableSource;
import io.reactivex.CompletableTransformer;
import io.reactivex.Observable;
import io.reactivex.disposables.Disposable;
import io.reactivex.internal.disposables.ArrayCompositeDisposable;
/**
 * @author nekocode (nekocode.cn@gmail.com)
 */
public class BindLifecycleCompletableTransformer<T> extends AbstractBindLifecycleTransformer
        implements CompletableTransformer {
    public BindLifecycleCompletableTransformer(
            @NonNull Observable<LifecycleEvent> lifecycleObservable,
            @NonNull LifecycleEvent event) {
        super(lifecycleObservable, event);
    }
    @Override
    public CompletableSource apply(Completable upstream) {
        return new BindLifecycleCompletable(upstream);
    }
    private class BindLifecycleCompletable extends Completable {
        private final CompletableSource mUpstream;
        private BindLifecycleCompletable(CompletableSource upstream) {
            this.mUpstream = upstream;
        }
        @Override
        protected void subscribeActual(final CompletableObserver downstream) {
            final ArrayCompositeDisposable frc = new ArrayCompositeDisposable(2);
            downstream.onSubscribe(frc);
            receiveEventCompletable()
                    .subscribe(new CompletableObserver() {
                        @Override
                        public void onSubscribe(Disposable d) {
                            frc.setResource(0, d);
                        }
                        @Override
                        public void onComplete() {
                            frc.dispose();
                        }
                        @Override
                        public void onError(Throwable e) {
                            frc.dispose();
                        }
                    });
            mUpstream.subscribe(new CompletableObserver() {
                @Override
                public void onSubscribe(Disposable d) {
                    frc.setResource(1, d);
                }
                @Override
                public void onComplete() {
                    frc.dispose();
                    downstream.onComplete();
                }
                @Override
                public void onError(Throwable e) {
                    frc.dispose();
                    downstream.onError(e);
                }
            });
        }
    }
}
```

## BindLifecycleFlowableTransformer


```java

package euphoria.psycho.rxlifecycle.transformer;
import androidx.annotation.NonNull;
import euphoria.psycho.rxlifecycle.LifecycleEvent;
import org.reactivestreams.Publisher;
import org.reactivestreams.Subscriber;
import org.reactivestreams.Subscription;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.atomic.AtomicLong;
import java.util.concurrent.atomic.AtomicReference;
import io.reactivex.CompletableObserver;
import io.reactivex.Flowable;
import io.reactivex.FlowableTransformer;
import io.reactivex.Observable;
import io.reactivex.disposables.Disposable;
import io.reactivex.internal.disposables.DisposableHelper;
import io.reactivex.internal.subscriptions.SubscriptionHelper;
import io.reactivex.internal.util.AtomicThrowable;
import io.reactivex.internal.util.HalfSerializer;
/**
 * @author nekocode (nekocode.cn@gmail.com)
 */
public class BindLifecycleFlowableTransformer<T> extends AbstractBindLifecycleTransformer
        implements FlowableTransformer<T, T> {
    public BindLifecycleFlowableTransformer(
            @NonNull Observable<LifecycleEvent> lifecycleObservable,
            @NonNull LifecycleEvent event) {
        super(lifecycleObservable, event);
    }
    @Override
    public Publisher<T> apply(final Flowable<T> upstream) {
        return new BindLifecycleFlowable(upstream);
    }
    private class BindLifecycleFlowable extends Flowable<T> {
        private final Publisher<T> mUpstream;
        private BindLifecycleFlowable(Publisher<T> upstream) {
            this.mUpstream = upstream;
        }
        @Override
        protected void subscribeActual(final Subscriber<? super T> downstream) {
            final MainSubscriber mainSubscriber = new MainSubscriber(downstream);
            downstream.onSubscribe(mainSubscriber);
            receiveEventCompletable().subscribe(mainSubscriber.other);
            mUpstream.subscribe(mainSubscriber);
        }
        final class MainSubscriber extends AtomicInteger implements Subscriber<T>, Subscription {
            private static final long serialVersionUID = 919611990130321642L;
            final Subscriber<? super T> actual;
            final OtherObserver other;
            final AtomicReference<Subscription> s;
            final AtomicLong requested;
            final AtomicThrowable error;
            MainSubscriber(Subscriber<? super T> actual) {
                this.actual = actual;
                this.requested = new AtomicLong();
                this.s = new AtomicReference<>();
                this.other = new OtherObserver();
                this.error = new AtomicThrowable();
            }
            @Override
            public void onSubscribe(Subscription s) {
                SubscriptionHelper.deferredSetOnce(this.s, requested, s);
            }
            @Override
            public void onNext(T t) {
                HalfSerializer.onNext(actual, t, this, error);
            }
            @Override
            public void onError(Throwable t) {
                DisposableHelper.dispose(other);
                HalfSerializer.onError(actual, t, this, error);
            }
            @Override
            public void onComplete() {
                DisposableHelper.dispose(other);
                HalfSerializer.onComplete(actual, this, error);
            }
            @Override
            public void request(long n) {
                SubscriptionHelper.deferredRequest(s, requested, n);
            }
            @Override
            public void cancel() {
                SubscriptionHelper.cancel(s);
                DisposableHelper.dispose(other);
            }
            final class OtherObserver extends AtomicReference<Disposable> implements CompletableObserver {
                private static final long serialVersionUID = -6684536082750051972L;
                @Override
                public void onSubscribe(Disposable d) {
                    DisposableHelper.setOnce(this, d);
                }
                @Override
                public void onError(Throwable e) {
                    DisposableHelper.dispose(this);
                    SubscriptionHelper.cancel(s);
                }
                @Override
                public void onComplete() {
                    DisposableHelper.dispose(this);
                    SubscriptionHelper.cancel(s);
                }
            }
        }
    }
}
```

## BindLifecycleMaybeTransformer


```java

package euphoria.psycho.rxlifecycle.transformer;
import androidx.annotation.NonNull;
import euphoria.psycho.rxlifecycle.LifecycleEvent;
import io.reactivex.CompletableObserver;
import io.reactivex.Maybe;
import io.reactivex.MaybeObserver;
import io.reactivex.MaybeSource;
import io.reactivex.MaybeTransformer;
import io.reactivex.Observable;
import io.reactivex.disposables.Disposable;
import io.reactivex.internal.disposables.ArrayCompositeDisposable;
/**
 * @author nekocode (nekocode.cn@gmail.com)
 */
public class BindLifecycleMaybeTransformer<T> extends AbstractBindLifecycleTransformer
        implements MaybeTransformer<T, T> {
    public BindLifecycleMaybeTransformer(
            @NonNull Observable<LifecycleEvent> lifecycleObservable,
            @NonNull LifecycleEvent event) {
        super(lifecycleObservable, event);
    }
    @Override
    public MaybeSource<T> apply(Maybe<T> upstream) {
        return new BindLifecycleMaybe(upstream);
    }
    private class BindLifecycleMaybe extends Maybe<T> {
        private final MaybeSource<T> mUpstream;
        private BindLifecycleMaybe(MaybeSource<T> upstream) {
            this.mUpstream = upstream;
        }
        @Override
        protected void subscribeActual(final MaybeObserver<? super T> downstream) {
            final ArrayCompositeDisposable frc = new ArrayCompositeDisposable(2);
            downstream.onSubscribe(frc);
            receiveEventCompletable()
                    .subscribe(new CompletableObserver() {
                        @Override
                        public void onSubscribe(Disposable d) {
                            frc.setResource(0, d);
                        }
                        @Override
                        public void onComplete() {
                            frc.dispose();
                        }
                        @Override
                        public void onError(Throwable e) {
                            frc.dispose();
                        }
                    });
            mUpstream.subscribe(new MaybeObserver<T>() {
                @Override
                public void onSubscribe(Disposable d) {
                    frc.setResource(1, d);
                }
                @Override
                public void onSuccess(T t) {
                    frc.dispose();
                    downstream.onSuccess(t);
                }
                @Override
                public void onComplete() {
                    frc.dispose();
                    downstream.onComplete();
                }
                @Override
                public void onError(Throwable e) {
                    frc.dispose();
                    downstream.onError(e);
                }
            });
        }
    }
}
```

## BindLifecycleObservableTransformer


```java

package euphoria.psycho.rxlifecycle.transformer;
import androidx.annotation.NonNull;
import euphoria.psycho.rxlifecycle.LifecycleEvent;
import io.reactivex.CompletableObserver;
import io.reactivex.Observable;
import io.reactivex.ObservableSource;
import io.reactivex.ObservableTransformer;
import io.reactivex.Observer;
import io.reactivex.disposables.Disposable;
import io.reactivex.internal.disposables.ArrayCompositeDisposable;
/**
 * @author nekocode (nekocode.cn@gmail.com)
 */
public class BindLifecycleObservableTransformer<T> extends AbstractBindLifecycleTransformer
        implements ObservableTransformer<T, T> {
    public BindLifecycleObservableTransformer(
            @NonNull Observable<LifecycleEvent> lifecycleObservable,
            @NonNull LifecycleEvent event) {
        super(lifecycleObservable, event);
    }
    @Override
    public ObservableSource<T> apply(Observable<T> upstream) {
        return new BindLifecycleObservable(upstream);
    }
    private class BindLifecycleObservable extends Observable<T> {
        private final ObservableSource<T> mUpstream;
        private BindLifecycleObservable(ObservableSource<T> upstream) {
            this.mUpstream = upstream;
        }
        @Override
        protected void subscribeActual(final Observer<? super T> downstream) {
            final ArrayCompositeDisposable frc = new ArrayCompositeDisposable(2);
            downstream.onSubscribe(frc);
            receiveEventCompletable()
                    .subscribe(new CompletableObserver() {
                        @Override
                        public void onSubscribe(Disposable d) {
                            frc.setResource(0, d);
                        }
                        @Override
                        public void onComplete() {
                            frc.dispose();
                        }
                        @Override
                        public void onError(Throwable e) {
                            frc.dispose();
                        }
                    });
            mUpstream.subscribe(new Observer<T>() {
                @Override
                public void onSubscribe(Disposable d) {
                    frc.setResource(1, d);
                }
                @Override
                public void onNext(T t) {
                    downstream.onNext(t);
                }
                @Override
                public void onComplete() {
                    frc.dispose();
                    downstream.onComplete();
                }
                @Override
                public void onError(Throwable e) {
                    frc.dispose();
                    downstream.onError(e);
                }
            });
        }
    }
}
```

## BindLifecycleSingleTransformer


```java

package euphoria.psycho.rxlifecycle.transformer;
import androidx.annotation.NonNull;
import euphoria.psycho.rxlifecycle.LifecycleEvent;
import io.reactivex.CompletableObserver;
import io.reactivex.Observable;
import io.reactivex.Single;
import io.reactivex.SingleObserver;
import io.reactivex.SingleSource;
import io.reactivex.SingleTransformer;
import io.reactivex.disposables.Disposable;
import io.reactivex.internal.disposables.ArrayCompositeDisposable;
/**
 * @author nekocode (nekocode.cn@gmail.com)
 */
public class BindLifecycleSingleTransformer<T> extends AbstractBindLifecycleTransformer
        implements SingleTransformer<T, T> {
    public BindLifecycleSingleTransformer(
            @NonNull Observable<LifecycleEvent> lifecycleObservable,
            @NonNull LifecycleEvent event) {
        super(lifecycleObservable, event);
    }
    @Override
    public SingleSource<T> apply(Single<T> upstream) {
        return new BindLifecycleSingle(upstream);
    }
    private class BindLifecycleSingle extends Single<T> {
        private final SingleSource<T> mUpstream;
        private BindLifecycleSingle(SingleSource<T> upstream) {
            this.mUpstream = upstream;
        }
        @Override
        protected void subscribeActual(final SingleObserver<? super T> downstream) {
            final ArrayCompositeDisposable frc = new ArrayCompositeDisposable(2);
            downstream.onSubscribe(frc);
            receiveEventCompletable()
                    .subscribe(new CompletableObserver() {
                        @Override
                        public void onSubscribe(Disposable d) {
                            frc.setResource(0, d);
                        }
                        @Override
                        public void onComplete() {
                            frc.dispose();
                        }
                        @Override
                        public void onError(Throwable e) {
                            frc.dispose();
                        }
                    });
            mUpstream.subscribe(new SingleObserver<T>() {
                @Override
                public void onSubscribe(Disposable d) {
                    frc.setResource(1, d);
                }
                @Override
                public void onSuccess(T t) {
                    frc.dispose();
                    downstream.onSuccess(t);
                }
                @Override
                public void onError(Throwable e) {
                    frc.dispose();
                    downstream.onError(e);
                }
            });
        }
    }
}
```


