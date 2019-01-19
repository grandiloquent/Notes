# Java: Architect Android apps with MVP, Dagger, Retrofit & RxJava - RxJavaExample

## build


```java

apply plugin: 'com.android.application'
android {
    compileSdkVersion 25
    buildToolsVersion "25.0.2"
    defaultConfig {
        applicationId "com.tetraandroid.retrofitexample"
        minSdkVersion 16
        targetSdkVersion 25
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}
ext {
    retrofitVersion = '2.1.0'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
        exclude group: 'com.android.support', module: 'support-annotations'
    })
    compile 'com.android.support:appcompat-v7:25.1.1'
    compile 'com.android.support.constraint:constraint-layout:1.0.0-beta4'
    testCompile 'junit:junit:4.12'
    //rest interactions
    compile "com.squareup.retrofit2:retrofit:$retrofitVersion"
    //JSON Parsing
    compile "com.squareup.retrofit2:converter-gson:$retrofitVersion"
    compile 'com.google.code.gson:gson:2.6.2'
    //http logging
    compile 'com.squareup.okhttp3:logging-interceptor:3.4.2'
    //injection
    compile 'com.google.dagger:dagger:2.9'
    annotationProcessor 'com.google.dagger:dagger-compiler:2.9'
    provided 'javax.annotation:jsr250-api:1.0'
    //RxJava
    compile 'io.reactivex:rxandroid:1.2.1'
    compile 'io.reactivex:rxjava:1.2.6'
    compile "com.squareup.retrofit2:adapter-rxjava:$retrofitVersion"
}
```

## ExampleInstrumentedTest


```java

package com.tetraandroid.retrofitexample;
import android.content.Context;
import android.support.test.InstrumentationRegistry;
import android.support.test.runner.AndroidJUnit4;
import org.junit.Test;
import org.junit.runner.RunWith;
import static org.junit.Assert.*;
/**
 * Instrumentation test, which will execute on an Android device.
 *
 * @see <a href="http://d.android.com/tools/testing">Testing documentation</a>
 */
@RunWith(AndroidJUnit4.class)
public class ExampleInstrumentedTest {
    @Test
    public void useAppContext() throws Exception {
        // Context of the app under test.
        Context appContext = InstrumentationRegistry.getTargetContext();
        assertEquals("com.tetraandroid.retrofitexample", appContext.getPackageName());
    }
}
```

## AndroidManifest


```java

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="com.tetraandroid.retrofitexample">
  <uses-permission android:name="android.permission.INTERNET"></uses-permission>
  <application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:name="com.tetraandroid.rxjavaexample.root.App"
    android:supportsRtl="true"
    android:theme="@style/AppTheme">
    <activity android:name="com.tetraandroid.rxjavaexample.MainActivity">
      <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
      </intent-filter>
    </activity>
  </application>
</manifest>
```

## MainActivity


```java

package com.tetraandroid.rxjavaexample;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import com.tetraandroid.retrofitexample.R;
import com.tetraandroid.rxjavaexample.http.TwitchAPI;
import com.tetraandroid.rxjavaexample.http.apimodel.Top;
import com.tetraandroid.rxjavaexample.http.apimodel.Twitch;
import com.tetraandroid.rxjavaexample.root.App;
import javax.inject.Inject;
import java.util.List;
import retrofit2.Call;
import retrofit2.Callback;
import retrofit2.Response;
import rx.Observable;
import rx.Observer;
import rx.Subscriber;
import rx.android.schedulers.AndroidSchedulers;
import rx.functions.Func1;
import rx.schedulers.Schedulers;
public class MainActivity extends AppCompatActivity {
    @Inject
    TwitchAPI twitchAPI;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ((App)getApplication()).getComponent().inject(this);
        Call<Twitch> call = twitchAPI.getTopGames("replace_here_the_client_id_generated_by_the_twitch_api");
        call.enqueue(new Callback<Twitch>() {
            @Override
            public void onResponse(Call<Twitch> call, Response<Twitch> response) {
                List<Top> gameList = response.body().getTop();
                for (Top top : gameList){
                    System.out.println(top.getGame().getName());
                }
            }
            @Override
            public void onFailure(Call<Twitch> call, Throwable t) {
                     t.printStackTrace();
            }
        });
        twitchAPI.getTopGamesObservable("replace_here_the_client_id_generated_by_the_twitch_api").flatMap(new Func1<Twitch, Observable<Top>>() {
            @Override
            public Observable<Top> call(Twitch twitch) {
                return Observable.from(twitch.getTop());
            }
        }).flatMap(new Func1<Top, Observable<String>>() {
            @Override
            public Observable<String> call(Top top) {
                return Observable.just(top.getGame().getName());
            }
        }).subscribeOn(Schedulers.io()).observeOn(AndroidSchedulers.mainThread()).subscribe(new Observer<String>() {
            @Override
            public void onCompleted() {
            }
            @Override
            public void onError(Throwable e) {
            }
            @Override
            public void onNext(String s) {
                System.out.println("From rx java: " + s);
            }
        });
       twitchAPI.getTopGamesObservable("replace_here_the_client_id_generated_by_the_twitch_api").flatMap(new Func1<Twitch, Observable<Top>>() {
            @Override
            public Observable<Top> call(Twitch twitch) {
                return Observable.from(twitch.getTop());
            }
        }).flatMap(new Func1<Top, Observable<Integer>>() {
            @Override
            public Observable<Integer> call(Top top) {
                return Observable.just(top.getGame().getPopularity());
            }
        }).subscribeOn(Schedulers.io()).observeOn(AndroidSchedulers.mainThread()).subscribe(new Observer<Integer>() {
           @Override
           public void onCompleted() {
           }
           @Override
           public void onError(Throwable e) {
               e.printStackTrace();
           }
           @Override
           public void onNext(Integer integer) {
               System.out.println("From rx java: Popularity is " + integer.toString());
           }
       });
        twitchAPI.getTopGamesObservable("replace_here_the_client_id_generated_by_the_twitch_api").flatMap(new Func1<Twitch, Observable<Top>>() {
            @Override
            public Observable<Top> call(Twitch twitch) {
                return Observable.from(twitch.getTop());
            }
        }).flatMap(new Func1<Top, Observable<String>>() {
            @Override
            public Observable<String> call(Top top) {
                return Observable.just(top.getGame().getName());
            }
        }).filter(new Func1<String, Boolean>() {
            @Override
            public Boolean call(String s) {
                return s.startsWith("H");
            }
        }).subscribeOn(Schedulers.io()).observeOn(AndroidSchedulers.mainThread()).subscribe(new Subscriber<String>() {
            @Override
            public void onCompleted() {
            }
            @Override
            public void onError(Throwable e) {
            }
            @Override
            public void onNext(String s) {
                System.out.println("From rx java with filter: " + s);
            }
        });
    }
}
```

## ApiModule


```java

package com.tetraandroid.rxjavaexample.http;
import dagger.Module;
import dagger.Provides;
import okhttp3.OkHttpClient;
import okhttp3.logging.HttpLoggingInterceptor;
import retrofit2.Retrofit;
import retrofit2.adapter.rxjava.RxJavaCallAdapterFactory;
import retrofit2.converter.gson.GsonConverterFactory;
@Module
public class ApiModule {
    public final String BASE_URL = "https://api.twitch.tv/kraken/";
    @Provides
    public OkHttpClient provideClient() {
        HttpLoggingInterceptor interceptor = new HttpLoggingInterceptor();
        interceptor.setLevel(HttpLoggingInterceptor.Level.BODY);
        return new OkHttpClient.Builder().addInterceptor(interceptor).build();
    }
    @Provides
    public Retrofit provideRetrofit(String baseURL, OkHttpClient client) {
        return new Retrofit.Builder()
                .baseUrl(baseURL)
                .client(client)
                .addConverterFactory(GsonConverterFactory.create())
                .addCallAdapterFactory(RxJavaCallAdapterFactory.create())
                .build();
    }
    @Provides
    public TwitchAPI provideApiService() {
        return provideRetrofit(BASE_URL, provideClient()).create(TwitchAPI.class);
    }
}
```

## TwitchAPI


```java

package com.tetraandroid.rxjavaexample.http;
import com.tetraandroid.rxjavaexample.http.apimodel.Twitch;
import retrofit2.Call;
import retrofit2.http.GET;
import retrofit2.http.Header;
import rx.Observable;
public interface TwitchAPI {
    @GET("games/top")
    Call<Twitch> getTopGames(@Header("Client-Id") String clientId);
    @GET("games/top")
    Observable <Twitch> getTopGamesObservable(@Header("Client-Id") String clientId);
}
```

## Box


```java

package com.tetraandroid.rxjavaexample.http.apimodel;
import com.google.gson.annotations.Expose;
import com.google.gson.annotations.SerializedName;
import javax.annotation.Generated;
@Generated("org.jsonschema2pojo")
public class Box {
    @SerializedName("large")
    @Expose
    private String large;
    @SerializedName("medium")
    @Expose
    private String medium;
    @SerializedName("small")
    @Expose
    private String small;
    @SerializedName("template")
    @Expose
    private String template;
    /**
     *
     * @return
     *     The large
     */
    public String getLarge() {
        return large;
    }
    /**
     *
     * @param large
     *     The large
     */
    public void setLarge(String large) {
        this.large = large;
    }
    /**
     *
     * @return
     *     The medium
     */
    public String getMedium() {
        return medium;
    }
    /**
     *
     * @param medium
     *     The medium
     */
    public void setMedium(String medium) {
        this.medium = medium;
    }
    /**
     *
     * @return
     *     The small
     */
    public String getSmall() {
        return small;
    }
    /**
     *
     * @param small
     *     The small
     */
    public void setSmall(String small) {
        this.small = small;
    }
    /**
     *
     * @return
     *     The template
     */
    public String getTemplate() {
        return template;
    }
    /**
     *
     * @param template
     *     The template
     */
    public void setTemplate(String template) {
        this.template = template;
    }
}
```

## Game


```java

package com.tetraandroid.rxjavaexample.http.apimodel;
import com.google.gson.annotations.Expose;
import com.google.gson.annotations.SerializedName;
import javax.annotation.Generated;
@Generated("org.jsonschema2pojo")
public class Game {
    @SerializedName("name")
    @Expose
    private String name;
    @SerializedName("popularity")
    @Expose
    private Integer popularity;
    @SerializedName("_id")
    @Expose
    private Integer id;
    @SerializedName("giantbomb_id")
    @Expose
    private Integer giantbombId;
    @SerializedName("box")
    @Expose
    private Box box;
    @SerializedName("logo")
    @Expose
    private Logo logo;
    @SerializedName("_links")
    @Expose
    private Links_ links;
    /**
     *
     * @return
     *     The name
     */
    public String getName() {
        return name;
    }
    /**
     *
     * @param name
     *     The name
     */
    public void setName(String name) {
        this.name = name;
    }
    /**
     *
     * @return
     *     The popularity
     */
    public Integer getPopularity() {
        return popularity;
    }
    /**
     *
     * @param popularity
     *     The popularity
     */
    public void setPopularity(Integer popularity) {
        this.popularity = popularity;
    }
    /**
     *
     * @return
     *     The id
     */
    public Integer getId() {
        return id;
    }
    /**
     *
     * @param id
     *     The _id
     */
    public void setId(Integer id) {
        this.id = id;
    }
    /**
     *
     * @return
     *     The giantbombId
     */
    public Integer getGiantbombId() {
        return giantbombId;
    }
    /**
     *
     * @param giantbombId
     *     The giantbomb_id
     */
    public void setGiantbombId(Integer giantbombId) {
        this.giantbombId = giantbombId;
    }
    /**
     *
     * @return
     *     The box
     */
    public Box getBox() {
        return box;
    }
    /**
     *
     * @param box
     *     The box
     */
    public void setBox(Box box) {
        this.box = box;
    }
    /**
     *
     * @return
     *     The logo
     */
    public Logo getLogo() {
        return logo;
    }
    /**
     *
     * @param logo
     *     The logo
     */
    public void setLogo(Logo logo) {
        this.logo = logo;
    }
    /**
     *
     * @return
     *     The links
     */
    public Links_ getLinks() {
        return links;
    }
    /**
     *
     * @param links
     *     The _links
     */
    public void setLinks(Links_ links) {
        this.links = links;
    }
}
```

## Links


```java

package com.tetraandroid.rxjavaexample.http.apimodel;
import javax.annotation.Generated;
import com.google.gson.annotations.Expose;
import com.google.gson.annotations.SerializedName;
@Generated("org.jsonschema2pojo")
public class Links {
    @SerializedName("self")
    @Expose
    private String self;
    @SerializedName("next")
    @Expose
    private String next;
    /**
     *
     * @return
     *     The self
     */
    public String getSelf() {
        return self;
    }
    /**
     *
     * @param self
     *     The self
     */
    public void setSelf(String self) {
        this.self = self;
    }
    /**
     *
     * @return
     *     The next
     */
    public String getNext() {
        return next;
    }
    /**
     *
     * @param next
     *     The next
     */
    public void setNext(String next) {
        this.next = next;
    }
}
```

## Links_


```java

package com.tetraandroid.rxjavaexample.http.apimodel;
import javax.annotation.Generated;
@Generated("org.jsonschema2pojo")
public class Links_ {
}
```

## Logo


```java

package com.tetraandroid.rxjavaexample.http.apimodel;
import javax.annotation.Generated;
import com.google.gson.annotations.Expose;
import com.google.gson.annotations.SerializedName;
@Generated("org.jsonschema2pojo")
public class Logo {
    @SerializedName("large")
    @Expose
    private String large;
    @SerializedName("medium")
    @Expose
    private String medium;
    @SerializedName("small")
    @Expose
    private String small;
    @SerializedName("template")
    @Expose
    private String template;
    /**
     *
     * @return
     *     The large
     */
    public String getLarge() {
        return large;
    }
    /**
     *
     * @param large
     *     The large
     */
    public void setLarge(String large) {
        this.large = large;
    }
    /**
     *
     * @return
     *     The medium
     */
    public String getMedium() {
        return medium;
    }
    /**
     *
     * @param medium
     *     The medium
     */
    public void setMedium(String medium) {
        this.medium = medium;
    }
    /**
     *
     * @return
     *     The small
     */
    public String getSmall() {
        return small;
    }
    /**
     *
     * @param small
     *     The small
     */
    public void setSmall(String small) {
        this.small = small;
    }
    /**
     *
     * @return
     *     The template
     */
    public String getTemplate() {
        return template;
    }
    /**
     *
     * @param template
     *     The template
     */
    public void setTemplate(String template) {
        this.template = template;
    }
}
```

## Top


```java

package com.tetraandroid.rxjavaexample.http.apimodel;
import javax.annotation.Generated;
import com.google.gson.annotations.Expose;
import com.google.gson.annotations.SerializedName;
@Generated("org.jsonschema2pojo")
public class Top {
    @SerializedName("game")
    @Expose
    private Game game;
    @SerializedName("viewers")
    @Expose
    private Integer viewers;
    @SerializedName("channels")
    @Expose
    private Integer channels;
    /**
     *
     * @return
     *     The game
     */
    public Game getGame() {
        return game;
    }
    /**
     *
     * @param game
     *     The game
     */
    public void setGame(Game game) {
        this.game = game;
    }
    /**
     *
     * @return
     *     The viewers
     */
    public Integer getViewers() {
        return viewers;
    }
    /**
     *
     * @param viewers
     *     The viewers
     */
    public void setViewers(Integer viewers) {
        this.viewers = viewers;
    }
    /**
     *
     * @return
     *     The channels
     */
    public Integer getChannels() {
        return channels;
    }
    /**
     *
     * @param channels
     *     The channels
     */
    public void setChannels(Integer channels) {
        this.channels = channels;
    }
}
```

## Twitch


```java

package com.tetraandroid.rxjavaexample.http.apimodel;
import com.google.gson.annotations.Expose;
import com.google.gson.annotations.SerializedName;
import javax.annotation.Generated;
import java.util.ArrayList;
import java.util.List;
@Generated("org.jsonschema2pojo")
public class Twitch {
    @SerializedName("_total")
    @Expose
    private Integer total;
    @SerializedName("_links")
    @Expose
    private Links links;
    @SerializedName("top")
    @Expose
    private List<Top> top = new ArrayList<Top>();
    /**
     *
     * @return
     *     The total
     */
    public Integer getTotal() {
        return total;
    }
    /**
     *
     * @param total
     *     The _total
     */
    public void setTotal(Integer total) {
        this.total = total;
    }
    /**
     *
     * @return
     *     The links
     */
    public Links getLinks() {
        return links;
    }
    /**
     *
     * @param links
     *     The _links
     */
    public void setLinks(Links links) {
        this.links = links;
    }
    /**
     *
     * @return
     *     The top
     */
    public List<Top> getTop() {
        return top;
    }
    /**
     *
     * @param top
     *     The top
     */
    public void setTop(List<Top> top) {
        this.top = top;
    }
}
```

## App


```java

package com.tetraandroid.rxjavaexample.root;
import android.app.Application;
import com.tetraandroid.rxjavaexample.http.ApiModule;
public class App extends Application {
    private ApplicationComponent component;
    @Override
    public void onCreate() {
        super.onCreate();
        component = DaggerApplicationComponent.builder()
                .applicationModule(new ApplicationModule(this))
                .apiModule(new ApiModule())
                .build();
    }
    public ApplicationComponent getComponent() {
        return component;
    }
}
```

## ApplicationComponent


```java

package com.tetraandroid.rxjavaexample.root;
import com.tetraandroid.rxjavaexample.MainActivity;
import com.tetraandroid.rxjavaexample.http.ApiModule;
import javax.inject.Singleton;
import dagger.Component;
@Singleton
@Component(modules = {ApplicationModule.class, ApiModule.class})
public interface ApplicationComponent {
    void inject(MainActivity target);
}
```

## ApplicationModule


```java

package com.tetraandroid.rxjavaexample.root;
import android.app.Application;
import android.content.Context;
import javax.inject.Singleton;
import dagger.Module;
import dagger.Provides;
@Module
public class ApplicationModule {
    private Application application;
    public ApplicationModule(Application application) {
        this.application = application;
    }
    @Provides
    @Singleton
    public Context provideContext() {
        return application;
    }
}
```

## activity_main


```java

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
  xmlns:app="http://schemas.android.com/apk/res-auto"
  xmlns:tools="http://schemas.android.com/tools"
  android:id="@+id/activity_main"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  tools:context=".MainActivity">
  <TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Hello World!"
    app:layout_constraintBottom_toBottomOf="@+id/activity_main"
    app:layout_constraintLeft_toLeftOf="@+id/activity_main"
    app:layout_constraintRight_toRightOf="@+id/activity_main"
    app:layout_constraintTop_toTopOf="@+id/activity_main" />
</android.support.constraint.ConstraintLayout>
```

## colors


```java

<?xml version="1.0" encoding="utf-8"?>
<resources>
  <color name="colorPrimary">#3F51B5</color>
  <color name="colorPrimaryDark">#303F9F</color>
  <color name="colorAccent">#FF4081</color>
</resources>
```

## dimens


```java

<resources>
  <!-- Default screen margins, per the Android Design guidelines. -->
  <dimen name="activity_horizontal_margin">16dp</dimen>
  <dimen name="activity_vertical_margin">16dp</dimen>
</resources>
```

## strings


```java

<resources>
  <string name="app_name">RetrofitExample</string>
</resources>
```

## styles


```java

<resources>
  <!-- Base application theme. -->
  <style name="AppTheme"
    parent="Theme.AppCompat.Light.DarkActionBar">
    <!-- Customize your theme here. -->
    <item name="colorPrimary">@color/colorPrimary</item>
    <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
    <item name="colorAccent">@color/colorAccent</item>
  </style>
</resources>
```

## dimens


```java

<resources>
  <!-- Example customization of dimensions originally defined in res/values/dimens.xml
         (such as screen margins) for screens with more than 820dp of available width. This
         would include 7" and 10" devices in landscape (~960dp and ~1280dp respectively). -->
  <dimen name="activity_horizontal_margin">64dp</dimen>
</resources>
```

## ExampleUnitTest


```java

package com.tetraandroid.retrofitexample;
import org.junit.Test;
import static org.junit.Assert.*;
/**
 * Example local unit test, which will execute on the development machine (host).
 *
 * @see <a href="http://d.android.com/tools/testing">Testing documentation</a>
 */
public class ExampleUnitTest {
    @Test
    public void addition_isCorrect() throws Exception {
        assertEquals(4, 2 + 2);
    }
}
```

