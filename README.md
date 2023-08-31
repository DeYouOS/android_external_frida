# Frida

Dynamic instrumentation toolkit for developers, reverse-engineers, and security
researchers. Learn more at [frida.re](https://frida.re/).

Two ways to install
===================

## 1. Install from prebuilt binaries

This is the recommended way to get started. All you need to do is:

    pip install frida-tools # CLI tools
    pip install frida       # Python bindings
    npm install frida       # Node.js bindings

You may also download pre-built binaries for various operating systems from
Frida's [releases](https://github.com/frida/frida/releases) page on GitHub.

## 2. Build your own binaries

### Dependencies

For running the Frida CLI tools, e.g. `frida`, `frida-ls-devices`, `frida-ps`,
`frida-kill`, `frida-trace`, `frida-discover`, etc., you need Python plus a
few packages:

    pip install colorama prompt-toolkit pygments

### Linux

    make

### Apple OSes

First make a trusted code-signing certificate. You can use the guide at
https://sourceware.org/gdb/wiki/PermissionsDarwin in the sections
“Create a certificate in the System Keychain” and “Trust the certificate
for code signing”. You can use the name `frida-cert` instead of `gdb-cert`
if you'd like.

Next export the name of the created certificate to relevant environment
variables, and run `make`:

    export MACOS_CERTID=frida-cert
    export IOS_CERTID=frida-cert
    export WATCHOS_CERTID=frida-cert
    export TVOS_CERTID=frida-cert
    make

To ensure that macOS accepts the newly created certificate, restart the
`taskgated` daemon:

    sudo killall taskgated

### Windows

    frida.sln

### Build

    export ANDROID_NDK_ROOT=~/android_sdk/ndk/25.2.9519653
    rm -rf build/frida-android-*/ && \
    rm -rf build/tmp-android-*/ && \
    make core-android-arm && make core-android-arm64 && \
    adb root && \
    adb remount && \
    adb push build/frida-android-arm64/bin/frida-server /system/bin/frida-server && \
    adb push build/frida-android-arm/lib/frida/32/frida-helper /system/bin/frida-helper-32 && \
    adb push build/frida-android-arm64/lib/frida/64/frida-helper /system/bin/frida-helper-64 && \
    adb push build/frida-android-arm/lib/frida/32/frida-agent.so /system/lib/frida-agent.so && \
    adb push build/frida-android-arm64/lib/frida/64/frida-agent.so /system/lib64/frida-agent.so && \
    adb reboot

    adb shell ps -A | grep frida-server | awk '{print $2}' | xargs adb shell kill -9 &&
    adb forward tcp:62001 tcp:62001 && frida -H 127.0.0.1:62001 --token deyousofrida -f assa.xxx.66 -l '/home/deyouos/block.js'

adb shell /system/bin/frida-server -l 127.0.0.1:62001
adb pull /sys/fs/selinux/policy && adb logcat -b events -d | audit2allow -p policy


(Requires Visual Studio 2022.)

See [https://frida.re/docs/building/](https://frida.re/docs/building/)
for details.

## Learn more

Have a look at our [documentation](https://frida.re/docs/home/).
