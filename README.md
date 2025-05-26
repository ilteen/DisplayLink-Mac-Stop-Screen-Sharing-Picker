# Get rid of the annoying purple "DisplayLink Manager is recoding your screen" icon in the menu bar on macOS >= 14

This is the issue: https://www.displaylink.org/forum/showthread.php?p=96680  
It's officially descriped as a "feature": https://support.displaylink.com/knowledgebase/articles/2007602-macos-sonoma-14-screen-sharing-picker-and-stop-sh  
This fix is all thanks to lockieluke's comment: https://github.com/lwouis/alt-tab-macos/issues/2606#issuecomment-2042674340  

This works, because Terminal (an Apple app) will now launch DisplayLink Manager, instead of DisplayLink Manager itself (a 3rd party app).  
Apple apps don't show the fucking purple icon, that's why this works.    

1. Create a file at `~/Library/LaunchAgents/local.displaylink.plist` and paste this in:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>Label</key>
	<string>local.displaylink</string>
	<key>ProgramArguments</key>
	<array>
		<string>/bin/sh</string>
		<string>-c</string>
		<string>/usr/bin/osascript $HOME/.scripts/displaylink.scpt</string>
	</array>
	<key>RunAtLoad</key>
	<true/>
</dict>
</plist>
```

 2. Make sure you turn off **Launch at login** in DisplayLink Manager
3. Create a file at `~/.scripts/displaylink.scpt` and paste this in:

```applescript
tell application "Terminal"
	do script "screen -dmS DisplayLinkUserAgent /Applications/DisplayLink\\ Manager.app/Contents/MacOS/DisplayLinkUserAgent"
	delay 1
	if it is running then quit
end tell
```

 4. Run `launchctl load ~/Library/LaunchAgents/local.displaylink.plist` to load the launch agent profile you just created
5. Logout and log back in, you might see the Terminal flash for a fraction of a second as it's starting Displaylink but it should start working in a second

