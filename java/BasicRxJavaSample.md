# Java: BasicRxJavaSample

## Injection


```java

package com.example.android.observability;
import android.content.Context;
import com.example.android.observability.persistence.LocalUserDataSource;
import com.example.android.observability.persistence.UsersDatabase;
import com.example.android.observability.ui.ViewModelFactory;
/**
 * Enables injection of data sources.
 */
public class Injection {
    public static UserDataSource provideUserDataSource(Context context) {
        UsersDatabase database = UsersDatabase.getInstance(context);
        return new LocalUserDataSource(database.userDao());
    }
    public static ViewModelFactory provideViewModelFactory(Context context) {
        UserDataSource dataSource = provideUserDataSource(context);
        return new ViewModelFactory(dataSource);
    }
}
```

## UserDataSource


```java

package com.example.android.observability;
import com.example.android.observability.persistence.User;
import io.reactivex.Flowable;
/**
 * Access point for managing user data.
 */
public interface UserDataSource {
    /**
     * Gets the user from the data source.
     *
     * @return the user from the data source.
     */
    Flowable<User> getUser();
    /**
     * Inserts the user into the data source, or, if this is an existing user, updates it.
     *
     * @param user the user to be inserted or updated.
     */
    void insertOrUpdateUser(User user);
    /**
     * Deletes all users from the data source.
     */
    void deleteAllUsers();
}
```

## LocalUserDataSource


```java

package com.example.android.observability.persistence;
import com.example.android.observability.UserDataSource;
import io.reactivex.Flowable;
/**
 * Using the Room database as a data source.
 */
public class LocalUserDataSource implements UserDataSource {
    private final UserDao mUserDao;
    public LocalUserDataSource(UserDao userDao) {
        mUserDao = userDao;
    }
    @Override
    public Flowable<User> getUser() {
        return mUserDao.getUser();
    }
    @Override
    public void insertOrUpdateUser(User user) {
        mUserDao.insertUser(user);
    }
    @Override
    public void deleteAllUsers() {
        mUserDao.deleteAllUsers();
    }
}
```

## User


```java

package com.example.android.observability.persistence;
import androidx.room.ColumnInfo;
import androidx.room.Entity;
import androidx.room.Ignore;
import androidx.room.PrimaryKey;
import androidx.annotation.NonNull;
import java.util.UUID;
/**
 * Immutable model class for a User
 */
@Entity(tableName = "users")
public class User {
    @NonNull
    @PrimaryKey
    @ColumnInfo(name = "userid")
    private String mId;
    @ColumnInfo(name = "username")
    private String mUserName;
    @Ignore
    public User(String userName) {
        mId = UUID.randomUUID().toString();
        mUserName = userName;
    }
    public User(String id, String userName) {
        this.mId = id;
        this.mUserName = userName;
    }
    public String getId() {
        return mId;
    }
    public String getUserName() {
        return mUserName;
    }
}
```

## UserDao


```java

package com.example.android.observability.persistence;
import androidx.room.Dao;
import androidx.room.Insert;
import androidx.room.OnConflictStrategy;
import androidx.room.Query;
import io.reactivex.Flowable;
/**
 * Data Access Object for the users table.
 */
@Dao
public interface UserDao {
    /**
     * Get the user from the table. Since for simplicity we only have one user in the database,
     * this query gets all users from the table, but limits the result to just the 1st user.
     *
     * @return the user from the table
     */
    @Query("SELECT * FROM Users LIMIT 1")
    Flowable<User> getUser();
    /**
     * Insert a user in the database. If the user already exists, replace it.
     *
     * @param user the user to be inserted.
     */
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    void insertUser(User user);
    /**
     * Delete all users.
     */
    @Query("DELETE FROM Users")
    void deleteAllUsers();
}
```

## UsersDatabase


```java

package com.example.android.observability.persistence;
import androidx.room.Database;
import androidx.room.Room;
import androidx.room.RoomDatabase;
import android.content.Context;
/**
 * The Room database that contains the Users table
 */
@Database(entities = {User.class}, version = 1)
public abstract class UsersDatabase extends RoomDatabase {
    private static volatile UsersDatabase INSTANCE;
    public abstract UserDao userDao();
    public static UsersDatabase getInstance(Context context) {
        if (INSTANCE == null) {
            synchronized (UsersDatabase.class) {
                if (INSTANCE == null) {
                    INSTANCE = Room.databaseBuilder(context.getApplicationContext(),
                            UsersDatabase.class, "Sample.db")
                            .build();
                }
            }
        }
        return INSTANCE;
    }
}
```

## UserActivity


```java

package com.example.android.observability.ui;
import androidx.lifecycle.ViewModelProviders;
import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import android.util.Log;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import com.example.android.observability.Injection;
import com.example.android.persistence.R;
import io.reactivex.android.schedulers.AndroidSchedulers;
import io.reactivex.disposables.CompositeDisposable;
import io.reactivex.schedulers.Schedulers;
/**
 * Main screen of the app. Displays a user name and gives the option to update the user name.
 */
public class UserActivity extends AppCompatActivity {
    private static final String TAG = UserActivity.class.getSimpleName();
    private TextView mUserName;
    private EditText mUserNameInput;
    private Button mUpdateButton;
    private ViewModelFactory mViewModelFactory;
    private UserViewModel mViewModel;
    private final CompositeDisposable mDisposable = new CompositeDisposable();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_user);
        mUserName = findViewById(R.id.user_name);
        mUserNameInput = findViewById(R.id.user_name_input);
        mUpdateButton = findViewById(R.id.update_user);
        mViewModelFactory = Injection.provideViewModelFactory(this);
        mViewModel = ViewModelProviders.of(this, mViewModelFactory).get(UserViewModel.class);
        mUpdateButton.setOnClickListener(v -> updateUserName());
    }
    @Override
    protected void onStart() {
        super.onStart();
        // Subscribe to the emissions of the user name from the view model.
        // Update the user name text view, at every onNext emission.
        // In case of error, log the exception.
        mDisposable.add(mViewModel.getUserName()
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(userName -> mUserName.setText(userName),
                        throwable -> Log.e(TAG, "Unable to update username", throwable)));
    }
    @Override
    protected void onStop() {
        super.onStop();
        // clear all the subscriptions
        mDisposable.clear();
    }
    private void updateUserName() {
        String userName = mUserNameInput.getText().toString();
        // Disable the update button until the user name update has been done
        mUpdateButton.setEnabled(false);
        // Subscribe to updating the user name.
        // Re-enable the button once the user name has been updated
        mDisposable.add(mViewModel.updateUserName(userName)
                .subscribeOn(Schedulers.io())
                .observeOn(AndroidSchedulers.mainThread())
                .subscribe(() -> mUpdateButton.setEnabled(true),
                        throwable -> Log.e(TAG, "Unable to update username", throwable)));
    }
}
```

## UserViewModel


```java

package com.example.android.observability.ui;
import androidx.lifecycle.ViewModel;
import com.example.android.observability.UserDataSource;
import com.example.android.observability.persistence.User;
import io.reactivex.Completable;
import io.reactivex.Flowable;
/**
 * View Model for the {@link UserActivity}
 */
public class UserViewModel extends ViewModel {
    private final UserDataSource mDataSource;
    private User mUser;
    public UserViewModel(UserDataSource dataSource) {
        mDataSource = dataSource;
    }
    /**
     * Get the user name of the user.
     *
     * @return a {@link Flowable} that will emit every time the user name has been updated.
     */
    public Flowable<String> getUserName() {
        return mDataSource.getUser()
                // for every emission of the user, get the user name
                .map(user -> {
                    mUser = user;
                    return user.getUserName();
                });
    }
    /**
     * Update the user name.
     *
     * @param userName the new user name
     * @return a {@link Completable} that completes when the user name is updated
     */
    public Completable updateUserName(final String userName) {
        return Completable.fromAction(() -> {
            // if there's no use, create a new user.
            // if we already have a user, then, since the user object is immutable,
            // create a new user, with the id of the previous user and the updated user name.
            mUser = mUser == null
                    ? new User(userName)
                    : new User(mUser.getId(), userName);
            mDataSource.insertOrUpdateUser(mUser);
        });
    }
}
```

## ViewModelFactory


```java

package com.example.android.observability.ui;
import androidx.lifecycle.ViewModel;
import androidx.lifecycle.ViewModelProvider;
import com.example.android.observability.UserDataSource;
/**
 * Factory for ViewModels
 */
public class ViewModelFactory implements ViewModelProvider.Factory {
    private final UserDataSource mDataSource;
    public ViewModelFactory(UserDataSource dataSource) {
        mDataSource = dataSource;
    }
    @Override
    public <T extends ViewModel> T create(Class<T> modelClass) {
        if (modelClass.isAssignableFrom(UserViewModel.class)) {
            return (T) new UserViewModel(mDataSource);
        }
        throw new IllegalArgumentException("Unknown ViewModel class");
    }
}
```


