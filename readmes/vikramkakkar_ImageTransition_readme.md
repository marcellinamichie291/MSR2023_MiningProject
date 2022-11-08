# ImageTransition

... is a small android library to transition between a circular ImageView from one Activity to a rectangular ImageView in the launched Activity. 

Gradle dependency
-----------------

```    
compile 'com.appeaser.imagetransitionlibrary:imagetransitionlibrary:0.0.1'
```

Walkthrough
-----------
Following gif has been taken from the sample application available here: [<img alt="Get it on Google Play" height="45px" src="https://play.google.com/intl/en_us/badges/images/apps/en-play-badge-border.png" />][1]

<p align="center">
    <img src="https://github.com/vikramkakkar/ImageTransition/blob/master/img/image_transition.gif?raw=true" width="300" height="533" />
</p>

The transition has been slowed down for demo purposes.

Components
----------

**ImageTransition:** [Transition](https://developer.android.com/reference/android/transition/Transition.html) based on [ChangeBounds](https://developer.android.com/reference/android/transition/ChangeBounds.html) that provides animation support between a circular and rectangular ImageView (implemented as TransitionImageView) residing in two different activities. Works on API version >= 21.

**TransitionImageView:** A modified version of Henning Dodenhof's [CircleImageView](https://github.com/hdodenhof/CircleImageView). Transition animators are run on this widget.

**ImageTransitionUtils:** Provides [SharedElementCallback](https://developer.android.com/reference/android/support/v4/app/SharedElementCallback.html) that is used to set values accessed in `ImageTransition`.

**ImageTransitionCompat:** *under development* [Transition](https://developer.android.com/reference/android/support/transition/Transition.html) based on [ChangeBounds](https://developer.android.com/reference/android/support/transition/ChangeBounds.html) that provides animation support between a circular and rectangular ImageView (implemented as TransitionImageView) residing in the same activity or fragment. Requires API version >= 14. 

Usage
-----

1. Use com.appeaser.imagetransitionlibrary.TransitionImageView in place of ImageView. The `rounding` value can be set using `app:tiv_rounding` attribute. Value must be within [0,1] - **0** for no rounding, **1** for perfect rounding. Set `android:transitionName` attribute.

```
<com.appeaser.imagetransitionlibrary.TransitionImageView
    android:id="@+id/image"
    android:layout_width="80dp"
    android:layout_height="80dp"
    android:scaleType="centerCrop"
    android:transitionName="@string/iv_transition_name"
    app:tiv_rounding="0"/>
```    

Note: `com.appeaser.imagetransitionlibrary.TransitionImageView` only works with `scaleType="centerCrop"`. This restriction has been inherited from Henning Dodenhof's [CircleImageView](https://github.com/hdodenhof/CircleImageView).        

2. Provide `@transition/itl_image_transition` as the value for `android:windowSharedElementEnterTransition` & `android:windowSharedElementExitTransition` under your Activity theme in `styles.xml`:

```
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
	<item name="colorPrimary">...</item>
    <item name="colorPrimaryDark">...</item>
    <item name="colorAccent">...</item>

    ....
    ....

    <!-- @transition/itl_image_transition is provided by ImageTransition library -->
    <item name="android:windowSharedElementEnterTransition">@transition/itl_image_transition</item>
    <item name="android:windowSharedElementExitTransition">@transition/itl_image_transition</item>
</style>
```

This should ideally be placed in your `values-v21/styles.xml` file. Refer to the sample app for an example.

3. In the second Activity's `onCreate(Bundle)` method, add the following line of code:

```
public class SecondActivity extends AppCompatActivity {

    ....
    ....

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        ....
        ....

        // SharedElementCallback needs be set in the second Activity.
        // See ImageTrainsitionUtil for more info.
        setEnterSharedElementCallback(ImageTransitionUtil.DEFAULT_SHARED_ELEMENT_CALLBACK);
    }

    ....
    ....

    @Override
    public void onBackPressed() {
        supportFinishAfterTransition();
        super.onBackPressed();
    }
}
```

The sample app shows this approach.

Note: `ImageTransitionUtil.DEFAULT_SHARED_ELEMENT_CALLBACK` can be used only when transitioning from `tiv_rounding="1"` to `tiv_rounding="0"`. If you're transitioning between any other values, use:

    setEnterSharedElementCallback(ImageTransitionUtil
        .prepareSharedElementCallbackFor(_your start rounding value_, _your end rounding value_));

If you'd like to change the duration of the transition, or use the transition within your own set of transitions, or use a different interpolator, include the following:

```
<transitionSet
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:interpolator="your interpolator">

    ....
    ....

    <transition
        class="com.appeaser.imagetransitionlibrary.ImageTransition"
        android:duration="your duration" />

    ....
    ....

</transitionSet>
```

The library declares its min SDK version as 14. If your app's *minSdkVersion* is set to a lower value, include the following in your app's manifest: 

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="...">

    <uses-sdk tools:overrideLibrary="com.appeaser.imagetransitionlibrary"/>

    <application ...>
        ...
        ...
    </application>

</manifest>
```

API version requirements
------------------------
For image transition between activities, version 21 or above is required.
*Under development* For image transition within an activity, use `ImageTransitionCompat` which works on API 14 and above. Note that `ImageTransitionCompat` will not work if defined in XML.

Acknowledgements
----------------

As mentioned above, the library depends on Henning Dodenhof's [CircleImageView](https://github.com/hdodenhof/CircleImageView), released under Apache Licence, version 2.0. Many thanks to Henning Dodenhof for open-sourcing their work.

The example app uses [Picasso](http://square.github.io/picasso/), released under Apache Licence, version 2.0. 

License
-------
    Copyright (c) 2016 Vikram Kakkar

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

         http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
	
	
	
[1]: https://play.google.com/store/apps/details?id=com.appeaser.imagetransition