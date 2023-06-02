TWRP device tree for Redmi Note 11 Pro 4G ( VIVA )
===============================================

| Basic                   | Spec Sheet                                                                                                                     	|
| -----------------------:|:------------------------------------------------------------------------------------------------------------------------------ 	|
| Codename                | `viva`       																													|
| Device name             | `Redmi Note 11 Pro 4G`                                                                                             				|
| Model names             | `2201116TG` "EU" global model<br/>`2201116TI` India model                                                 						|
| CPU                     | Octa-core                                                                                                                      	|
| Chipset                 | Mediatek Helio G96 (12 nm)                                                                       								|
| GPU                     | Mali-G57 MC2                                                                                                                   	|
| Memory                  | 6/8 GB RAM                                                                                                                    	|
| Shipped Android Version | Android 11, 12, 13 (MIUI 13 - MIUI 14)                                                                             				|
| Storage                 | 64/128 GB                                                                                                                     	|
| Battery                 | Li-Po 5000 mAh, non-removable                                                                                                	|
| Display                 | Super AMOLED, 120Hz, 700 nits, 1200 nits (peak)                                                       							|
| Camera (Back)(Main)     | 108 MP, f/1.9, 26mm (wide), 1/1.52", 0.7µm, PDAF. 8 MP, f/2.2, 118˚ (ultrawide). 2 MP, f/2.4, (macro). 2 MP, f/2.4, (depth)  	|
| Camera (Front)          | 16 MP, f/2.5, (wide), 1/3.06" 1.0µm                                          													|

![image](https://fdn.gsmarena.com/imgroot/reviews/22/xiaomi-redmi-note-11-pro/lifestyle/-1024w2/gsmarena_015.jpg)

### BOARD_USES_RECOVERY_AS_BOOT

Keep in mind, `viva` has NO `recovery` partition.
Recovery is part of the boot partition, so it takes care of normal boot and recovery.

**SO MAKE SURE YOU HAVE A BACKUP `boot.img`**

### Building

Basic instructions. From there you'll need to research.

1. Get started with https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp/tree/twrp-12.1
   ```
   repo init -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp.git -b twrp-12.1
   repo sync -j5 --current-branch --no-clone-bundle --no-tags
   ```

1. Add this device tree to `device/xiaomi/viva`.
   ```
   mkdir -p device/xiaomi
   cd device/xiaomi
   git clone https://github.com/stuepz/device_xiaomi_viva-ofox.git viva
   cd ../..
   ```

1. Try an `eng` build.
   ```
   export ALLOW_MISSING_DEPENDENCIES=true; . build/envsetup.sh; lunch twrp_viva-eng
   make bootimage
   ```

1. You should now be able to flash `out/target/product/viva/boot.img`
   ```
   cd out/target/product/viva
   fastboot flash boot boot.img
   ```

### Experimental builds

#### 1. Flash *just* TWRP

https://github.com/stuepz/device_xiaomi_viva-ofox/releases
Find the `.img` files under a releases' assets.
And flash like normal. `fastboot flash boot twrp-boot-xxx.img`

#### 2. Install the ramdisk

Boot into TWRP, then:
- `adb push stock-boot.img /sdcard/stock-boot.img` (get this from an official OTA file).
- `adb push fox-boot-xxx.img /sdcard/fox-boot-xxx.img`
- Install the **Stock** `stock-boot.img` for your OS version. **DO NOT** reboot yet.
- Install the ramdisk from `/sdcard/twrp-boot-xxx.img`. **DO NOT** reboot yet.
- or Flash Current Recovery on Menu Recovery

#### 3. Rooting

Booting the phone normally:
- Open the Magisk app.
- Choose `/sdcard/boot-plus-fox.img` to be patched.
- Note the filename you get in the log `/sdcard/Download/magisk_patched-XXXXXXX.img`
- `adb reboot recovery`

When rebooted to TWRP:
- Install the file you got `/sdcard/Download/magisk_patched-XXXXXXX.img` to Boot.
- Reboot! You're done!
