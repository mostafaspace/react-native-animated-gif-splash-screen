# Animated Gif Support react-native-splash-screen

A splash screen API for react-native which can programatically hide and show the splash screen. Works on iOS and Android.

## Installation

- Clone this project


### Configuration:

**Android:**

Update the `MainActivity.java` to use `react-native-splash-screen` via the following changes:

```java

import org.devio.rn.splashscreen.SplashScreen; // here

public class MainActivity extends ReactActivity {
   @Override
    protected void onCreate(Bundle savedInstanceState) {
        SplashScreen.show(this, R.id.splash_image_view, R.drawable.launch_screen);  // here
        super.onCreate(savedInstanceState);
    }
    // ...other code
}
```

**iOS:**

Update `AppDelegate.m` with the following additions:


```obj-c
#import "AppDelegate.h"

#import <React/RCTBundleURLProvider.h>
#import <React/RCTRootView.h>
#import "RNSplashScreen.h"  // here
#import "FLAnimatedImage.h" // here

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    // ...other code

  //SPLASH SCREEN
  NSURL *url = [[NSBundle mainBundle] URLForResource:@"splash" withExtension:@"gif"];//need to be aspectFill
  NSData *data = [NSData dataWithContentsOfURL:url];
  FLAnimatedImage *image = [FLAnimatedImage animatedImageWithGIFData:data];
  FLAnimatedImageView *imageView = [[FLAnimatedImageView alloc] init];
  imageView.contentMode = UIViewContentModeScaleAspectFill;
  imageView.animatedImage = image;
  
  [RNSplashScreen showSplash:@"LaunchScreen" inRootView:rootView loadingView:imageView];
  //SPLASH SCREEN

    return YES;
}

@end

```

## Getting started  

Import `react-native-splash-screen` in your JS file.

`import SplashScreen from 'react-native-splash-screen'`    

### Android:

Create a file called `launch_screen.xml` in `app/src/main/res/layout` (create the `layout`-folder if it doesn't exist). The contents of the file should be the following:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">
    <ImageView android:layout_width="match_parent" android:layout_height="match_parent" android:src="@drawable/launch_screen" android:scaleType="centerCrop" />
</RelativeLayout>
```

Customize your launch screen by creating a `launch_screen.png`-file and placing it in an appropriate `drawable`-folder. Android automatically scales drawable, so you do not necessarily need to provide images for all phone densities.
You can create splash screens in the following folders:
* `drawable-ldpi`
* `drawable-mdpi`
* `drawable-hdpi`
* `drawable-xhdpi`
* `drawable-xxhdpi`
* `drawable-xxxhdpi`

Add a color called `primary_dark` in `app/src/main/res/values/colors.xml`

```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="primary_dark">#000000</color>
</resources>
```


**Optional steps：**

If you want the splash screen to be transparent, follow these steps.

Open `android/app/src/main/res/values/styles.xml` and add `<item name="android:windowIsTranslucent">true</item>` to the file. It should look like this:

```xml
<resources>
    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- Customize your theme here. -->
        <!--设置透明背景-->
        <item name="android:windowIsTranslucent">true</item>
    </style>
</resources>
```

**To learn more see [examples](https://github.com/crazycodeboy/react-native-splash-screen/tree/master/examples)**


If you want to customize the color of the status bar when the splash screen is displayed:

Create `android/app/src/main/res/values/colors.xml` and add
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="status_bar_color"><!-- Colour of your status bar here --></color>
</resources>
```

Create a style definition for this in `android/app/src/main/res/values/styles.xml`:
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="SplashScreenTheme" parent="SplashScreen_SplashTheme">
        <item name="colorPrimaryDark">@color/status_bar_color</item>
    </style>
</resources>
```

Change your `show` method to include your custom style:
```java
SplashScreen.show(this, R.style.SplashScreenTheme);
```

### iOS    

Customize your splash screen via `LaunchScreen.storyboard` or `LaunchScreen.xib`。

**Learn more to see [examples](https://github.com/crazycodeboy/react-native-splash-screen/tree/master/examples)**

- [via LaunchScreen.storyboard Tutorial](https://github.com/crazycodeboy/react-native-splash-screen/blob/master/add-LaunchScreen-tutorial-for-ios.md)


## Usage

Use like so:

```javascript
import SplashScreen from 'react-native-splash-screen'

export default class WelcomePage extends Component {

    componentDidMount() {
    	// do stuff while splash screen is shown
        // After having done stuff (such as async tasks) hide the splash screen
        SplashScreen.hide();
    }
}
```

## API


| Method | Type     | Optional | Description                         |
|--------|----------|----------|-------------------------------------|
| show() | function | false    | Open splash screen (Native Method ) |
| hide() | function | false    | Close splash screen                 |

## Testing

### Jest

For Jest to work you will need to mock this component. Here is an example:

```
// __mocks__/react-native-splash-screen.js
export default {
  show: jest.fn().mockImplementation( () => { console.log('show splash screen'); } ),
  hide: jest.fn().mockImplementation( () => { console.log('hide splash screen'); } ),
}
```

## Troubleshooting 

### Splash screen always appears stretched/distorted
Add the ImageView with a scaleType in the `launch_screen.xml`, e.g.:
```
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:orientation="vertical"
>
  <ImageView 
    android:src="@drawable/launch_screen"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:scaleType="centerCrop"
  >
  </ImageView>
</FrameLayout>
```

---

**[MIT Licensed](https://github.com/crazycodeboy/react-native-splash-screen/blob/master/LICENSE)**
