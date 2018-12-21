# Head First Android Development

- [Chapter 1. Getting Started: Diving In](#chapter-1-getting-started-diving-in)
- [Chapter 2. Building Interactive Apps: Apps That Do Something](#chapter-2-building-interactive-apps-apps-that-do-something)
- [Chapter 3. Multiple Activities and Intents: State Your Intent](#chapter-3-multiple-activities-and-intents-state-your-intent)
- [Chapter 4. The Activity Lifecycle: Being an Activity](#chapter-4-the-activity-lifecycle-being-an-activity)
- [Chapter 5. Views and View Groups: Enjoy the View](#chapter-5-views-and-view-groups-enjoy-the-view)
- [Chapter 6. Constraint Layouts: Put Things in Their Place](#chapter-6-constraint-layouts-put-things-in-their-place)
- [Chapter 7. List Views and Adapters: Getting Organized](#chapter-7-list-views-and-adapters-getting-organized)
- [Chapter 8. Support Libraries and App Bars: Taking Shortcuts](#chapter-8-support-libraries-and-app-bars-taking-shortcuts)
- [Chapter 9. Fragments: Make It Modular](#chapter-9-fragments-make-it-modular)
- [Chapter 10. Fragments for Larger Interfaces: Different Size, Different Interface](#chapter-10-fragments-for-larger-interfaces-different-size-different-interface)
- [Chapter 11. Dynamic Fragments: Nesting Fragments](#chapter-11-dynamic-fragments-nesting-fragments)
- [Chapter 12. Design Support Library: Swipe Right](#chapter-12-design-support-library-swipe-right)
- [Chapter 13. Recycler Views and Card Views: Get Recycling](#chapter-13-recycler-views-and-card-views-get-recycling)
- [Chapter 14. Navigation Drawers: Going Places](#chapter-14-navigation-drawers-going-places)
- [Chapter 15. SQLite Databases: Fire Up the Database](#chapter-15-sqlite-databases-fire-up-the-database)
- [Chapter 16. Basic Cursors: Getting Data Out](#chapter-16-basic-cursors-getting-data-out)
- [Chapter 17. Cursors and Asynctasks: Staying in the Background](#chapter-17-cursors-and-asynctasks-staying-in-the-background)
- [Chapter 18. Started Services: At Your Service](#chapter-18-started-services-at-your-service)
- [Chapter 19. Bound Services and Permissions: Bound Together](#chapter-19-bound-services-and-permissions-bound-together)

## Chapter 1. Getting Started: Diving In

- Versions of Android have a version number, API level, and code name.
- Android Studio is a special version of IntelliJ IDEA that interfaces with the Android Software Development Kit (SDK) and the Gradle build system.
- A typical Android app is composed of activities, layouts, and resource files.
- Layouts describe what your app looks like. They’re held in the  `app/src/main/res/layout`  folder.
- Activities describe what your app does, and how it interacts with the user. The activities you write are held in the  `app/src/main/java`  folder.
- AndroidManifest.xml contains information about the app itself. It lives in the  `app/src/main`  folder.
- An AVD is an Android Virtual Device. It runs in the Android emulator and mimics a physical Android device.
- An APK is an Android application package. It’s like a JAR file for Android apps, and contains your app’s bytecode, libraries, and resources. You install an app on a device by installing the APK.
- Android apps run in separate processes using the Android runtime (ART).
- The  `<TextView>`  element is used for displaying text.

## Chapter 2. Building Interactive Apps: Apps That Do Something

- The  `<Button>`  element is used to add a button.
- The  `<Spinner>`  element is used to add a spinner, which is a drop-down list of values.
- All GUI components are types of view. They inherit from the Android View class.
-  `strings.xml`  is a String resource file. It’s used to separate out text values from the layouts and activities, and supports localization.
- Add a String to  `strings.xml`  using:

		<string name="name">Value</string>
- Reference a String in the layout using:

		"@string/name"
- Add an array of String values to strings.xml using:

		<string-array name="array">
		    <item>string1</item>
		    ...
		</string-array>	
- Reference a string-array in the layout using:

		"@array/array_name"

- Make a button call a method when clicked by adding the following to the layout:

		android:onClick="clickMethod"
- There needs to be a corresponding method in the activity:

		public void clickMethod(View view){
		}	
-  `R.java`  is generated for you. It enables you to get references for layouts, GUI components, Strings, and other resources in your Java code.
- Use  `findViewById()`  to get a reference to a view.
- Use `setText()`  to set the text in a view.
- Use  `getSelectedItem()`  to get the selected item in a spinner.
- Add a custom class to an Android project by going to File menu→New...→Java Class.

## Chapter 3. Multiple Activities and Intents: State Your Intent

- A task is two or more activities chained together.
- The  `<EditText>` element defines an editable text field for entering text. It inherits from the Android View class.
- You can add a new activity in Android Studio by choosing File → New... → Activity.
- Each activity you create must have an entry in  `AndroidManifest.xml` .
- An intent is a type of message that Android components use to communicate with one another.
- An explicit intent specifies the component the intent is targeted at. You create an explicit intent using Intent intent = new Intent(this, Target.class);.
- To start an activity, call  `startActivity(intent)` . If no activities are found, it throws an ActivityNotFoundException.
- Use the  `putExtra()`  method to add extra information to an intent.
- Use the  `getIntent()`  method to retrieve the intent that started the activity.
- Use the  `get*Extra()`  methods to retrieve extra information associated with the intent.  `getStringExtra()`  retrieves a String,  `getIntExtra()`  retrieves an int, and so on.
- An activity action describes a standard operational action an activity can perform. For example, to send a message, use  `Intent.ACTION_SEND` .
- To create an implicit intent that specifies an action, use  `Intent intent = new Intent(action);` .
- To describe the type of data in an intent, use the  `setType()`  method.
- Android resolves intents based on the named component, action, type of data, and categories specified in the intent. It compares the contents of the intent with the intent filters in each app’s AndroidManifest.xml. An activity must have a category of  `DEFAULT`  if it is to receive an implicit intent.
- The  `createChooser()`  method allows you to override the default Android activity chooser dialog. It lets you specify a title for the dialog, and doesn’t give the user the option of setting a default activity. If no activities can receive the intent it is passed, it displays a message. The  `createChooser()` method returns an Intent.
- You retrieve the value of a  `String`  resource using  `getString(R.string.stringname);`.

## Chapter 4. The Activity Lifecycle: Being an Activity

- Each app runs in its own process by default.
- Only the main thread can update the user interface.
- Use a Handler to schedule code or post code to a different thread.
- A device configuration change results in the activity being destroyed and recreated.
- Your activity inherits the lifecycle methods from the android.app.Activity class. If you override any of these methods, you need to call up to the method in the superclass.
-  `onSaveInstanceState(Bundle)`  enables your activity to save its state before the activity gets destroyed. You can use the Bundle to restore state in  `onCreate()` .
- You add values to a Bundle using  `bundle.put*("name", value)` . You retrieve values from the bundle using  `bundle.get*("name")` .
-  `onCreate()`  and  `onDestroy()`  deal with the birth and death of the activity.
-  `onRestart()` ,  `onStart()` , and  `onStop()`  deal with the visibility of the activity.
-  `onResume()`  and  `onPause()`  handle when the activity gains and loses the focus.

## Chapter 5. Views and View Groups: Enjoy the View

- GUI components are all types of view. They are all subclasses of the  `android.view.View`  class.
- All layouts are subclasses of the  `android.view.ViewGroup`  class. A view group is a type of view that can contain multiple views.
- The layout XML file gets converted to a ViewGroup containing a hierarchical tree of views.
- A linear layout lists views either horizontally or vertically. You specify the direction using the android:orientation attribute.
- A frame layout stacks views.
- Use  `android:padding*`  attributes to specify how much padding you want around a view.
- In a linear layout, use android:layout_weight if you want a view to use up extra space in the layout.
-  `android:layout_gravity`  lets you say where you want views to appear in their available space.
-  `android:gravity`  lets you say where you want the contents to appear inside a view.
-  `<ToggleButton>` defines a toggle button that allows you to choose between two states by clicking a button.
-  `<Switch>` defines a switch control that behaves in the same way as a toggle button. It requires API level 14 or above.
-  `<CheckBox>` defines a checkbox.
- To define a group of radio buttons, first use <RadioGroup> to define the radio group. Then put individual radio buttons in the radio group using <RadioButton>.
- Use  `<ImageView>` to display an image.
-  `<ImageButton>` defines a button with no text, just an image.
- Add scrollbars using `<ScrollView>` or  `<HorizontalScrollView>` .
- A Toast is a pop-up message.

## Chapter 6. Constraint Layouts: Put Things in Their Place

- Constraint layouts are designed to work with Android Studio’s design editor. They have their own library and can be used in apps where the minimum SDK is API level 9 or above.
- Position views by adding constraints. Each view needs at least one horizontal and one vertical constraint.
- Center views by adding constraints to opposite sides of the view. Change the view’s bias to update its position between the constraints.
- You can change a view’s size to match its constraints if the view has constraints on opposing sides.
- You can specify a  `width:height`  aspect ratio for the view’s size.
- Clicking on the Infer Constraints button adds constraints to views based on their position in the blueprint.

## Chapter 7. List Views and Adapters: Getting Organized

- Sort your ideas for activities into top-level activities, category activities, and detail/edit activities. Use the category activities to navigate from the top-level activities to the detail/edit activities.
- A list view displays items in a list. Add it to your layout using the  `<ListView>`  element.
- Use  `android:entries`  in your layout to populate the items in your list views from an array defined in  `strings.xml` .
- An adapter acts as a bridge between an  `AdapterView`  and a data source. ListViews and Spinners are both types of  `AdapterView` .
- An  `ArrayAdapter`  is an adapter that works with arrays.
- Handle click events on Buttons using  `android:onClick`  in the layout code. Handle click events elsewhere by creating a listener and implementing its click event.

## Chapter 8. Support Libraries and App Bars: Taking Shortcuts

- You add a basic app bar by applying a theme that contains one.
- The Android Support Libraries provide backward compatibility with older versions of Android.
- The AppCompatActivity class is a type of activity that resides in the v7 AppCompat Support Library. In general, your activity needs to extend the AppCompatActivity class whenever you want an app bar that provides backward compatibility with older versions of Android.
- The  `android:theme`  attribute in  `AndroidManifest.xml`  specifies which theme to apply.
- You define styles in a style resource file using the  `<style>`  element. The name attribute gives the style a name. The parent attribute specifies where the style should inherit its properties from.
- The latest app bar features are in the Toolbar class in the v7 AppCompat Support Library. You can use a toolbar as your app bar.
- Add actions to your app bar by adding them to a menu resource file.
- Add the items in the menu resource file to the app bar by implementing the activity’s `onCreateOptionsMenu()`  method.
- You determine what items should do when clicked by implementing the activity’s  `onOptionsItemSelected()`  method.
- Add an Up button to your app bar to navigate up the app’s hierarchy. Specify the hierarchy in AndroidManifest.xml. Use the ActionBar  `setDisplayHomeAsUpEnabled()`  method to enable the Up button.
- You can share content by adding the share action provider to your app bar. Add it by including it in your menu resource file. Call its  `setShareIntent()`  method to pass it an intent describing the content you wish to share.

## Chapter 9. Fragments: Make It Modular

- A fragment is used to control part of a screen. It can be reused across multiple activities.
- A fragment has an associated layout.
- The  `onCreateView()`  method gets called each time Android needs the fragment’s layout.
- Add a fragment to an activity’s layout using the  `<fragment>` element and adding a  `name`  attribute.
- The fragment lifecycle methods tie in with the states of the activity that contains the fragment.
- The Fragment class doesn’t extend the Activity class or implement the Context class.
- Fragments don’t have a  `findViewById()`  method. Instead, use the  `getView()`  method to get a reference to the root view, then call the view’s  `findViewById()`  method.
- A list fragment is a fragment that comes complete with a  `ListView` . You create one by subclassing  `ListFragment` .

## Chapter 10. Fragments for Larger Interfaces: Different Size, Different Interface

- Make apps look different on different devices by putting separate layouts in device-appropriate folders.
- Android keeps track of places you’ve visited within an app by adding them to the back stack as separate transactions. Pressing the Back button pops the last transaction off the back stack.
- Use a frame layout to add, replace, or remove fragments programmatically using fragment transactions.
- Begin the transaction by calling the FragmentManager `beginTransaction()`  method. This creates a  `FragmentTransaction` object.
- Add, replace, and delete fragments using the FragmentTransaction  `add()` ,  `replace()` , and  `remove()`  methods.
- Add a transaction to the back stack using the FragmentTransaction  `addToBackStack()`  method.
- Commit a transaction using the FragmentTransaction  `commit()` method. This applies all the updates in the transaction.
- Save the state of a fragment’s variables in the Fragment  `onSaveInstanceState()`  method.
- Restore the state of a fragment’s variables in the Fragment  `onCreate()`  method.

## Chapter 11. Dynamic Fragments: Nesting Fragments

- Fragments can contain other fragments.
- If you use the  `android:onClick`  attribute in a fragment, Android will look for a method of that name in the fragment’s parent activity.
- Instead of using the  `android:onClick`  attribute in a fragment, make the fragment implement the  `View.OnClickListener`  interface and implement its  `onClick()`  method.
- If you use the  `<fragment>`  element in your layout, the fragment gets recreated when you rotate the device. If your fragment is dynamic, use a fragment transaction instead.
- Fragments contain two methods for getting a fragment manager,  `getFragmentManager()`  and  `getChildFragmentManager()` .
-  `getFragmentManager()`  gets a reference to the fragment manager associated with the fragment’s parent activity. Any fragment transactions you create using this fragment manager are added to the back stack as extra transactions.
-  `getChildFragmentManager()`  gets a reference to the fragment manager associated with the fragment’s parent fragment. Any fragment transactions you create using this fragment manager are nested inside the parent fragment transaction.

## Chapter 12. Design Support Library: Swipe Right

- Enable swipe navigation using a view pager.
- You tell a view pager about its pages by implementing a fragment pager adapter.
- Use the fragment pager adapter’s  `getCount()`  method to tell the view pager how many pages it should have. Use its  `getItem()`  method to tell it which fragment should appear on each page.
- Add tab navigation by implementing a tab layout. Put the toolbar and tab layout inside an app bar layout in your layout code, then attach the tab layout to the view pager in your activity code.
- The tab layout comes from the Android Design Support Library. This library helps you implement the material design guidelines in your app.
- Use a coordinator layout to coordinate animations between views.
- Add scrollable content the coordinator layout can coordinate using a nested scroll view.
- Use a collapsing toolbar layout to add a toolbar that collapses and grows in response to user scroll actions.
- Use a FAB (floating action button) to promote common or important user actions.
- A snackbar lets you display short messages that the user can interact with.

## Chapter 13. Recycler Views and Card Views: Get Recycling

- Card views and recycler views have their own Support Libraries.
- Add a card view to a layout using the  `<android.support.v7.widget.CardView>`  element.
- Give the card view rounded corners using the  `cardCornerRadius`  attribute. This requires a namespace of " `http://schemas.android.com/apk/res-auto` ".
- Give the card view a drop shadow using the  `cardElevation`  attribute. This requires a namespace of " `http://schemas.android.com/apk/res-auto` ".
- Recycler views work with adapters that are subclasses of  `RecyclerView.Adapter` .
- When you create your own  `RecyclerView.Adapter` , you must define the view holder and implement the  `onCreateViewHolder()` ,  `onBindViewHolder()` , and  `getItemCount()`  methods.
- You add a recycler view to a layout using the  `<android.support.v7.widget.RecyclerView>`  element. You give it a scrollbar using the  `android:scrollbars`  attribute.
- Use a layout manager to specify how items in a recycler view should be arranged. A  `LinearLayoutManager` arranges items in a linear list, a  `GridLayoutManager` arranges items in a grid, and a  `StaggeredGridLayoutManager` arranges items in a staggered grid.

## Chapter 14. Navigation Drawers: Going Places

- Use a navigation drawer if you want to provide the user with a large number of shortcuts, or group them into sections.
- Create a navigation drawer by adding a drawer layout to your activity’s layout. The drawer layout’s first element needs to be a view that defines the activity’s main content, usually a layout containing a  `Toolbar`  and  `FrameLayout` . Its second element defines the contents of the drawer, usually a  `NavigationView` .
- The  `NavigationView`  comes from the Design Support Library. It controls most of the drawer’s behavior.
- You add a header to your drawer by creating a layout for it, and adding the header’s resource ID to the navigation view’s  `headerLayout`  attribute.
- You add items to the drawer by creating a menu resource, and adding the menu’s resource ID to the navigation view’s  `menu` attribute.
- Add items to the menu resource in the order in which you want them to appear in the drawer.
- If you want to highlight which item in the drawer the user selects, add the menu items to a group and set the group’s  `checkableBehavior`  attribute to " `single` ".
- Use an  `ActionBarDrawerToggle`  to display a “burger” icon in the activity’s toolbar. This provides a visual sign that the activity has a navigation drawer. Clicking on it opens the drawer.
- Respond to the user clicking on items in the drawer by making your activity implement the  `NavigationView.OnNavigationItemSelectedListener`  interface. Register the activity with the navigation view as a listener, then implement the  `onNavigationItemSelected()`  method.
- Close the navigation drawer using the DrawerLayout  `closeDrawer()` method.

## Chapter 15. SQLite Databases: Fire Up the Database

- Android uses SQLite as its backend database to persist data.
- The  `SQLiteDatabase`  class gives you access to the SQLite database.
- A SQLite helper lets you create and manage SQLite databases. You create a SQLite helper by extending the  `SQLiteOpenHelper`  class.
- You must implement the SQLiteOpenHelper  `onCreate()`  and  `onUpgrade()`  methods.
- The database gets created the first time it needs to be accessed. You need to give the database a name and version number, starting at 1. If you don’t give the database a name, it will just get created in memory.
- The  `onCreate()`  method gets called when the database first gets created.
- The  `onUpgrade()`  method gets called when the database needs to be upgraded.
- Execute SQL using the SQLiteDatabase  `execSQL(String)`  method.
- Use the SQL  `ALTER TABLE`  command to change an existing table. Use  `RENAMETO`  to rename the table, and  `ADDCOLUMN`  to add a column.
- Use the SQL  `DROP TABLE`  command to delete a table.
- Add records to tables using the  `insert()`  method.
- Update records using the  `update()`  method.
- Remove records from tables using the  `delete()`  method.

## Chapter 16. Basic Cursors: Getting Data Out

- A  `cursor`  lets you read from and write to the database.
- You create a cursor by calling the SQLiteDatabase  `query()` method. Behind the scenes, this builds a  `SQL SELECT`  statement.
- The  `getWritableDatabase()`  method returns a  `SQLiteDatabase` object that allows you to read from and write to the database.
- The `getReadableDatabase()`  returns a  `SQLiteDatabase` object. This gives you read-only access to the database. It may also allow you to write to the database, but this isn’t guaranteed.
- Navigate through a cursor using the  `moveTo*()`  methods.
- Get values from a cursor using the  `get*()`  methods. Close cursors and database connections after you’ve finished with them.
- A cursor adapter is an adapter that works with cursors. Use  `SimpleCursorAdapter`  to populate a list view with the values returned by a cursor.

## Chapter 17. Cursors and Asynctasks: Staying in the Background

## Chapter 18. Started Services: At Your Service

- A service is an application component that can perform tasks in the background. It doesn’t have a user interface.
- A started service can run in the background indefinitely, even when the activity that started it is destroyed. Once the operation is done, it stops itself.
- A bound service is bound to another component such as an activity. The activity can interact with it and get results.
- A scheduled service is one that’s scheduled to run at a particular time.
- You can create a simple started service by extending the  `IntentService`  class, overriding its  `onHandleIntent()` method and adding a public constructor.
- You declare services in  `AndroidManifest.xml` using the  `<service>`  element.
- You start a started service using the  `startService()`  method.
- When a started service is created, its  `onCreate()`  method gets called, followed by  `onStartCommand()` . If the service is an IntentService,  `onHandleIntent()`  is then called in a separate thread. When the service has finished running,  `onDestroy()` gets called before the service is destroyed.
- The  `IntentService` class inherits lifecycle methods from the  `Service`  class.
- You log messages using the  `Android.util.Log`  class. You can view these messages in the logcat in Android Studio.
- You create a notification using a notification builder. Each notification must include a small icon, a title, and some text as a bare minimum.
- A heads-up notification has its priority set to high, and vibrates the device or plays a sound when it’s issued.
- You tell the notification which activity to start when it’s clicked by creating a pending intent and adding it to the notification as an action.
- You issue the notification using a notification manager. You create a notification manager using Android’s notification service.

## Chapter 19. Bound Services and Permissions: Bound Together

- You create a bound service by extending the  `Service`  class. You define your own  `Binder`  object, and override the  `onBind()` method.
- Bind a component to a service using the  `bindService()`  method.
- Use a  `ServiceConnection`  so that your activity can get a reference to the service when it’s bound.
- Unbind a component from a service using the  `unbindService()`  method.
- When a bound service is created, its  `onCreate()`  method is called.  `onBind()`  gets called when a component binds to the service.
- When all components have unbound from the service, its  `onUnbind()`  method is called.
- A bound service is destroyed when no components are bound to it. Its  `onDestroy()`  method is called just before the service is destroyed.
- Use the Android Location Services to determine the current location of the device.
- To get the current location of the device, you need to declare that the app requires  `ACCESS_FINE_LOCATION`  permission in  `AndroidManifest.xml` .
- Get location updates using a LocationListener.
- A LocationManager gives you access to Android’s Location Services. Get the best location provider available on the device using its  `getBestProvider()`  method. Request location updates from the provider using  `requestLocationUpdates()` .
- Use  `removeUpdates()`  to stop getting location updates.
- If your target SDK is API level 23 or above, check at runtime whether your app has been granted a permission using the  `ContextCompat.checkSelfPermission()`  method.
- Request permissions at runtime using  `ActivityCompat.requestPermissions()` .
- Check the user’s response to a permission request by implementing the activity’s  `onRequestPermissionsResult()`  method.


