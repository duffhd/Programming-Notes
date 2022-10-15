## Remove MacOS sandbox

Flutter MacOS apps have sandbox activated by default, which means that you can’t run processes with `Process.run()`.

To remove the sandbox inside the `macos/Runner` folder find every file which is terminated with `.entitlements`

Delete the key-value pair that contain:

```xml
<key>com.apple.security.app-sandbox</key>
<true/>
```

Save the files and sandboxing should be disable after re-compiling the program.