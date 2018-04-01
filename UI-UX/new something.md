# UI/UX

## 1 Collapsing Action Bar

- Prerequisite:
	- set your activity's layout to ` "AppTheme.NoActionBar" `.
	```XML

    <style name="AppTheme.NoActionBar" parent="AppTheme">
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
        <item name="windowActionModeOverlay">true</item>
    </style>

    ```

	- open your XMLs' design tab & add actionbar with required add-ons.
  	  ** DO NOT add image background **, it will just cause more complications.
  	  You can add image later by yourself.
	  This will add the following code:
	  <I only wanted a collapsable toolbar>
	  <I dropped $$$i$$$ in places where i want to explain,DONOT DIRECTLY COPY>
		
	```XML

	<?xml version="1.0" encoding="utf-8"?>
	<android.support.design.widget.CoordinatorLayout 
		xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent">
	    <android.support.design.widget.AppBarLayout
	        android:id="@+id/appbar"											
	        android:layout_height="192dp"																	$$$1$$$
	        android:layout_width="match_parent"
	        																								$$$2$$$
            >	        																							
	        <android.support.design.widget.CollapsingToolbarLayout
	            android:layout_width="match_parent"
	            android:layout_height="match_parent"
	            app:toolbarId="@+id/toolbar"
	            app:layout_scrollFlags="scroll|enterAlways|enterAlwaysCollapsed"							$$$3$$$
	            app:contentScrim="?attr/colorPrimary">														$$$4$$$
	            <android.support.v7.widget.Toolbar
	                android:id="@+id/toolbar"
	                android:layout_height="?attr/actionBarSize"												$$$5$$$
	                android:layout_width="match_parent"
	                																						$$$6$$$
	                																						$$$7$$$	 $$$8$$$

	                />
	        </android.support.design.widget.CollapsingToolbarLayout>
	    </android.support.design.widget.AppBarLayout>
	    <android.support.v4.widget.NestedScrollView
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        app:layout_behavior="android.support.design.widget.AppBarLayout$ScrollingViewBehavior"			$$$9$$$
	                																						$$$10$$$

	        >
	        <android.support.constraint.ConstraintLayout
	            android:layout_width="match_parent"
	            android:layout_height="match_parent"
	            tools:context="com.work.of.chaostools.attempt1.Main2Activity">
	            <!-- Your Work ZONE -->
	        </android.support.constraint.ConstraintLayout>
	    </android.support.v4.widget.NestedScrollView>
	</android.support.design.widget.CoordinatorLayout>



	```

#### Explainations:
So basically the layout for a collapsable action bar would be:


```
-----------------------------------------------
|       ----------------	
|		|		|-----------------
|		|		|	
|		|		|		|------------------|
|		[ABL]	[CtL]	|[Your toolbar]	   |
|		|		|		|------------------|
|		|		|
[CL]	|		|-----------------
|		-----------------
|		-------------------------
|		|
|		|				|-------------------|
|		[NestSV]		|[Your Root Layout] |
|		|				|-------------------|
|		|			
|		-------------------------
------------------------------------------------
(a 4-level deep toolbar, and a 3 level deep root view , just for some cool UI,wth?! )

```
  - 1 AppBar Layout's height: will control your layout's height when in expanded mode.

  - 2 [empty]:here you should add another attribute `android:theme="@style/AppTheme.ActionBarStyle"`
      it will help you control the toolbar's text and icon colors when in expanded mode.In themes, I
      Designed the following 2 themes which are NOT extending AppTheme. They should extend a theme
      from `ThemeOverlay.something.something` themeclass because our app theme is currently no 
      actionbar.Moreover The other themes like `Theme.appcompat..` does not work.
      ```xml
	    <style name="AppTheme.ActionBarLight" parent="ThemeOverlay.AppCompat.Light"/>
	    <style name="AppTheme.ActionBarStyle" parent="ThemeOverlay.AppCompat.Dark.ActionBar" />
      ```
  
  - 3 Scrollbars flag: they are for controlling collaping action bars's scrolling's behavior.[need 
      to read more about that]
  
  - 4 `app:contentScrim="?attr/colorPrimary"` this is a nice attribute. it controls the color that 
  	   will be temporarily shown when user transitions from closed appbar to expanded appbar it will
  	   be also shown when user is scrolling .this colr will also be the one which will be shown as 
  	   toolbar color whenuser is scrolling downa nd downa nd suddenly it swipes up and toolbar 
  	   appears.it only takes ?attr/... (i guess)

  - 5  Height of toolbar in normal state. yu should provvide `?attr/...` because its a mobile/screen 
       specific value (i guess)

  - 6&7 [empty] `app:title="@string/app_name"` & `app:popupTheme="@style/AppTheme.ActionBarStyle" `:
        These are the 2 lines that you might wanna add in your toolbar to show title and set theme  
        for toolbar in closed mode for the reasons same as in point 2.

  - 8  [empty] `app:layout_collapseMode="pin"` in toolbar to 'pin'the normal mode-toolbar when doing
  		point 4 and not just 'toolbar coming and hiding away instantly'(i guess)


  - 9  `app:layout_behavior="android.support...." ` in nested scroll view to provide the animation
  	   of toolbar hiding/coming back automatically. this could be set in a recycler view too.

  - 10 [empty]fill viewport in recycler view to fill viewport 

                


