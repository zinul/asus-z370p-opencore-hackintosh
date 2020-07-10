## OpenCore configuration for an Asus Prime Z370-P motherboard and UHD 630 iGPU graphics
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

### Notes
- Configuration works with 10.15.4, but results in black screen when using 10.15.5. If you want to upgrade beyond 10.15.4, you will need to modify the properties in:
*config.plist > Root > DeviceProperties > Add > PciRoot(0x0)/Pci(0x2,0x0). Either framebuffer patching or busID patching will likely fix this problem.*
