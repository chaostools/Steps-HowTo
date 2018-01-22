# Add up navigation
Each screen in your app that is not the main entry point (all screens that are not the "home" screen) should provide navigation so the user can return to the logical parent screen in the app hierarchy by tapping the Up button in the app bar.

All you need to do is declare which activity is the logical parent in the AndroidManifest.xml file. So open the file at app > manifests > AndroidManifest.xml, locate the <activity> tag for DisplayMessageActivity and replace it with the following:

```
<activity android:name=".DisplayMessageActivity"
          android:parentActivityName=".MainActivity">
    <!-- The meta-data tag is required if you support API level 15 and lower -->
    <meta-data
        android:name="android.support.PARENT_ACTIVITY"
        android:value=".MainActivity" />
</activity>
```

---
