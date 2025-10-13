# Android SDK

### fedora-install
```sh
# Install dependencies
sudo dnf install -y wget unzip java-21-openjdk

app ${APPNAME} env

# Create SDK directory
sudo mkdir -p ${ANDROID_HOME}/cmdline-tools
sudo chown -R ${USER} ${ANDROID_HOME}

# Download and unzip command-line tools
wget https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip -O cmdline-tools.zip
unzip cmdline-tools.zip -d ${ANDROID_HOME}/cmdline-tools
mv ${ANDROID_HOME}/cmdline-tools/cmdline-tools $ANDROID_HOME/cmdline-tools/latest
rm cmdline-tools.zip

# Accept licenses
yes | sdkmanager --licenses

# Install SDK components
sdkmanager "platform-tools" "platforms;android-30" "build-tools;30.0.3"

# Download and unzip NDK
wget https://dl.google.com/android/repository/android-ndk-r26b-linux.zip -O ndk.zip
unzip ndk.zip -d ${ANDROID_HOME}/ndk
rm ndk.zip
```

### env
Set up environment variables

```sh evaluate
export ANDROID_HOME=/opt/android-sdk
export ANDROID_NDK_HOME=${ANDROID_HOME}/ndk/android-ndk-r26b
export PATH=$PATH:${ANDROID_HOME}/cmdline-tools/latest/bin:${ANDROID_HOME}/platform-tools:${ANDROID_NDK_HOME}
```
