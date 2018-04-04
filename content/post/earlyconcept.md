---
title: "The Early Concept of Abeona"
date: 2018-03-07T13:01:07+09:00
author: Octavio Putra
tags: ["Octavio", "Design", "Branding", "XML", "Sprint 1"]
---

Hello, my name is Tjokorde Gde Agung Octavio Putra and i am the hipster in this group along with Felicia. So far i've been dealing with several stuffs such as sketching the early mockups, making presentations, designing our logo and guidelines, and also a bit of coding for our app. On this post i would like to share some of my work for the past few weeks since the project began

## Early Sketches
From the requirements that our partner has given, i've made some quick sketches on how the app would look. First off is the android app itself. As a frequent traveler, i've had a vision on how the app should look like in order to ease travelers. First off is the chat menu itself

![Chat Dialog](/img/mockup/chat-dialog.png)
![Onboarding Dialog](/img/mockup/souvenir-request.png)

Taking inspiration from personal assistant apps like Siri and Ada, the chat screen should be a simple chat screen with user onboarding for handling user requests. With user onboarding, we can gather the user's preference without bothering them. Abby, our personal assistant's name, will give the user a list of items according to their request which the user will sort throught using simple gestures until the found one that they prefer.

![Product Page](/img/mockup/restaurant-detail.png)

When the user found the service that they want. We will then provide them the necessary details about the service which they requested. The details vary per product, but it generally provides information about the what the service is, how to get it, and the reviews around the product. There might also be special service specific information served in tabs such as menus for restaurants.  

![Admin Console](/img/mockup/admin-page.png)

Our personal assistant app wont be fully automated from start to finish, which is why our hard-working admins will need a comfortable console for them to work in. The admin console provides a simple interface for admins to chat with users that requested service along with all the tools they need to respond.

## Pitching Presentation
After designing the early sketches, the next step is to show it to our partner and the entire class. We wanted to share our vision about the whats, whys and hows surrounding our app. Which is why we opted for a presentation that is visually interesting and compelling in order to communicate our vision. The presentation can be viewed at [Canva](https://www.canva.com/design/DACvRb_HaDQ/ljR9Sj1Rk-Lbi_uHQIoKkA/view?utm_content=DACvRb_HaDQ&utm_campaign=designshare&utm_medium=link&utm_source=sharebutton)

## Mockup Creation
From now on its Felicia's time in the spotlight. Felicia has done a great work researching about user onboarding. Her research and hard work culminated in a mockup that's intuitive and easy to use. Even so, problems with her pc makes the end product look a little bit rushed so some polish might be needed in the future. Other that that, the mockup conveys our vision of onboarding perfectly.

## Overall Branding
During the mockup development, we also thought about the branding of our apps. We needed a logo, so i've designed one.

![First Logo](/img/abeona-old-2.png)
![First Logo](/img/abeona-old-1.png)

 The first batch of the logo is a simple mountain range surrounded a circle. The mountains represent an outward journey with its rounded edges that promotes a youthful appeal. The circle surrounding the mountain represents freedom and communion. The blue color represents intelligence, communication, and efficiency. The logotype under the logo represents our name in a clear, but also soothing way. But, since the logo was made in a hurry, it lacks impact and looks a little bland with an awkward look between the mountains and the circle surrounding it. Which is why I've revised the logo to fix these problems.

![Second Logo](/img/abeona-new.png)
![Second Logo](/img/abeona-new-2.png)
![Second Logo](/img/abeona-new-3.png)

In the second batch, the bottom edge of the mountain is conjoined to the circle to give some kind of connection between the shapes. I've also changed the logogram into a lighter font for a more elegant feel.

#### The Final Design

Finally, the design has reached its final stage. The research, design, and asset production has finally culminated in a finished product mockup that you can see below.
![Final Abeona Design](/img/final-design.png)

## Making a Splash Screen

My first hands on experience with coding our app is creating the splash screen view. The splash screen is the screen that appears between the user clicking the app and the app showing its first activity. This might seem counterproductive as the general consensus for UX is to not waste the user's time on useless views. But, with increasing app load times, this is a simple way to keep the user preoccupied during the load time, without adding to the load time itself. Which is why the best way to present a splash screen is not by creating a new activity, but by making it as an activity's theme background. To do this, first create an XML drawable in the drawable folder

```XML
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:drawable="@color/foreground_material_dark"/>

    <item>
        <bitmap
            android:gravity="center"
            android:src="@drawable/abeona_icon"
        />
    </item>

</layer-list>
```

Next, set this as the splash activity's background in the theme. For this, change the styles.xml file and add a new theme for the splash activity

```XML
<resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

    <style name="AppTheme.NoActionBar">
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
        <item name="android:textColorPrimary">@color/text_color_primary</item>
    </style>

    <style name="SplashTheme" parent="Theme.AppCompat.NoActionBar">
        <item name="android:windowBackground">@drawable/splash</item>
        <item name="android:colorForeground">@color/foreground_material_dark</item>
    </style>

    <style name="AppTheme.AppBarOverlay" parent="ThemeOverlay.AppCompat.Dark.ActionBar" />

    <style name="AppTheme.PopupOverlay" parent="ThemeOverlay.AppCompat.Light" />

</resources>
```

Afterwards, set the window background attribute to your XML drawable in the SplashTheme. Edit the AndroidManifest.xml

```XML
	<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="ga.abeona.abeona">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name=".SplashActivity"
            android:theme="@style/SplashTheme">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>


</manifest>
```

And finally the SplashActivity class itself

```Kotlin
class SplashActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val intent = Intent(this, MainActivity::class.java)
        startActivity(intent)
        finish()
    }
}
```

And now you have a splash screen that shows for exactly the same amount of time that it takes to load your app.

That's all for my progress for this week, see you in the next few weeks.
