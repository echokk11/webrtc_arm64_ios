# Build of Webrtc for arm64 sim

Casue the google webrtc.xcfrmaework install by pod only compiled in x64, we can't run simulator in m1/m2 mac,
also we did't want run xcode in Rosetta mode. so build it ourself. hopes can help you.

在使用m1,m2 mac电脑开发ios时候如果用到google的webrtc库，直接使用pod安装会运行不起来，因为他上传的库只有x64的编译，所以我们为了不在打开Rosetta的情况下使用Xcode，自己编译可以提供给arm64模拟器以及真机都可以运行的库

> simulator and arm64 for m1 mac (including m1 pro,m1 max,m2... all Mac computers with Apple silicon)

![](Xnip2023-02-10_17-39-21.jpg)

#### step 1: get tools
```bash
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
export PATH=`pwd`/depot_tools:"$PATH"
```

#### step 2: get source of Webrtc
```bash
mkdir -p build-webrtc
cd build-webrtc
fetch --nohooks webrtc_ios  #this will take a long time
gclient sync
```

#### step 3: build
```bash
# for true device
./src/tools_webrtc/ios/build_ios_libs.sh --build_config release --arch arm64 -o ./src/out_ios_libs_arm64

# for simulator
./src/tools_webrtc/ios/build_ios_libs.sh --build_config release --arch simulator:arm64 -o ./src/out_ios_libs_sim_arm64

# merge
xcodebuild -create-xcframework -framework ./src/out_ios_libs_arm64/WebRTC.xcframework/ios-arm64/WebRTC.framework \
  -framework ./src/out_ios_libs_sim_arm64/WebRTC.xcframework/ios-arm64-simulator/WebRTC.framework \
  -output src/WebRTC.xcframework
```

#### step 4: use
drag `src/WebRTC.xcframework` to xcode and start build

## thanks
https://github.com/pnoyelle/WebRTC_IOS
