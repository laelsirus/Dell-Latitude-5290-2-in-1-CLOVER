#  Dell Latitude 5290 2-in-1 UHD620 iGPU CLOVER

## Specifics

- CPU : Intel® Core™ i5-8350U Processor (6M Cache, up to 3.60 GHz)
- Graphics : Intel® UHD Graphics 620
- Sound : Realtek ALC3253 (ALC225)
- Display : 12.3 Inch 1920 X 1280 (WUXGA+) 3:2 10 Points Multi Touch
- Memory : Samsung LPDDR3 8GB 1867MHZ (4GB * 2 Dual Channel)
- SSD : TOSHIBA KXG60ZMV256G 256GB (2280), Western Digital PC SN520 NVMe SSD 512GB (2230)
- Wireless : BCM94352Z(DW1560) (WWAN Slot * 1)
- External Port & Slot : USB 3.2 Gen 1  * 2, TYPE C * 2 (USB 3.2 Gen 1, Displayport, Power Delivery), I2C PORT * 1, Audio Jack * 1, Smart Card Reader * 1 USIM Slot * 1, Noble Wedge Lock Slot * 1
- Battery : 42WHr
- Windows 10 Pro


## BIOS/Clover Bootloader/macOS Version

- BIOS : 1.12.1
- Clover Bootloader : Above v2.5k
- macOS : 10.14.X, 10.15.X


## BIOS Setup

- Load Optimized Defaults


## DSDT Patch

- 'XTBT (TBSE, CPGN)' to 'XTB2 (TBSE, CPGN)'
- Add 'Method (XTB2, 2)' just before 'Method (XTBT, 2, Serialized)'  
        Method (XTB2, 2)
        {
            XTBT (Arg0, Arg1)
        }
- [misc] Remove _PRW from LID
- [sys] AC Adapter Fix
- [sys] Add IMEI
- [sys] Fix _WAK Arg0 v2
- [sys] Fix Mutex with non-zero SyncLevel
- [sys] Fix PNOT/PPNT
- [sys] HPET Fix
- [sys] IRQ Fix
- [sys] OS Check Fix (Windows 10)
- [sys] RTC Fix
- [sys] Shutdown Fix v2
- [sys] SMBUS Fix
- [GPIO] GPIO Controller Enable [SKL+]
- Rename 'HECI' to 'IMEI'


## SSDT

- SSDT-DEEPIDLE.aml [For Thunderbolt 3 Model]
- SSDT-EC.aml
- SSDT-KEY-DELL-WN09.aml
- SSDT-PNLF.aml
- SSDT-PRTSC-F13.aml
- SSDT-RMNE.aml
- SSDT-TB3.aml [For Thunderbolt 3 Model]
- SSDT-TYPC.aml [For Thunderbolt 3 Model]
- SSDT-UIAC.aml
- SSDT-UPRW.aml


## Drivers64UEFI

- ApfsDriverLoader-64.efi
- FwRuntimeServices.efi
- OcQuirks.efi
- VBoxHfs-64.efi
- VirtualSmc.efi


## Kexts

- AirportBrcmFixup.kext
- AppleALC.kext
- BrcmBluetoothInjector.kext
- BrcmFirmwareData.kext
- BrcmPatchRAM3.kext
- CPUFriend.kext
- CPUFriendDataProvider.kext    -    Generated with one-key-cpufriend by stevezhengshiqi
- EFICheckDisabler.kext
- Lilu.kext
- NullEthernet.kext
- SMCBatteryManager.kext
- SMCLightSensor.kext
- SMCProcessor.kext
- SMCSuperIO.kext
- USBPorts.kext    -    Generated with Hackintool, HS / SS port matching and realignment
- VirtualSMC.kext
- VoodooI2C.kext
- VoodooI2CHID.kext
- VoodooPS2Controller.kext    -    Edit VoodooPS2Trackpad.kext's plist (ProductID:1218, VendorID:044E)
- WhateverGreen.kext

***CPUFriend.kext, and CPUFriendDataProvider.kext are not mandatory kext  
But creating it for your system will help you manage power***


## ETC

***After Installation***
- Turn off FileVault2 (necessary)  
- Remove these boot flags  
    -v  
    debug=0x100  
    keepsyms=1
- Additional patches are required for iMessage and Facetime activation
- HiDPI 1920 * 1280 (3840 * 2560) can be added, but it requires more resources
- It is recommended that you do a new ACPI patch on your system
- Rebuild kext cache for touchscreen activation
- Install Karabiner-Elements.app to activate Alps touchpad that is disabled after touchscreen activation

***Intel® Core™ i5-8350U Processor***
- CPUFriendDataProvider.kext has been modified to manage the operation of the 'Intel® Core ™ i5-8350U Processor'  
  If your CPU is not 'Intel® Core™ i5-8350U Processor', remove or regenerate the CPUFriendDataProvider.kext

***Intel® UHD Graphics 620***
- This build is compatible for 'Dell Latitude 5290 2-in-1' system uses iGPU of 'Intel® UHD Graphics 620'  
  If your iGPU is not 'Intel® UHD Graphics 620', additional graphics patches might be required

***Thunderbolt 3***
- Thunderbolt 3 needs testing

***NullEthernet.kext & ssdt-rmne.aml***
- Null Ethernet is a way to prevent a Mac address-based license for some software from being broken when a wireless card is absent or replaced (including iCloud)  
  If you do not need to consider blocking software licenses by changing your Mac address, you can remove it

***Fn Key***
- 'Fn' + 'r' || PrtScr = F13
- 'Fn' + 's' = F14 (Brightness down)
- 'Fn' + 'b' = F15 (Brightness up)
- 'Fn' + 'Esc', 'F1', 'F2', 'F3', 'F4', 'F6', 'F7', 'F10', 'PrtScr', 'Arrows'


## What Works

***Graphics/Display***
- Intel® UHD Graphics 620 QE/CI, 2048MB vRam
- Type C DP 2 ports Video / Audio output Hot Swap
- Brightness control
- Lid Close Sleep with Magnetic Travel Keyboard
- I2C touch screen Up to 5 points Gesture action (recognized as Magic Trackpad 2)

***Audio***
- Built-in speaker
- Built-in microphone
- Line input
- DP Audio Output

***Input***
- I2C touch screen Up to 5 points Gesture action (recognized as Magic Trackpad 2)
- I2C Keyboard (Magnetic Travel Keyboard) with Backlight
- Touchpad (Magnetic Travel Keyboard, recognized as mouse)
- Volume button, window button, power button

***Power Management***
- CPU/Speed Step
- Battery
- Type C PD 2 Ports Charging, PowerShare
- Sleep/Wake

***Storage Device***
- Full Size/Type C USB 2.0, 3.0 Hot Swap
- m.2 NVME 2280/ m.2 SATA 2280 1 Slot and m.2 NVME 2230(2242)/ m.2 SATA 2230(2242) 1 Slot

***Wireless communication***
- iMessage/FaceTime/App Store
- Wi-Fi, Bluetooth, Airdrop, Continuity with macOS compatible wireless card


## Issues
- Popping sound from Headphone Jack when entering audio idle state while battery in use  
  Set the Input to Line In once in the System Preference - Sound, it works normally again  
  This issue is not fully resolved yet

- Wireless Communication - DW1830(BCM943602BAED) is not recommended on this model  
  &ensp; Issues 1 : When the battery is in use, bluetooth not works properly after sleep  
  &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; It needs to reinstall the Bluetooth driver on Windows to get back to normal  
  &ensp; Issues 2 : If check 'Wake for Wi-Fi network access', wifi speed will be very slow after sleep  
  BCM94352Z(DW1560) does not have this issue, so it is recommended

- WWAN communication via WWAN card, USIM, and Legacy_Sierra_QMI.kext is feasible, but has not been tested yet

- Magic Trackpad 2 touch screen via VoodooI2C, VoodooI2CHID may not be recognized for new installations or OS updates  
  In this case touchscreen works after kext cache rebuild (sudo kextcache -i /)  
  Once recognized, the touch screen will not be lost  
  After recognized the touch screen, the Alps touchpad of the Magnetic Travel Keyboard is disabled, which can be activated using Karabiner  
  Check 'Alps Touchpad (Alps)' in 'Karabiner-Elements Preferences - Devices - Basic configuration'

- MicroSD slot not working properly  
  If you use modified Sinetek-rtsx.kext, you can use HFS+ formatted SD card, but there are still some problems

- I2C front and rear camera (AVStream2500, OV5670, OV8858) not recognized

- 3:2 resolution HiDPI of under 1913 * 1275 (3825 * 2550) not works through known method

- FileVault2 not works

## Screenshots

![01SystemOverview](https://user-images.githubusercontent.com/46496967/60284881-9ac76f80-9947-11e9-9127-c1dde7dc62b0.png)

![02SystemDisplay](https://user-images.githubusercontent.com/46496967/60283632-9cdbff00-9944-11e9-88d5-adb758d4b514.png)

![03VideoProc](https://user-images.githubusercontent.com/46496967/60283640-a1081c80-9944-11e9-979e-c31ffce3ceab.png)

![04IntelPowerGadget](https://user-images.githubusercontent.com/46496967/60283642-a1a0b300-9944-11e9-8b3e-e1a7283cd61f.png)

![05Geekbench_CPU](https://user-images.githubusercontent.com/46496967/60283643-a1a0b300-9944-11e9-98b4-fd5650762e0c.png)

![06Geekbench_GPU](https://user-images.githubusercontent.com/46496967/60283649-a2d1e000-9944-11e9-847e-7d6399875ad6.png)

**Origin USB Ports**
![07USBOrigin](https://user-images.githubusercontent.com/46496967/60283654-a36a7680-9944-11e9-8ca0-efb77f46b023.png)

**Edited USB Ports**
![08USBEdit](https://user-images.githubusercontent.com/46496967/60283655-a49ba380-9944-11e9-98ad-48ce1f903b61.png)
