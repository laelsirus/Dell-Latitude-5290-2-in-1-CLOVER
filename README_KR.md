# Dell Latitude 5290 2-in-1


## 기기 사양

- CPU : Intel® Core™ i5-8350U Processor (6M Cache, up to 3.60 GHz)
- Graphics : Intel® UHD Graphics 620
- Sound : Realtek ALC3253 (ALC225)
- Display : 12.3 Inch 1920 X 1280 (WUXGA+) 3:2 10 Points Multi Touch
- Memory : Samsung LPDDR3 8GB 1867MHZ (4GB * 2 Dual Channel)
- SSD : TOSHIBA KXG60ZMV256G 256GB (2280), Western Digital PC SN520 NVMe SSD 512GB (2230)
- Wireless : BCM94352Z(DW1560) (WWAN Slot * 1)
- Battery : 42WHr


## 바이오스/클로버 부트로더/macOS 버전

- 바이오스 : 1.12.1
- 클로버 부트로더 : v5.0 이상
- macOS : 10.14.X, 10.15.X


## 바이오스 셋업

- Load Optimized Defaults


## DSDT 패치

- syntax error 수정  

수정 전
```
                If (LEqual (PM6H, One))
                {
                    CreateBitField (BUF0, \_SB.PCI0._Y0C._RW, ECRW)  // _RW_: Read-Write Status
                    Store (Zero, ECRW (If (PM0H)
                            {
                                CreateDWordField (BUF0, \_SB.PCI0._Y0D._LEN, F0LN)  // _LEN: Length
                                Store (Zero, F0LN)
                            }))
                }
```
수정 후
```
                If (LEqual (PM6H, One))
                {
                    CreateBitField (BUF0, \_SB.PCI0._Y0C._RW, ECRW)  // _RW_: Read-Write Status
                    Store (Zero, ECRW)

                }

                If (PM0H)
                {
                    CreateDWordField (BUF0, \_SB.PCI0._Y0D._LEN, F0LN)  // _LEN: Length
                    Store (Zero, F0LN)
                }
```
- ```XTBT (TBSE, CPGN)``` 를 ```XTB2 (TBSE, CPGN)``` 으로 찾아 바꾸기
- 아래의 값을 ```Method (XTBT, 2, Serialized)``` 직전에 추가
```
        Method (XTB2, 2)
        {
            XTBT (Arg0, Arg1)
        }
```
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
- ```HECI``` 를 ```IMEI``` 으로 찾아 바꾸기
- BRT6 Method 수정 - 'Fn + F11, Fn + F12' 키로 밝기 제어  

수정 전
```
        Method (BRT6, 2, NotSerialized)
        {
            If (LEqual (Arg0, One))
            {
                Notify (LCD, 0x86)
            }

            If (And (Arg0, 0x02))
            {
                Notify (LCD, 0x87)
            }
        }
```
수정 후
```
        Method (BRT6, 2, NotSerialized)
        {
            If (LEqual (Arg0, One))
            {
                Notify (^^LPCB.PS2K, 0x0406)
            }

            If (And (Arg0, 0x02))
            {
                Notify (^^LPCB.PS2K, 0x0405)
            }
        }
```

***MaciASL 패치 소스가 필요합니다  
_RehabMan Laptop [http://raw.github.com/RehabMan/Laptop-DSDT-Patch/master]  
_VoodooI2C-Patches [http://raw.github.com/alexandred/VoodooI2C-Patches/master]***  
***커널 패닉을 방지하기 위해 본인의 기기에서 DSDT를 추출하여 패치하는 것을 권장합니다***


## SSDT

- SSDT-ALC225.aml
- SSDT-DEEPIDLE.aml ***[썬더볼트 3 모델 한정]***
- SSDT-EC-USBX.aml [USB 전력 제어]
- SSDT-PLUG.aml [PluginType=1]
- SSDT-PNLF.aml [밝기 제어]
- SSDT-PRTSC-F13.aml [PrtScr 키를 F13 키로 맵핑]
- SSDT-RMNE.aml [Null Ethernet]
- SSDT-TB3.aml ***[썬더볼트 3 모델 한정]***
- SSDT-TYPC.aml ***[썬더볼트 3 모델 한정]***
- SSDT-UPRW.aml [잠자기 상태에서 USB로 인한 깨우기 방지, 기타 USB 이슈 해결]
- SSDT-XOSI.aml [밝기 제어 키, 전원 버튼, 터치 스크린을 위한 OS Check Fix]


## 클로버 ACPI 핫패치

- change GFX0 to IGPU [내장 그래픽 Fix]
- change HDAS to HDEF [오디오 Fix]
- change HECI to IMEI
- change MEI to IMEI
- change ECDV to EC [USB Fix]
- change OSID to XSID [밝기 제어 키를 위한 OS Check Fix]
- change \_OSI to XOSI [밝기 제어 키를 위한 OS Check Fix]
- change UPRW to XPRW [잠자기 상태에서 USB로 인한 깨우기 방지]
- change GPRW to YPRW [잠자기 상태에서 USB로 인한 깨우기 방지]
- change \_RMV to XRMV [***썬더볼트 3 모델을*** 위한 Type C 핫 스왑 Fix]


## Drivers64UEFI

- ApfsDriverLoader.efi [acidanthera_AppleSupportPkg]
- OcQuirks.efi [ReddestDream_OcQuirks]
- OpenRuntime.efi [ReddestDream_OcQuirks]
- VBoxHfs.efi [acidanthera_AppleSupportPkg]
- VirtualSmc.efi [acidanthera_VirtualSMC]


## Kexts

- AirportBrcmFixup.kext
- AppleALC.kext
- BrcmBluetoothInjector.kext
- BrcmFirmwareData.kext
- BrcmPatchRAM3.kext
- CodecCommander.kext
- CPUFriend.kext
- CPUFriendDataProvider.kext    -    stevezhengshiqi의 one-key-cpufriend를 이용하여 생성
- EFICheckDisabler.kext
- Lilu.kext
- NullEthernet.kext
- SMCBatteryManager.kext
- SMCLightSensor.kext
- SMCProcessor.kext
- SMCSuperIO.kext
- USBPorts.kext    -    Hackintool을 이용하여 생성, HS/SS 포트 매칭과 재정렬
- VirtualSMC.kext
- VoodooI2C.kext
- VoodooI2CHID.kext
- VoodooPS2Controller.kext    -    /VoodooPS2Controller.kext/Contents/PlugIns/VoodooPS2Trackpad.* 제거
- WhateverGreen.kext

***CPUFriend.kext, CPUFriendDataProvider.kext는 필수 kext가 아니지만 본인의 시스템에 맞게 제작하면 전원 관리에 도움이 됩니다***


## 클로버 Boot Arguments

- dart=0 [사이드카 활성화]
- brcmfx-country=#a [범용 국가 코드로 지정]


## 클로버 Devices-Properties

- 오디오 레이아웃 설정, 디스플레이 오디오 활성화
```
            <key>PciRoot(0x0)/Pci(0x1f,0x3)</key>
            <dict>
                <key>device-id</key>
                <data>cKEAAA==</data>
                <key>layout-id</key>
                <data>HgAAAA==</data>
            </dict>
```
- Intel® UHD Graphics 620
```
            <key>PciRoot(0x0)/Pci(0x2,0x0)</key>
            <dict>
                <key>AAPL,GfxYTile</key>
                <data>AQAAAA==</data>
                <key>AAPL,ig-platform-id</key>
                <data>AgAmWQ==</data>
                <key>device-id</key>
                <data>FlkAAA==</data>
                <key>dpcd-max-link-rate</key>
                <data>CgAAAA==</data>
                <key>enable-dpcd-max-link-rate-fix</key>
                <data>AQAAAA==</data>
                <key>enable-hdmi20</key>
                <data>AQAAAA==</data>
                <key>framebuffer-con0-alldata</key>
                <data>AAASAAIAAACYAAAAAQUSAAAEAADHAwAAAgQSAAAEAADHAwAA</data>
                <key>framebuffer-con0-enable</key>
                <data>AQAAAA==</data>
                <key>framebuffer-fbmem</key>
                <data>AACQAA==</data>
                <key>framebuffer-patch-enable</key>
                <data>AQAAAA==</data>
                <key>framebuffer-stolenmem</key>
                <data>AAAwAQ==</data>
                <key>framebuffer-unifiedmem</key>
                <data>AAAAgA==</data>
            </dict>
```


## ETC

***설치 이후***
- 아래의 Boot Arguments 제거  
    -v  
    debug=0x100  
    keepsyms=1
- 아이메세지와 페이스타임 활성화를 위해 추가적인 패치가 필요합니다 (Board Serial Number, Serial Number, SmUUID)
- 터치 스크린의 활성화를 위해 켁스트 캐시를 리빌드하세요 [sudo kextcache -i /]
- 터치 스크린 활성화 후 비활성화된 Alps Touchpad의 활성화를 위해 Karabiner-Elements.app을 설치하세요
- 1920 * 1280 (3840 * 2560)의 HiDPI 를 추가할 수 있지만, 많은 전력을 사용합니다

***Intel® Core™ i5-8350U Processor***
- CPUFriendDataProvider.kext는 'Intel® Core ™ i5-8350U Processor'를 위해 수정되었습니다  
  'Intel® Core™ i5-8350U Processor'가 아니라면, CPUFriendDataProvider.kext을 삭제하거나 재생성하세요

***NullEthernet.kext & SSDT-RMNE.aml***
- Null Ethernet은 무선 카드가 없거나 교체되었을 때 맥 어드레스 기반의 소프트웨어 라이센스가 깨지지 않도록 해줍니다  
  이를 고려할 필요 없다면 제거해도 됩니다

***Fn 키***
- 'Fn' + 'r' || PrtScr = F13
- 'Fn' + 'F11' || 'Fn' + 's' = F14 (밝기 감소)
- 'Fn' + 'F12' || 'Fn' + 'b' = F15 (밝기 증가)
- 'Fn' + 'Esc', 'F1', 'F2', 'F3', 'F4', 'F5', 'F6', 'F7', 'F10', 'PrtScr', 'Arrows'
- '전원 버튼'을 짧게 누르고 때면 잠자기 상태로 전환되고, 길게 누르면 전원 메뉴가 표시됩니다

***OS X El Capitan 설치를 위한 준비***
- FakeCPUID를 Skylake H로 추가 : 0x0506E3
- SMBIOS 수정 : MacBookPro12,1
- NVME를 위해 켁스트 추가 : HackrNVMeFamily-10_11_6.kext


## 작동하는 것

***그래픽/디스플레이***
- Intel® UHD Graphics 620 QE/CI, 2048MB vRam
- Type C DP 2개 포트의 비디오/오디오 출력 핫 스왑
- 썬더볼트 디스플레이 출력
- 밝기 제어
- 사이드카

***오디오***
- 내장 스피커
- 내장 마이크
- 라인 입력
- DP 오디오 출력

***입력***
- I2C 터치 스크린의 5점 터치 제스처 동작 (매직 트랙패드 2로 인식)
- Dell Latitude 2-in-1 휴대용 키보드 백라이트 동작
- 휴대용 키보드의 터치패드 (마우스로 인식)
- 볼륨 버튼, 윈도우 버튼(옵션 키), power button(잠자기/전원 키)

***전원 관리***
- CPU/스피드 스텝
- 배터리
- Type C PD 2개 포트의 충전, PowerShare
- 잠자기/깨우기 : 썬더볼트 3 모델의 경우 [이슈] 항목의 'SSDT-DEEPIDLE.aml' 참고
- 휴대용 키보드의 덮개 닫아 잠자기 동작

***USB, 저장장치***
- 풀 사이즈/Type C USB 2.0, 3.0 핫 스왑
- m.2 NVME 2280/ m.2 SATA 2280 1개 슬롯과 m.2 NVME 2230(2242)/m.2 SATA 2230(2242) 1개 슬롯
- Thunderbolt 3

***무선 통신***
- 아이메세지/페이스타임/앱스토어
- 와이파이, 블루투스, 에어드랍, 사이드카, 연속성 기능


## 이슈

- 배터리 사용 중 잠자기 상태에서 해드폰 잭의 연결상태가 변경되면, 깨우기 이후에 잡음과 출력 문제가 발생합니다  
  이를 해결하기 위해선 다시 잠자기, 깨우기를 해야 합니다

- 썬더볼트 3 핫 스왑은 작동하지만 썬더볼트 3 기기가 연결 해제되었을 때 커널 패닉이 일어날 수 있습니다

- SSDT-DEEPIDLE.aml는 썬더볼트 3 모델에서 Type C 를 통한 외부 기기의 핫 스왑을 가능하게 해줍니다  
  하지만 잠자기 전력 효율이 나빠집니다  
  SSDT-DEEPIDLE.aml를 제거하면 Type C 핫 스왑이 중단되지만, 잠자기 전력 효율은 정상적으로 작동합니다

- 매직 트랙패드 2 터치 스크린은 클린 설치 혹은 OS 업데이트 이후 인식되지 않을 수 있습니다  
  이 경우 켁스트 캐시를 리빌드하면 터치 스크린은 동작합니다 [sudo kextcache -i /]  
  터치 스크린 동작 이후, 휴대용 키보드의 Alps Touchpad가 비활성화되는데, Karabiner를 이용하여 활성화 시킬 수 있습니다  
  'Karabiner-Elements Preferences - Devices - Basic configuration'에서 'Alps Touchpad (Alps)'를 체크하여 활성화합니다

- MicroSD 슬롯은 정상 동작하지 않습니다
  수정된 Sinetek-rtsx.kext를 이용하면 HFS 포맷된 SD 카드는 사용 할 수 있지만, 여러 문제가 있습니다

- I2C 전면/후면 카메라는 인식되지 않습니다 (AVStream2500, OV5670, OV8858)


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


README_EN
https://github.com/laelsirus/Dell-Latitude-5290-2-in-1-UHD620-iGPU-CLOVER/blob/master/README.md
