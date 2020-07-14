## OpenCore EFI configurations for an Asus Prime Z370-P motherboard and UHD 630 iGPU graphics
### Hardware
- **Motherboard:** ASUS Prime Z370-P Intel Socket LGA 1151
- **CPU:** Intel i3-8100 Coffee Lake 8th Gen
- **GPU:** Intel UHD 630
        Supports DVI-D with max. resolution 1920 x 1200 @ 60 Hz
        Supports HDMI 1.4b with max. resolution 4096 x 2160 @ 24 Hz / 2560 x 1600 @ 60 Hz
        Maximum shared memory of 1024 MB (for iGPU exclusively)

- **SATA:** 2 x M.2 Socket 3, with M Key, type 2242/2260/2280 storage devices support (SATA mode & X4 PCIE mode)

- **LAN:** Realtek® RTL8111H, 1 x Gigabit LAN Controller(s)

- **Audio:** Realtek® ALC887 8-Channel High Definition Audio CODEC

- **USB:** 8 x USB 3.1 Gen 1 port(s) (4 at back panel, +blue)
        6 x USB 2.0 port(s) (2 at back panel, )

### Forcing RGB mode (EDID override)
A common problem encountered is a purple/magenta hue in the display, which is caused by Catalina thinking the monitor is a Television and sending out YPbPr signal instead of RGB, you can rectify this by following these steps and forcing RGB mode:
1. Disabling SIP by changing ```config.plist -> NVRAM -> Block -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> csr-active-config from 00000000 to FF070000```
2. Restarting the computer and clearing NVRAM to impliment the changes.
3. Mount drive as writable by running in terminal: ```sudo mount -uw /```
4. Run in terminal: ```killall Finder```
5. Download [patch-edid.rb](https://gist.github.com/adaugherity/7435890) by adaugherity and in terminal run: ```ruby <path-to-patch-edid.rb>```
6. Copy the generated folder "DisplayVendorID-****" into ```system -> Libary -> Displays -> Contents -> Resources -> Overrides``` replacing the existing folder (it would be wise to backup the original folder if you want to undo the changes later)
7. Restart computer and there should be no magenta hue to the screen and in in ```System Preferences -> Display -> Color``` there should be a new color profile listed called "your-monitor-name> - forced RGB mode (EDID overide)"

### System failing to wake
An initial problem faced was the system being unresponsive after sleeping, forcing the user to restart the machine. This problem can be solved by adding the boot argument: ```igfxonln=1``` to: ```config.plist -> NVRAM -> Block -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args```

### Black Screen in 10.15.4+
Configuration works with 10.15.4, but results in black screen when using 10.15.5, I have yet to research further into finding a fix for this. If you want to upgrade beyond 10.15.4, a likely fix will be found by modifying the properties found in:
```config.plist -> Root > DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0)```. Either framebuffer patching or busID patching will likely fix this problem.
