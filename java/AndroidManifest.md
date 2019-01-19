# Java: AndroidManifest

##  [`<application>`](https://developer.android.com/guide/topics/manifest/application-element.html?hl=zh-cn)


```java
<application android:allowTaskReparenting=["true" | "false"]
             android:allowBackup=["true" | "false"]
             android:allowClearUserData=["true" | "false"]
             android:backupAgent="string"
             android:backupInForeground=["true" | "false"]
             android:banner="drawable resource"
             android:debuggable=["true" | "false"]
             android:description="string resource"
             android:directBootAware=["true" | "false"]
             android:enabled=["true" | "false"]
             android:extractNativeLibs=["true" | "false"]
             android:fullBackupContent="string"
             android:fullBackupOnly=["true" | "false"]
             android:hasCode=["true" | "false"]
             android:hardwareAccelerated=["true" | "false"]
             android:icon="drawable resource"
             android:isGame=["true" | "false"]
             android:killAfterRestore=["true" | "false"]
             android:largeHeap=["true" | "false"]
             android:label="string resource"
             android:logo="drawable resource"
             android:manageSpaceActivity="string"
             android:name="string"
             android:networkSecurityConfig="xml resource"
             android:permission="string"
             android:persistent=["true" | "false"]
             android:process="string"
             android:restoreAnyVersion=["true" | "false"]
             android:requiredAccountType="string"
             android:resizeableActivity=["true" | "false"]
             android:restrictedAccountType="string"
             android:supportsRtl=["true" | "false"]
             android:taskAffinity="string"
             android:testOnly=["true" | "false"]
             android:theme="resource or theme"
             android:uiOptions=["none" | "splitActionBarWhenNarrow"]
             android:usesCleartextTraffic=["true" | "false"]
             android:vmSafeMode=["true" | "false"] >
    . . .
</application>
```

##  [`<activity>`](https://developer.android.com/guide/topics/manifest/activity-element?hl=zh-cn)

```java
<activity android:allowEmbedded=["true" | "false"]
          android:allowTaskReparenting=["true" | "false"]
          android:alwaysRetainTaskState=["true" | "false"]
          android:autoRemoveFromRecents=["true" | "false"]
          android:banner="drawable resource"
          android:clearTaskOnLaunch=["true" | "false"]
          android:configChanges=["mcc", "mnc", "locale",
                                 "touchscreen", "keyboard", "keyboardHidden",
                                 "navigation", "screenLayout", "fontScale",
                                 "uiMode", "orientation", "screenSize",
                                 "smallestScreenSize"]
          android:documentLaunchMode=["intoExisting" | "always" |
                                  "none" | "never"]
          android:enabled=["true" | "false"]
          android:excludeFromRecents=["true" | "false"]
          android:exported=["true" | "false"]
          android:finishOnTaskLaunch=["true" | "false"]
          android:hardwareAccelerated=["true" | "false"]
          android:icon="drawable resource"
          android:label="string resource"
          android:launchMode=["standard" | "singleTop" |
                              "singleTask" | "singleInstance"]
          android:maxRecents="integer"
          android:multiprocess=["true" | "false"]
          android:name="string"
          android:noHistory=["true" | "false"]
          android:parentActivityName="string"
          android:permission="string"
          android:process="string"
          android:relinquishTaskIdentity=["true" | "false"]
          android:resizeableActivity=["true" | "false"]
          android:screenOrientation=["unspecified" | "behind" |
                                     "landscape" | "portrait" |
                                     "reverseLandscape" | "reversePortrait" |
                                     "sensorLandscape" | "sensorPortrait" |
                                     "userLandscape" | "userPortrait" |
                                     "sensor" | "fullSensor" | "nosensor" |
                                     "user" | "fullUser" | "locked"]
          android:stateNotNeeded=["true" | "false"]
          android:supportsPictureInPicture=["true" | "false"]
          android:taskAffinity="string"
          android:theme="resource or theme"
          android:uiOptions=["none" | "splitActionBarWhenNarrow"]
          android:windowSoftInputMode=["stateUnspecified",
                                       "stateUnchanged", "stateHidden",
                                       "stateAlwaysHidden", "stateVisible",
                                       "stateAlwaysVisible", "adjustUnspecified",
                                       "adjustResize", "adjustPan"] >
    . . .
</activity>
```

###  `android:allowBackup`

Whether to allow the application to participate in the backup and restore infrastructure. If this attribute is set to false, no backup or restore of the application will ever be performed, even by a full-system backup that would otherwise cause all application data to be saved via adb. The default value of this attribute is true.

###  `android:backupAgent`

The name of the class that implements the application's backup agent, a subclass of [BackupAgent](https://developer.android.com/reference/android/app/backup/BackupAgent.html). The attribute value should be a fully qualified class name (such as, "com.example.project.MyBackupAgent"). However, as a shorthand, if the first character of the name is a period (for example, ".MyBackupAgent"), it is appended to the package name specified in the  [`<manifest>`](https://developer.android.com/guide/topics/manifest/manifest-element.html)  element.

There is no default. The name must be specified.

###  `android:fullBackupContent`

This attribute points to an XML file that contains full backup rules for [Auto Backup](https://developer.android.com/guide/topics/data/autobackup.html). These rules determine what files get backed up. For more information, see [XML Config Syntax](https://developer.android.com/guide/topics/data/autobackup.html#XMLSyntax) for Auto Backup.

This attribute is optional. If it is not specified, by default, Auto Backup includes most of your app's files. For more information, see [Files that are backed up](https://developer.android.com/guide/topics/data/autobackup.html#Files).



