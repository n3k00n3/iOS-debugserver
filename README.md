# iOS-debugserver
LLDB debugserver for iOS with modified entitlements, allowing for debugging process.

```
Install Xcode.app
```


### Mount the developer diskimage

```
hdiutil mount /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport/16.0/DeveloperDiskImage.dmg
```


### Copy the debuserver to home folder (You decide the path)

```
cp /Volumes/DeveloperDiskImage/usr/bin/debugserver ~/
```

### Iproxy 

```
iproxy 2222 22
```


### Transfer debugerserver to Iphone

```
scp -P 2222 debugserver root@localhost:/private/var
```

#### Connect to Iphone
```
ssh root@localhost -p2222
```


---
## on Iphone

```
cd /private/var
vi entitlement.xml
```

**copy this file to entitlment.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>*</string>
    </array>

    <key>com.apple.springboard.debugapplications</key>
    <true/>

    <key>run-unsigned-code</key>
    <true/>

    <key>get-task-allow</key>
    <true/>

    <key>task_for_pid-allow</key>
    <true/>

    <key>platform-application</key>
    <true/>

    <key>com.apple.private.security.no-container</key>
    <true/>

    <key>com.apple.private.skip-library-validation</key>
    <true/>
</dict>
</plist>
```

### Sign the binary using ldid

```
ldid -Sentitlemnt.xml debugserver
chmod +x debugserver
```

### Searching the process

```
ps aux | grep jailbreak
```

### Run the debugserver

```
./debuserver 0.0.0.0:7777 --waitfor="path to process"
```



---
## On Desktop

### Connect using a macOS

```
lldb
```

#### Inside lldb

```
process connect connect:0.0.0.0:7777
```


## source

- [LLDB Manual](https://lldb.llvm.org/use/remote.html)
- [The Missing guide to debug third party apps on iOS](https://felipejfc.medium.com/the-ultimate-guide-for-live-debugging-apps-on-jailbroken-ios-12-4c5b48adf2fb)

