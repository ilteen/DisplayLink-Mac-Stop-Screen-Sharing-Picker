# Get rid of the annoying purple "DisplayLink Manager is recoding your screen" icon in the menu bar on macOS >= 14

This is all thanks to lockieluke: https://github.com/lwouis/alt-tab-macos/issues/2606#issuecomment-2042674340

1. Create a file at `~/Library/LaunchAgents/local.displayllink.plist` and paste this in:

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
3. Create a file at `~/.scripts/alttab.scpt` and paste this in:

```applescript
tell application "Terminal"
	do script "screen -dmS DisplayLinkUserAgent /Applications/DisplayLink\\ Manager.app/Contents/MacOS/DisplayLinkUserAgent"
	delay 1
	if it is running then quit
end tell
```

 4. Run `launchctl load ~/Library/LaunchAgents/local.displaylink.plist` to load the launch agent profile you just created
5. Logout and log back in, you might see the Terminal flash for a fraction of a second as it's starting AltTab but it should start working in a second

