# Build Apps with Termux

In this tutorial, you'll learn how to build APKs with Termux, without needing Proot.

> [!NOTE]
> This guide assumes familiarity with Termux and Gradle basics. The steps below will help you get set up quickly.


After facing challenges in building apps and testing multiple methods, I’ve put together the best way to do it. Just follow the steps below to get started with building your apps.

## Steps

### 1. Install Termux

[Termux](https://github.com/termux/termux-app/releases) is a powerful terminal app for Android, and it will serve as your development environment.

### 2. Install Required Packages 

Before setting up the Android SDK, you’ll need some essential packages in Termux. This tutorial uses `Java-17`.
```bash
pkg i curl openjdk-17 -y
```

### 3. Install Android SDK

Next, we need the Android SDK. You can find setup instructions in its [repository](https://github.com/Sohil876/termux-sdk-installer), or run the following command directly:
```bash
curl -sSL https://raw.githubusercontent.com/Sohil876/termux-sdk-installer/main/installer.sh -o ~/install-android-sdk.sh && bash ~/install-android-sdk.sh -i
```

Once the SDK is installed, you’ll see a new directory named `android-sdk` in your Termux files. This directory contains all necessary tools.

Before moving on, you must accept Google licenses:
```bash
yes | sdkmanager --licenses
```

Set the Android platform version to `34`:
```bash
yes | sdkmanager "platforms;android-34"
```

### 4. Install Gradle

You’ll need Gradle `v7.6.4`, which is compatible with the Java version installed. Download and set it up as follows:
```bash
# Download Gradle
curl -L https://downloads.gradle-ldn.com/distributions/gradle-7.6.4-bin.zip -o $ANDROID_HOME/gradle-7.6.4-bin.zip

# Unzip it
unzip $ANDROID_HOME/gradle-*-bin.zip -d $ANDROID_HOME/
mv $ANDROID_HOME/gradle-*/ $ANDROID_HOME/gradle/

# Add Gradle to your PATH
echo 'export PATH=${PATH}:${ANDROID_HOME}/gradle/bin' >> ~/.bashrc
source ~/.bashrc
```

Now you can run Gradle and the Android SDK for any task you need.

However, there’s a known issue with Gradle in Termux where it cannot find the `aapt2` build tool. To fix this, specify it in the global Gradle properties.

Go to the file `~/android-sdk/.gradle/gradle.properties` (create it if it doesn’t exist) and add:
```properties
android.aapt2FromMavenOverride=/data/data/com.termux/files/home/android-sdk/build-tools/<version>/aapt2
```
Replace `<version>` with the actual version you have in your SDK.

---

Congratulations! You can now build Android apps using Termux, without Proot. Enjoy coding!
