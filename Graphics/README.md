# Graphics

## Tested on

- [X] 10.11
- [X] 10.13
- [X] 10.14

## Requirements - Acceleration (QE/CI)
- ig-platform-id set to ```0x19160000``` in clover.config > Graphics. Boot with invalid ig-platform-id during installation / first boot to prevent KP. Rebuild kextcache to get around this issue and get acceleration
- KextPatch in Clover, *only* Mac OS X 10.11:

```
<dict>
	<key>Comment</key>
	<string>Disable minStolenSize less or equal fStolenMemorySize assertion, 10.11.4 (based on Austere.J patch)</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>
	QTnEdio=
	</data>
	<key>InfoPlistPatch</key>
	<false/>
	<key>MatchOS</key>
	<string>10.11.x</string>
	<key>Name</key>
	<string>com.apple.driver.AppleIntelSKLGraphicsFramebuffer</string>
	<key>Replace</key>
	<data>
	QTnE6yo=
	</data>
</dict>
```

- KextPatch in Clover, *only* macOS 10.14:
```
<dict>
	<key>Comment</key>
	<string>Disable minStolenSize less or equal fStolenMemorySize assertion, 10.14 Credits (Sniki)</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>
	dkBI/wU=
	</data>
	<key>InfoPlistPatch</key>
	<false/>
	<key>MatchOS</key>
	<string>10.14.x</string>
	<key>Name</key>
	<string>com.apple.driver.AppleIntelSKLGraphicsFramebuffer</string>
	<key>Replace</key>
	<data>
	60BI/wU=
	</data>
</dict>
```

- Buggy graphics (usually occurs on 10.11) can be fixed by choosing the sRGB color scheme in Display settings

## Requirements - Brightness
- SSDT-PNLF.aml needed for brightness control, credit [Rehabman](https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightinjector-kext.218222/)
- For brightness keys I wrote this DSDT patch (With the help of [ACPIDebug](https://github.com/RehabMan/OS-X-ACPI-Debug). Only works if brightness keys call _Q14 and _Q15. More info [here](https://www.tonymacx86.com/threads/guide-patching-dsdt-ssdt-for-laptop-backlight-control.152659/)):

```
into method label _Q15 replace_content
begin
	Notify(\_SB.PCI0.LPC.KBD, 0x0405)\n
end;
into method label _Q14 replace_content
begin
	Notify(\_SB.PCI0.LPC.KBD, 0x0406)\n
end;
``` 

## Requirements - HiDPi

- Install [Lilu](https://github.com/acidanthera/Lilu) and [WhateverGreen](https://github.com/acidanthera/WhateverGreen)
- Enable HiDPi using ```sudo defaults write /Library/Preferences/com.apple.windowserver.plist DisplayResolutionEnabled -bool true```
- If you have the same DisplayVendorID(34a9) and DisplayProductID(96a2) as me you can use the display override file I've provided here (1600x900 HiDPi, 1867x1050 HiDPi, 1920x1080 HiDPi)
- If not you can use [this tool](https://comsysto.github.io/Display-Override-PropertyList-File-Parser-and-Generator-with-HiDPI-Support-For-Scaled-Resolutions/) to generate one yourself
- Place the display override file in /System/Library/Displays/Contents/Resources/Overrides/DisplayVendorID-XXXX/DisplayProductID-XXXX
- If the resolutions aren't showing in system preferences, try [RDM](https://github.com/avibrazil/RDM)
