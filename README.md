## react-native-video

A `<Video>` component for react-native, as seen in
[react-native-login](https://github.com/brentvatne/react-native-login)!

Requires react-native >= 0.19.0

### Add it to your project

Run `npm i -S react-native-video`

#### iOS

Install [rnpm](https://github.com/rnpm/rnpm) and run `rnpm link react-native-video`.

If you would like to allow other apps to play music over your video component, add:

**AppDelegate.m**

```objective-c
#import <AVFoundation/AVFoundation.h>  // import

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  ...
  [[AVAudioSession sharedInstance] setCategory:AVAudioSessionCategoryAmbient error:nil];  // allow
  ...
}
```

#### Android

Install [rnpm](https://github.com/rnpm/rnpm) and run `rnpm link react-native-video`

Or if you have trouble using [rnpm](https://github.com/rnpm/rnpm), make the following additions to the given files manually:

**android/settings.gradle**

```
include ':react-native-video'
project(':react-native-video').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-video/android')
```

**android/app/build.gradle**

```
dependencies {
   ...
   compile project(':react-native-video')
}
```

**MainActivity.java**

On top, where imports are:

```java
import com.brentvatne.react.ReactVideoPackage;
```

Under `.addPackage(new MainReactPackage())`:

```java
.addPackage(new ReactVideoPackage())
```

### Note: In react-native >= 0.29.0 you have to edit `MainApplication.java`

**MainApplication.java** (react-native >= 0.29.0)

On top, where imports are:

```java
import com.brentvatne.react.ReactVideoPackage;
```

Under `.addPackage(new MainReactPackage())`:

```java
.addPackage(new ReactVideoPackage())
```

## Usage
使用方法
```javascript
// Within your render function, assuming you have a file called
// "background.mp4" in your project. You can include multiple videos
// on a single screen if you like.

<Video source={{uri: "background"}}   // Can be a URL or a local file. 视频源 可以是url或本地文件
       rate={1.0}                     // 0 is paused, 1 is normal. 播放速度 0 暂停 正常
       volume={1.0}                   // 0 is muted, 1 is normal.  音量  0 静音 1 正常
       muted={false}                  // Mutes the audio entirely.  完全静音
       paused={false}                 // Pauses playback entirely.  全部暂停播放
       resizeMode="cover"             // Fill the whole screen at aspect ratio.  按纵横比填充整个屏幕
       repeat={true}                  // Repeat forever. 循环播放
       playInBackground={false}       // Audio continues to play when app entering background. 当应用程序进入背景音频继续播放。
       playWhenInactive={false}       // [iOS] Video continues to play when control or notification center are shown.
       progressUpdateInterval={250.0} // [iOS] Interval to fire onProgress (default to ~250ms)
       onLoadStart={this.loadStart}   // Callback when video starts to load 视频装载开始回调函数
       onLoad={this.setDuration}      // Callback when video loads 视频装载回调
       onProgress={this.setTime}      // Callback every ~250ms with currentTime 每250ms 回调一次 
       onEnd={this.onEnd}             // Callback when playback finishes 播放完毕回调
       onError={this.videoError}      // Callback when video cannot be loaded 视频不能装载回调
       style={styles.backgroundVideo} />

// Later on in your styles..
var styles = StyleSheet.create({
  backgroundVideo: {
    position: 'absolute',
    top: 0,
    left: 0,
    bottom: 0,
    right: 0,
  },
});
```


## Android Expansion File Usage
## Android 扩展文件使用 
android apk 扩展文件介绍 http://blog.csdn.net/myarrow/article/details/7760579
每个APK可以有2个扩展文件，每个文件最大限制是2GB。为了减少用户的带宽消耗，最好使用压缩格式文件吧。 这两扩展文件具有不同的用途：
第一个被称为 main (主)扩展文件，该扩展文件保护您程序中需要用到的附加数据；
第二个被称为 patch 扩展(修补)文件，该文件是可选的，并且应该只包含一些不同版本的补丁数据。
```javascript
// Within your render function, assuming you have a file called
// "background.mp4" in your expansion file. Just add your main and (if applicable) patch version
//当你要播放一个在扩展文件中的“background.mp4”视频文件时，可以用 source={{uri: "background", mainVer: 1, patchVer: 0}
//在扩展文件中 查找 background.mp4 播放，主版本为1 补丁版本为0
<Video source={{uri: "background", mainVer: 1, patchVer: 0}} // Looks for .mp4 file (background.mp4) in the given expansion version.
       rate={1.0}                   // 0 is paused, 1 is normal.
       volume={1.0}                 // 0 is muted, 1 is normal.
       muted={false}                // Mutes the audio entirely.
       paused={false}               // Pauses playback entirely.
       resizeMode="cover"           // Fill the whole screen at aspect ratio.
       repeat={true}                // Repeat forever.
       onLoadStart={this.loadStart} // Callback when video starts to load
       onLoad={this.setDuration}    // Callback when video loads
       onProgress={this.setTime}    // Callback every ~250ms with currentTime
       onEnd={this.onEnd}           // Callback when playback finishes
       onError={this.videoError}    // Callback when video cannot be loaded
       style={styles.backgroundVideo} />

// Later on in your styles..
var styles = Stylesheet.create({
  backgroundVideo: {
    position: 'absolute',
    top: 0,
    left: 0,
    bottom: 0,
    right: 0,
  },
});
```

### Load files with the RN Asset System
### 使用  RN Asset System 装载视频文件
The asset system [introduced in RN `0.14`](http://www.reactnative.com/react-native-v0-14-0-released/) allows loading image resources shared across iOS and Android without touching native code. As of RN `0.31` [the same is true](https://github.com/facebook/react-native/commit/91ff6868a554c4930fd5fda6ba8044dbd56c8374) of mp4 video assets for Android. As of [RN `0.33`](https://github.com/facebook/react-native/releases/tag/v0.33.0) iOS is also supported. Requires `react-native-video@0.9.0`.
RN 0.33 版本以后 支持 装载 mp4 视频资源文件
```
<Video
  repeat
  resizeMode='cover'
  source={require('../assets/video/turntable.mp4')}
  style={styles.backgroundVideo}
/>
```

### Play in background on iOS

To enable audio to play in background on iOS the audio session needs to be set to `AVAudioSessionCategoryPlayback`. See [Apple documentation][3] for additional details. (NOTE: there is now a ticket to [expose this as a prop]( https://github.com/react-native-community/react-native-video/issues/310) )

## Static Methods

`seek(seconds)`

Seeks the video to the specified time (in seconds). Access using a ref to the component

## Examples

- See an [Example integration][1] in `react-native-login` *note that this example uses an older version of this library, before we used `export default` -- if you use `require` you will need to do `require('react-native-video').default` as per instructions above.*
- Try the included [VideoPlayer example][2] yourself:

   ```sh
   git clone git@github.com:brentvatne/react-native-video.git
   cd react-native-video/Examples/VideoPlayer
   npm install
   open VideoPlayer.xcodeproj

   ```

   Then `Cmd+R` to start the React Packager, build and run the project in the simulator.

- [Lumpen Radio](https://github.com/jhabdas/lumpen-radio) contains another example integration using local files and full screen background video.

## TODOS

- [ ] Add support for captions
- [ ] Add support for playing multiple videos in a sequence (will interfere with current `repeat` implementation)
- [ ] Callback to get buffering progress for remote videos
- [ ] Bring API closer to HTML5 `<Video>` [reference](http://devdocs.io/html/element/video)

[1]: https://github.com/brentvatne/react-native-login/blob/56c47a5d1e23781e86e19b27e10427fd6391f666/App/Screens/UserInfoScreen.js#L32-L35
[2]: https://github.com/react-native-community/react-native-video/tree/master/example
[3]: https://developer.apple.com/library/ios/qa/qa1668/_index.html

---

**MIT Licensed**
