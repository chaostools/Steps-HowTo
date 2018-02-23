
# Android Localization Example

 February 21, 2018

 Since Android system supports multiple languages and allows user to choose language or

 locale, displaying your app content or resource based on the chosen locale allows users

 to view your app’s text content in user preferred language and to experience other

 content in a way that reects the culture of the selected locale.

 To support multiple languages or the localization in your android app, you need to create

 app resources for each locale that you want your app to support and keep them in the

 locale specic resource folders in your app. In this tutorial, you can learn how to set

 locale programmatically and how to implement localization in your application with an

 example.

## Android Localization

 To implement localization in your app, you need to create resources for each language in

 addition to default language and save them in language specic folders. For example,

<I> folder name of values folder for ja_JE locale is values-b+ja, you need to create locale </i>

 specic content for ja_JE locale and save them under values-b+ja folder. If locale is set to

 ja_JE, Android will pick resources from –b+ja folders. If there are no resources for current

 locale, then resources from default folder for example from values folder will be used.

 As shown below, to support, ja_JE and hi_In locales, string.xml les specic to those

 languages and drawables for those locales are created and store in locale specic folder.

## Device Locale

 User can set language on android device by going to settings > languages & input >

 languages and choosing a language and that will be the device locale. If resources for the

 user selected locale are not available in your app, then the default resources will be used.

## Setting Locale Programmatically

 You can override device locale by setting locale programmatically in your app. This will be

 useful if your app is used mainly in the target locale or if your app supports multiple

 locales and provides an option for user to set locale preference.

 You can set locale at activity level in activity or app level in Application object by creating

 Locale object, setting default locale by calling setDefault on Locale passing locale object

 and calling updateConguration on resources object passing conguration object as

 shown below.

```java
Locale jaLocale = new Locale("ja");
Locale.setDefault(locale);
```
```java
Configuration configuration = ctx.getResources().getConfiguration();
DisplayMetrics displayMetrics = ctx.getResources().getDisplayMetrics();
configuration.locale=locale;
```
```java
ctx.getResources().updateConfiguration(configuration, displayMetrics);
```
## Android Localization Example
<I>
 Below example displays registration screen in dierent languages based on the locale set programmatically. I created string resources in dierent languages using Google translate for this example. Below is the output  when locale is set to hi_IN  Below is the output when locale is set to ja_JE
</I>

## Activity

```java
import android.os.Bundle ;
import android.support.v7.app.AppCompatActivity ;
import android.view. View ;
import android.widget. Toast ;
```
```java
public class MainActivity extends AppCompatActivity implements View. OnClickListener {
@Override
protected void onCreate( Bundle savedInstanceState) {
AppUtils .setConfigChange( this );
super .onCreate(savedInstanceState);
```
```java
setContentView( R .layout.activity_main);
```
```java
findViewById( R .id.login_b).setOnClickListener( this );
}
@Override
public void onClick( View view) {
Toast .makeText( this ,
getResources().getString( R .string.login_success),
Toast. LENGTH_SHORT ).show();
}
}
```
## Layout

```xml
<? xml version="1.0" encoding="utf-8" ?>
<android.support.constraint.ConstraintLayout
xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:app="http://schemas.android.com/apk/res-auto"
xmlns:tools="http://schemas.android.com/tools"
android:layout_width="match_parent"
android:layout_height="match_parent"
tools:context="zoftino.com.fundamentals.MainActivity">
<TextView
android:id="@+id/login_tv"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:layout_marginLeft="8dp"
android:layout_marginRight="8dp"
android:layout_marginTop="16dp"
android:text="@string/login_head"
android:textAppearance="@style/TextAppearance.AppCompat.Large"
app:layout_constraintLeft_toLeftOf="parent"
app:layout_constraintRight_toRightOf="parent"
app:layout_constraintTop_toTopOf="parent" />
<android.support.design.widget.TextInputLayout
android:id="@+id/email_l"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:layout_marginLeft="8dp"
android:layout_marginRight="8dp"
app:layout_constraintLeft_toLeftOf="parent"
app:layout_constraintRight_toRightOf="parent"
app:layout_constraintTop_toBottomOf="@+id/login_tv">
<EditText
android:id="@+id/email"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:hint="@string/email_hint" />
</android.support.design.widget.TextInputLayout>
<android.support.design.widget.TextInputLayout
android:id="@+id/password_l"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:layout_marginLeft="8dp"
android:layout_marginRight="8dp"
app:layout_constraintLeft_toLeftOf="parent"
app:layout_constraintRight_toRightOf="parent"
app:layout_constraintTop_toBottomOf="@+id/email_l">
<EditText
android:id="@+id/password"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:hint="@string/password_hint"
android:inputType="textWebPassword" />
</android.support.design.widget.TextInputLayout>
<Button
android:id="@+id/login_b"
style="@style/Widget.AppCompat.Button.Colored"
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:layout_marginLeft="8dp"
android:layout_marginRight="8dp"
android:layout_marginTop="16dp"
android:text="@string/login_b"
app:layout_constraintLeft_toLeftOf="parent"
app:layout_constraintRight_toRightOf="parent"
app:layout_constraintTop_toBottomOf="@+id/password_l" />
<ImageView
android:id="@+id/img"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:layout_marginTop="24dp"
android:src= "@drawable/national_animal"
app:layout_constraintLeft_toLeftOf="parent"
app:layout_constraintRight_toRightOf="parent"
app:layout_constraintTop_toBottomOf="@+id/login_b"/>
</android.support.constraint.ConstraintLayout>
```
## Application

```java
import android.app. Application ;
import java.util. Locale ;
```
```java
public class LangApplication extends Application {
@Override
public void onCreate()
{
setLocale();
super .onCreate();
}
private void setLocale(){
Locale jaLocale = new Locale ("ja");
AppUtils .setLocale(jaLocale);
AppUtils .setConfigChange( this );
}
}
```
## Locale Utility

```
import android.content.Context;
import android.content.res.Configuration;
import android.util.DisplayMetrics;
```
```
import java.util.Locale;
```
```
public class AppUtils {
private static Locale locale;
```
```
public static void setLocale (Locale localeIn) {
locale = localeIn;
if (locale != null ) {
Locale.setDefault(locale);
}
}
public static void setConfigChange (Context ctx){
if (locale != null ){
Locale.setDefault(locale);
```
```
Configuration configuration = ctx.getResources().getConfiguration();
DisplayMetrics displayMetrics = ctx.getResources().getDisplayMetrics();
configuration.locale=locale;
```
```
ctx.getResources().updateConfiguration(configuration, displayMetrics);
}
}
}
```
## AndroidManifest

```xml
<? xml version="1.0" encoding="utf-8" ?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
package="zoftino.com.fundamentals">
<application
android:name=".LangApplication"
```
