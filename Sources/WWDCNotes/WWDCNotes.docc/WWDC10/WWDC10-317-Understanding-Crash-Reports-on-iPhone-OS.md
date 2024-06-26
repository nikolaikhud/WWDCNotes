# Understanding Crash Reports on iPhone OS

Even the best apps crash sometimes. Diagnosing and fixing your app's crashes can make the difference between improving it or losing your customers. Learn the skills you need to identify and fix your crashes and avoid common pitfalls.

@Metadata {
   @TitleHeading("WWDC10")
   @PageKind(sampleCode)
   @CallToAction(url: "https://developer.apple.com/devcenter/download.action?path=/videos/wwdc_2010__hd/session_317__understanding_crash_reports_on_iphone_os.mov", purpose: link, label: "Watch Video")

   @Contributors {
      @GitHubUser(zntfdr)
   }
}



## What are crash reports?

- When an application crashes, iOS writes out a Crash Report.
- This report contains diagnostic information that helps debug your application

Example of crash report

```shell
Incident Identifier: E6103EB1-FCF8-4887-AABE-7D2CE54DC204
CrashReporter Key:   7a62337857aa7d30d6bac1f9605c803f8a80d7d5
Hardware Model:      iPod3,1
Process:         SeismicXML [869]
Path:            /var/mobile/Applications/DBF2A211-F638-4D15-8A47-CD95C925FC38/SeismicXML.app/SeismicXML
Identifier:      SeismicXML
Version:         ??? (???)
Code Type:       ARM (Native)
Parent Process:  launchd [1]

Date/Time:       2010-05-21 16:35:34.652 -0700
OS Version:      iPhone OS 4.0 (8A274b)
Report Version:  104

Exception Type:  EXC_BAD_ACCESS (SIGBUS)
Exception Codes: KERN_PROTECTION_FAILURE at 0x00000000
Crashed Thread:  6

Thread 0:
0     libSystem.B.dylib          0x31227688 0x31226000 + 5768
1     libSystem.B.dylib          0x31229754 0x31226000 + 14164
2     CoreFoundation             0x325062c8 0x32494000 + 467656
3     CoreFoundation             0x32508582 0x32494000 + 476546
4     CoreFoundation             0x324b1841 0x32494000 + 121060
5     CoreFoundation             0x324b17ec 0x32494000 + 120812
6     GraphicsServices           0x304196f0 0x30416000 + 14064
7     GraphicsServices           0x3041979c 0x30416000 + 14236
8     UIKit                      0x326100e2 0x3260a000 + 24802
9     UIKit                      0x3260ecac 0x3260a000 + 19628
10    SeismicXML                 0x00002138 0x1000 + 4408
11    SeismicXML                 0x0000zobc 0x1000 + 4284

Thread 6 Crashed:
0     libobjc.A.dylib            0x348586e1 0x34e83000 + 10350
1     Foundation                 0x32144e6e 0x32139000 + 48750
2     Foundation                 0x321c9bb2 0x32139000 + 592818
3     libSystem.B.dylib          0x312a09b6 0x31226000 + 502198
4     libSystem.B.dylib          0x31296114 0x31226000 + 459028

Thread 6 crashed with ARM Thread State:
     r0: 0x32145b8d     r1: 0x3372602c     r2: 0x32145b8d     r3: 0x3372602c
     r4: 0x00b580b4     r5: 0x00000000     r6: 0x00006e07     r7: 0x06362da4
     r8: 0x321c97ed     r9: 0x001fc098    r10: 0x2fffe9e4    r11: 0x00000000
     ip: 0x3e4049c8     sp: 0x06362d6c     lr: 0x00003308     pc: 0x34e8586e
   cpsr: 0x00000030  

Binary Images:
0x1000 -     Ox7fff +SeismicXML armv6     <558ca043dbc407b84430fcazdaa1a63b> /var/mobile/Applications/DBF2A211-F638-4D15-8A47-CD95C925FC38/SeismicXML.app/SeismicXML
0x54c000 -   0x54dfff dns.so armv7        <240b8d3f07b4fcb234de598f867dela> /usr/lib/info/dns.so
0x2fe00000 - 0x2fe26fff dyld armv7        <a5c24f87740fd36d5d75d09f7e5577f8> /usr/lib/dyld
0x30005000 - 0x300adfff QuartzCore armv7  <30b70b64405d90493d485668c32572d8> /System/Library/Frameworks/QuartzCore.framework/QuartzCore
0x30188000 - 0x3018ffff libbz2.1.0.dylibarmv7  <5d079712f5a39708647292bccbd4c4e0> /usr/lib/libbz2.1.0.dylib
0x3022e000 - 0x3026cfff libVDSP.dylibarmv7  <cc8d6be7a5021266e26ebd05e9579852> /System/Library/Frameworks/Accelerate.framework/Frameworks/vecLib.framework/libvDSP.dylib
```

The first section of the crash report is the Process Information section:

```shell
Incident Identifier: E6103EB1-FCF8-4887-AABE-7D2CE54DC204 # Unique for each crash report
CrashReporter Key:   7a62337857aa7d30d6bac1f9605c803f8a80d7d5 # Uniquely identify a device (anonymized)
Hardware Model:      iPod3,1 # hardware model
Process:         SeismicXML [869] # crashed process, usually your application, And its pid.
Path:            /var/mobile/Applications/DBF2A211-F638-4D15-8A47-CD95C925FC38/SeismicXML.app/SeismicXML
Identifier:      SeismicXML
Version:         ??? (???)
Code Type:       ARM (Native)
Parent Process:  launchd [1]
```

The next section gives you some basic information about the crash:

```shell
Date/Time:       2010-05-21 16:35:34.652 -0700 # when it happened
OS Version:      iPhone OS 4.0 (8A274b) # the OS of the crash
Report Version:  104
```

The next is the Exception section:

```shell
Exception Type:  EXC_BAD_ACCESS (SIGBUS) # The type of crash
Exception Codes: KERN_PROTECTION_FAILURE at 0x00000000 # The exception code
Crashed Thread:  6 # The thread where the exception happened
```

The next sections are the threads backtraces. 

This is the backtrace of thread number 6:

```shell
Thread 6 Crashed:
0     libobjc.A.dylib            0x348586e1 0x34e83000 + 10350
1     Foundation                 0x32144e6e 0x32139000 + 48750
2     Foundation                 0x321c9bb2 0x32139000 + 592818
3     libSystem.B.dylib          0x312a09b6 0x31226000 + 502198
4     libSystem.B.dylib          0x31296114 0x31226000 + 459028
```

What is a backtrace?

- A backtrace lists all the active frames at the point of the crash
- it will give you a nested list of function calls that we'll meet when this crash occurred
- It is divided into four columns
  - The first column will give you the frame number
  - The second column will give the name of the binary for that frame (Foundation, etc)
  - The third column will give you the address of the function that was called
  - The fourth column divides this address, from the third column, into a base and an offset

Here are the four columns:

```shell
#1    #2                         #3        #4  
0     libobjc.A.dylib            0x348586e 0x34e83000 + 10350
```

The next section is the thread state, which shows the values and the registers at the time of the crash.

```shell
Thread 6 crashed with ARM Thread State:
     r0: 0x32145b8d     r1: 0x3372602c     r2: 0x32145b8d     r3: 0x3372602c
     r4: 0x00b580b4     r5: 0x00000000     r6: 0x00006e07     r7: 0x06362da4
     r8: 0x321c97ed     r9: 0x001fc098    r10: 0x2fffe9e4    r11: 0x00000000
     ip: 0x3e4049c8     sp: 0x06362d6c     lr: 0x00003308     pc: 0x34e8586e
   cpsr: 0x00000030  
```

The last section, called Binary Images, lists out all the binaries that were loaded at the time of the crash:

```shell
Binary Images:
0x1000 -     Ox7fff +SeismicXML armv6     <558ca043dbc407b84430fcazdaa1a63b> /var/mobile/Applications/DBF2A211-F638-4D15-8A47-CD95C925FC38/SeismicXML.app/SeismicXML
0x54c000 -   0x54dfff dns.so armv7        <240b8d3f07b4fcb234de598f867dela> /usr/lib/info/dns.so
0x2fe00000 - 0x2fe26fff dyld armv7        <a5c24f87740fd36d5d75d09f7e5577f8> /usr/lib/dyld
0x30005000 - 0x300adfff QuartzCore armv7  <30b70b64405d90493d485668c32572d8> /System/Library/Frameworks/QuartzCore.framework/QuartzCore
0x30188000 - 0x3018ffff libbz2.1.0.dylibarmv7  <5d079712f5a39708647292bccbd4c4e0> /usr/lib/libbz2.1.0.dylib
0x3022e000 - 0x3026cfff libVDSP.dylibarmv7  <cc8d6be7a5021266e26ebd05e9579852> /System/Library/Frameworks/Accelerate.framework/Frameworks/vecLib.framework/libvDSP.dylib
```

## Types of crash reports

- iPhone OS policy: iOS has several policies that every app must follow, otherwise the app will terminated. These are:
  - Watchdog timeout - happens when the app fails to Launch/Resume/Suspend/Quit within the given timeframe
  - User force-quit - happens when the user kills your app
  - Low Memory termination - If the OS realizes that it doesn't have enough memory to continue with interaction with the user, it will send out this low memory notification to applications. But if there isn't enough memory and if the application doesn't free up more memory, then your application will be terminated.

- Application crashes due to bugs

### Example of watchdog timeout crash report

Focusing on the Exception section

```shell
Exception Type: 00000020 
Exception Codes: 0x8badf00d 
Highlighted Thread: 0

Application Specific Information:
com.yourcompany.SeismicXML failed to resume in time
elapsed total CPU time (seconds): 1.030 (user 0.400, system 0.630), 5% 
CPU elapsed application CPU time (seconds): 0.000, 0% CPU
```

- watchdog timeout exceptions have exception code `0x8badf00d`
- the thread where this exception occurred, thread `0`, is the main thread
- the <kbd>Application Specific Information</kbd> tells you what your application was trying to do at the time

### Example of user force-quit crash report

Focusing on the Exception section:

```shell
Exception Type: 00000020
Exception Codes: 0xdeadfa11
Highlighted Thread: 0
```

- user force-quit exceptions have exception code `0xdeadfa11`

### Example of Low Memory crash report

These look different from the other crash types, as they need to tell you more information about the memory situation of the device at the time of the crash:

```shell
Incident Identifier: D4E8B5B8-E2EA-4F60-9416-FOBD98DCEFCA
CrashReporter Key:   15dc3be0eedf85fb074fa175be304082079b99eb
Hardware Model:      iPhone2.1
OS Version:          iPhone 0S 4.0 (8A274b)
Date:                2010-06-01 15:08:15 -0700

Free pages:         581
Wired pages:        10409
Purgeable pages:    0
Largest process:    SeismicXMLMem

Processes
         Name                 UUID                    Count resident pages
   SeismicXMLMem <82789a92cdd1d50ac2b17aa21f2670e2>   38263 (jettisoned) (active)
        installd <ffda7ad4d075b2fdf02216c7e2df9888>     224
notification_pro <880cfd236aafbb1cfbd0787040bc7ced>      91
    syslog_relay <ac90e2e7d96c8ef042d1a6ebfdfa830b>      74
    syslog_relay <ac90e2e7d96c8ef042d1a6ebfdfa830b>      74
            ptpd <9ff6cd3817f1952339f1db5d946f6038>     307
    MobileSafari <c2e954c7fb0e6edaa67aa29224344579>    1873 (jettisoned)
            Maps <665a1fc907f8551a9352bc0522ee36ed>    1095 (jettisoned)
        BTServer <c358dcf48aa131eb66c1996e59809ce1>     242
MobileMusicPlaye <20df528c77dea995ce5bee36e17f59a1>     832 (jettisoned)
     Preferences <f53c80952070f73aadd91ad75c6f43e9>     692 (jettisoned)
MobileStorageMou <23bab46f41cc216fbf66efbaa1786618>     123
            iapd <aa978437f697480fffbbb9b123951d96>     266
      MobileMail <94c71d196ac73109bb6daccie7f2ad10>     985 (jettisoned)
     MobilePhone <36f0c3714659d054ebe7326fe1d1731d>     816 (jettisoned)
             lsd <35af413c617c595603182e83b5403a71>     224
         notifyd <3ecc1a4ab4fd07980d0e6deb38fc5ef9>      82
     CommeCenter <ebaaac70a366bff399c5e0bd740f2d07>     318
     SpringBoard <66c8d430dd0f0bf611428ad1f584321f>    3890 (active)
          config <ab999221c9061690fabd93859a2306a7>     358
   fairplavd.N88 <91988ebaec26a3ced3d0bafc5a1c2195>      86
```

The first section give you basic process information:

```shell
Incident Identifier: D4E8B5B8-E2EA-4F60-9416-FOBD98DCEFCA
CrashReporter Key:   15dc3be0eedf85fb074fa175be304082079b99eb
Hardware Model:      iPhone2.1
OS Version:          iPhone 0S 4.0 (8A274b)
Date:                2010-06-01 15:08:15 -0700
```

The second section, more specific to low memory logs, tell you how many free pages there were at the time of the crash.

```shell
Free pages:         581
Wired pages:        10409
Purgeable pages:    0
Largest process:    SeismicXMLMem
```

- One page is 4 kilobytes
- So in this case there were `581` pages free, which is around 2 MB

The last section will give you a list of all the processes and how much memory each of them was using:

```shell
Processes
         Name                 UUID                    Count resident pages
   SeismicXMLMem <82789a92cdd1d50ac2b17aa21f2670e2>   38263 (jettisoned) (active)
        installd <ffda7ad4d075b2fdf02216c7e2df9888>     224
notification_pro <880cfd236aafbb1cfbd0787040bc7ced>      91
    syslog_relay <ac90e2e7d96c8ef042d1a6ebfdfa830b>      74
    syslog_relay <ac90e2e7d96c8ef042d1a6ebfdfa830b>      74
            ptpd <9ff6cd3817f1952339f1db5d946f6038>     307
    MobileSafari <c2e954c7fb0e6edaa67aa29224344579>    1873 (jettisoned)
            Maps <665a1fc907f8551a9352bc0522ee36ed>    1095 (jettisoned)
        BTServer <c358dcf48aa131eb66c1996e59809ce1>     242
MobileMusicPlaye <20df528c77dea995ce5bee36e17f59a1>     832 (jettisoned)
     Preferences <f53c80952070f73aadd91ad75c6f43e9>     692 (jettisoned)
MobileStorageMou <23bab46f41cc216fbf66efbaa1786618>     123
            iapd <aa978437f697480fffbbb9b123951d96>     266
      MobileMail <94c71d196ac73109bb6daccie7f2ad10>     985 (jettisoned)
     MobilePhone <36f0c3714659d054ebe7326fe1d1731d>     816 (jettisoned)
             lsd <35af413c617c595603182e83b5403a71>     224
         notifyd <3ecc1a4ab4fd07980d0e6deb38fc5ef9>      82
     CommeCenter <ebaaac70a366bff399c5e0bd740f2d07>     318
     SpringBoard <66c8d430dd0f0bf611428ad1f584321f>    3890 (active)
          config <ab999221c9061690fabd93859a2306a7>     358
   fairplavd.N88 <91988ebaec26a3ced3d0bafc5a1c2195>      86
```

- the third column here how many pages each process was using.
- the third column also tells you the state of each process
- jettisoned means that the OS is purging the process memory

Tips:

- Respect low memory notifications
- Release objects that can be reconstructed
- Release cached objects

## Acquire crash reports

- If you're the developer of the app and you're testing a debug build, you will find the crash logs in Xcode's Organizer.
- If you're distributing the debug app to a few users, they will need to sync the phone with their machine, and the logs will e located at the following paths:
  - Mac OS X: `~/Library/Logs/CrashReporter/MobileDevice/<DEVICE_NAME>`
  - Windows XP: `C:\Documents and Settings\<USERNAME>\Application Data\Apple Computer\Logs\CrashReporter\MobileDevice\<DEVICE_NAME>`
  - Windows Vista + 7: `C:\Users\<USERNAME>\AppData\Roaming\Apple Computer\Logs\CrashReporter\MobileDevice\<DEVICE_NAME>`
- For live apps, you can download crash reports from iTunes Connect (nowadays this is done automatically via Xcode's Organizer)

## Symbolicate crash reports

What is symbolication?  

- The process of going from addresses in the thread backtrace to symbol names
- helps us understand these crash logs better
- lets us jump into the line of code that caused this crash.

Unsymbolicated Crash Log:

```shell
Thread 0:
0   libSystem.B.dylib  0x31227688 0x31226000 + 5768
1   libSystem.B.dylib  0x31229754 0x31226000 + 14164
2   CoreFoundation     0x325062c8 0x32494000 + 467656 
3   CoreFoundation     0x32508582 0x32494000 + 476546 
4   CoreFoundation     0x324b18e4 0x32494000 + 121060 
5   CoreFoundation     0x324b17ec 0x32494000 + 120812 
6   CFNetwork          0x30569228 0x30501000 + 426536
7   Foundation         0x3217bcec 0x32139000 + 273644
8   SeismicXML         0x000029d4 0x1000 + 6612
```

Symbolicated Crash Log:

```shell
Thread 0:
0   libSystem.B.dylib 0x00001688 mach_msg_trap + 20
1   libSystem.B.dylib 0x00003754 mach_msg (mach_msg.c:99)
2   CoreFoundation    0x000722c8 __CFRunLoopServiceMachPort (CFRunLoop.c:1837)
3   CoreFoundation    0x00074582 __CFRunLoopRun (CFRunLoop.c:1961)
4   CoreFoundation    0x0001d8e4 CFRunLoopRunSpecific (CFRunLoop.c:2294)
5   CoreFoundation    0x0001d7ec CFRunLoopRunInMode (CFRunLoop.c:2316)
6   CFNetwork         0x00068228 CFURLConnectionSendSynchronousRequest + 244
7   Foundation        0x00042cec +[NSURLConnection sendSynchronousRequest:returningResponse:error:] + 76
8   SeismicXML        0x000029d4 -[SeismicXMLAppDelegate applicationDidFinishLaunching:] (SeismicXMLAppDelegate.m:129)
```

If you look at the line with our app binary frame (the last stack trace line), we can see the function name, file name and line number:

```shell
8   SeismicXML        0x000029d4 -[SeismicXMLAppDelegate applicationDidFinishLaunching:] (SeismicXMLAppDelegate.m:129)
                                                # function name                           # file name             # line number
```

How to go from a unsymbolicated to symbolicated crash log? Xcode does it for you:

- drag the crash log into Xcode, and Xcode will do the symbolication for you.
- for Xcode to be able to do that, it needs access to your app binary and <kbd>.dSYM</kbd> for that specific version of the app

## Common crashes

- Over released objects
- `Null` pointer dereference
- Insert `nil` object into an array or dictionary

Tips:

- If you see a `EXC_BAD_ACCESS` exception type, it means that the app is trying to access memory that you don't own, this tells you this is some type of memory issue
- If you see `SIGABRT`, it means that your application has asked the kernel to kill it, and then the kernel comes back and kills your app. This happens when something throws an exception and it gets caught, and the kernel comes in and kills your app. In this case, you should look for the thread backtrace that says `abort`