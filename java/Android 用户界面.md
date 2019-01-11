# Java: Android 用户界面

## Activity

-  `onPause()`

	Called when the system is about to start resuming a previous activity. This is typically used to commit unsaved changes to persistent data, stop animations and other things that may be consuming CPU, etc. Implementations of this method must be very quick because the next activity will not be resumed until this method returns.

	Followed by either  `onResume()`  if the activity returns back to the front, or  `onStop()`  if it becomes invisible to the user.

	Pre-Build.VERSION_CODES.HONEYCOMB

	 `onResume()`  or  `onStop()`

-  `onStop()`

	Called when the activity is no longer visible to the user, because another activity has been resumed and is covering this one. This may happen either because a new activity is being started, an existing one is being brought in front of this one, or this one is being destroyed.

	Followed by either  `onRestart()`  if this activity is coming back to interact with the user, or  `onDestroy()`  if this activity is going away.

	Yes

	 `onRestart()`  or  `onDestroy()`

- https://developer.android.com/reference/android/app/Activity
