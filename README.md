# Hackintosh-Thinkpad-L14-Gen-1

Attempt at running macOS Ventura on a Thinkpad L14 Gen 1 (Intel). Specs are as follows:

| Opencore |  macOS |
| ---------| ------ |
| 0.9.0    | 13.3.1 |


| Category    | Component                                            | Note                                                         |
| ----------- | ---------------------------------------------------- | ------------------------------------------------------------ |
| Type        | 20U1, 20U2                                           |                                                              |
| CPU         | Intel Core i5-10210U                                 |                                                              |
| GPU         | Intel UHD 630                                        |                                                              |
| SSD         | Crucial MX500 500 GB (CT500MX500SSD1)                | Replaced def. 256GB SSD and Installed 128GB M.2 (For Windows)|
| Screen      | 14" FHD 1920x1080 (IPS)                              | My model has no touch or pen input                           |
| Memory      | 8GB / 2666MHz DDR4                                   |                                                              |
| Battery     | Integrated Li-Polymer 43Wh                           | Single battery                                               |
| Camera      | 720p                                                 |                                                              |
| Wifi & BT   | Intel AX200                                          |                                                              |
| Keyboard    | PS2 Keyboard                                         |                                                              |
| Input       | Synaptics TM3471-040 TrackPad                        | Works over SMBus                                             |
| Audio       | ALC257                                               |                                                              |
| SD Card     | O2 Micro (1217:8621)                                 | Not working                                                  |
| UEFI Chip   | Winbond 25Q256JVEQ + (EC?) Winbond 25Q80DVSIG        | Useful if someone wants to disable CFG-Lock                  |
| Fingerprint | Goodix Fingerprint Sensor                            | No driver exists for this model neither in macOS nor Linux   |

## Status

<summary><strong>What's working ✅</strong></summary>

- [x] Battery percentage
- [x] Bluetooth - Intel AX200 `Works flawlessly with IntelBluetooth and some USBPatches`.
- [x] Wifi - Intel AX200  `Using Airportitlwm, works perfectly (2.4GHz Wi-Fi not recommended)`
- [x] CPU power management
- [x] GPU UHD hardware acceleration / performance
- [x] HDMI `VIEW NOTES BELOW`
- [x] iMessage, FaceTime, App Store, iTunes Store. `Generate your own SMBIOS`
- [x] Keyboard `Media and Brightness keys working.`
- [x] Audio -`"alcid=100. You need to manually switch between headphones and internal speakers."`
- [x] Microphone
- [x] Sleep/Wake `Needs 20 seconds to sleep. Working OK as of now`
- [x] TrackPoint  `Works perfectly. Just like on Windows or Linux.`
- [x] USB Ports `USB map created.`
- [x] Webcam
- [x] TouchPad `Testing right now using VoodooRMI and VoodooSMBus. Gestures work OK, though tracking is not consistent `

<summary><strong>What's NOT working</strong></summary>

- Sidecar or Handoff
- Micro SD Card Reader
- Fingerprint Reader

<details>
<summary><strong>ACPI Files</strong></summary>
<br>

| Component                   |
| --------------------------- |
| SSDT-AWAC.aml               |
| SSDT-BATX.aml  (disabled)   |
| SSDT-ECRW.aml               |
| SSDT-GPI0.aml               |
| SSDT-GPRW.aml               |
| SSDT-HPET.aml               |
| SSDT-Keyboard.aml           |
| SSDT-PLUG.aml               |
| SSDT-PNLF-CFL.aml           |
| SSDT-RHUB.aml               |
| SSDT-THINK.aml              |
| SSDT-UIAC.aml               |
| SDDT-USBX.aml               |

</details>

<details>
<summary><strong>Kernel extensions</strong></summary>
<br>
  
| Kext                   | Version |
| :--------------------- | ------- |
| Airportitlwm           | 2.2.0   |
| AppleALC               | 1.8.0   |
| BrightnessKeys         | 1.0.3   |
| CPUFriend              | 1.2.6   |
| CPUFriendDataProvider  | N/A     |
| ECEnabler              | 1.0.3   |
| HibernationFixup       | 1.4.8   |
| IntelBTPatcher         | 2.2.0   |
| IntelBluetoothFirmware | 2.2.0   |
| BlueToolFixup          | 2.6.5   |
| IntelMausi             | 1.0.7   |
| Lilu                   | 1.6.4   |
| NVMeFix                | 1.1.0   |
| SMCBatteryManager      | 1.2.7   |
| SMCProcessor           | 1.2.7   |
| USBMap                 | N/A     |
| VirtualSMC             | 1.3.1   |
| VoodooPS2Controller    | 2.3.5   |
| VoodooSMBUS            | 3.0     |
| VoodooRMI              | 1.3.5   |
| WhateverGreen          | 1.6.4   |
| YogaSMC                | 1.5.3   |

</details>


<summary><strong>Notes</strong></summary>


- Sleep and Wake needs extended testing. From closing the lid to actual sleeping it takes around 20 seconds. I had no problems with waking from sleep or hibernation for now
- A Broadcom Wi-Fi card is recommended.
- Battery life isn't quite up to spec. I suspect it is because of the CFG Lock, which cannot be disabled without BIOS modding (requires hardware). Battery patching is handled dynamically by ECEnabler, haven't had problems for now.
- HDMI works but needs extensive patching: External displays stop working after sleep or reboot, replugging fixes the issue but is far from ideal. Still, it is enough to get by. I'm working on this but if you've got a fix please report an issue. (Not tested on the update)


- USB-C fully tested (Thunderbolt3, DP, SuperSpeed Combo)
- DSDT Patches may need cleanup.
- Touchpad gestures now work using VoodooRMI, though I experienced erratic tracking at various times, mostly after sleep. Nothing major: tracking can get jumpy and palm rejection kicks in sometimes. Tolerable, but far from ideal. I already opened an issue in its GitHub repo.

# Initial setup
You NEED to supply your own SMBIOS, Serial number, MLB and UUID using GenSMBIOS, configured for a MacbookPro16,3
