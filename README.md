# Hackintosh-Thinkpad-L14-Gen-1

Attempt at running macOS Big Sur on a Thinkpad L14 Gen 1 (Intel). Specs are as follows:

| Opencore |  macOS |
| ---------| ------ |
| 0.7.2    | 11.2.2 |


| Category    | Component                                            | Note                                                         |
| ----------- | ---------------------------------------------------- | ------------------------------------------------------------ |
| Type        | 20U1, 20U2                                           |                                                              |
| CPU         | Intel Core i5-10210U                                 |                                                              |
| GPU         | Intel UHD 630                                        |                                                              |
| SSD         | Kingston A2000 512GB                                 | Replaced default 256GB SSD with a larger one                 |
| Screen      | 14" FHD 1920x1080                                    | My model has no touch or pen input                           |
| Memory      | 16GB / 2666MHz DDR4                                  |                                                              |
| Battery     | Integrated Li-Polymer 43Wh                           | Single battery                                               |
| Camera      | 720p                                                 |                                                              |
| Wifi & BT   | Intel AX200                                          |                                                              |
| Input       | PS2 Keyboard & SMBUS Synaptics TrackPad              | Not all media keys working                                   |
| Audio       | ALC257                                               |                                                              |
| SD Card     | O2 Micro (1217:8621)                                 |                                                              |
| UEFI Chip   | Winbond 25Q256JVEQ + (EC?) Winbond 25Q80DVSIG        | Useful if someone wants to disable CFG-Lock                  |
| Fingerprint | Goodix Fingerprint Sensor                            | No driver exists for this model neither in macOS nor Linux   |

## Status

<summary><strong>What's working ✅</strong></summary>

- [x] Battery percentage
- [x] Bluetooth - Intel AX200 `Works flawlessly with IntelBluetooth`.
- [x] Wifi - Intel AX200  `Using itlwm + Heliport. Not as native as Airportitlwm, but much more stable.`
- [x] CPU power management
- [x] GPU UHD hardware acceleration / performance
- [x] HDMI `VIEW NOTES BELOW`
- [x] iMessage, FaceTime, App Store, iTunes Store. `Generate your own SMBIOS`
- [x] Keyboard `Not all media keys work. Volume buttons do.`
- [x]  Audio -`"alcid=86"`
- [x] Microphone
- [x] Sleep/Wake `Sleep light gets stuck at breathing mode after waking up. Need SSDT Fix for this.`
- [x] TrackPoint  `Works perfectly. Just like on Windows or Linux.`
- [x] USB Ports `USB map created.`
- [x] Webcam
- [x] TouchPad `Works best right now with VoodooPS2. VoodooRMI and VoodooI2C introduce quirks like no keyboard/touchpad after sleep `

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
| SSDT-BAT.aml                |
| SSDT-EC-USBX-LAPTOP.aml     |
| SSDT-GPRW.aml               |
| SSDT-HPET.aml               |
| SSDT-Keyboard.aml           |
| SSDT-PLUG-DRTNIA.aml        |
| SSDT-PNLF-CFL.aml           |
| SSDT-RHUB.aml               |
| SSDT-Thinkpad_Clickpad.aml  |

</details>

<details>
<summary><strong>Kernel extensions</strong></summary>
<br>
  
| Kext                   | Version |
| :--------------------- | ------- |
| AppleALC               | 1.6.3   |
| BrightnessKeys         | 1.0.2   |
| CPUFriend              | 1.2.4   |
| ECEnabler              | 1.0.2   |
| IntelBluetoothFirmware | 1.1.2   |
| IntelBluetoothInjector | 1.1.2   |
| IntelMausi             | 1.0.7   |
| itlwm                  | 1.3.0   |
| Lilu                   | 1.5.5   |
| NVMeFix                | 1.0.9   |
| SMCBatteryManager      | 1.2.6   |
| SMCProcessor           | 1.2.6   |
| SMCSuperIO             | 1.2.6   |
| USBMap                 | N/A     |
| VirtualSMC             | 1.2.6   |
| VoodooPS2Controller    | 2.2.3   |
| VoodooSMBUS            | 3.0     |
| WhateverGreen          | 1.5.2   |

</details>


<summary><strong>Notes</strong></summary>

- The SSDT patch related to the keyboard comes from an E14, so it needs to be replaced with a custom patch to make special keys work.
- Sleep and Wake needs extended testing. From closing the lid to actual sleeping it takes around 20 seconds. I had no problems with waking from sleep or hibernation for now
- A Broadcom Wi-Fi card is recommended.
- Battery life isn't quite up to spec. I suspect it is because of the CFG Lock, which cannot be disabled without BIOS modding (requires hardware). Also, the DSDT patch for the battery comes from a Lenovo L13.
- Implementing the YogaSMC kext would alleviate many of these problems, but as of today, it KPs.
- HDMI works but needs extensive patching: External displays stop working after sleep or reboot, replugging fixes the issue but is far from ideal. Still, it is enough to get by. I'm working on this but if you've got a fix please report an issue.

*EDIT*: I've tested and the culprit for power usage is a combination of itlwm and SSDT-PLUG, though I think the problem might be somewhere else. itlwm does not let the CPU idle at less than 1.5W. Disabling itlwm and SSDT-PLUG gets me power readings as low as 0.7W, which surely increases battery life, but disableng SSDT-PLUG makes fans not ramp up on full load, which toasts the CPU. Solution? Get a supported Wi-Fi Card. 

- I haven't tested USB-C adapters or displays yet.
- DSDT Patches need cleanup.
- Touchpad gestures need to be slow to be detected. This is because of using VoodooPS2 instead of other solution like VoodooI2C and VoodooRMI. The latter two introduce problems with sleep/wake, so I am using VoodooPS2 as a Fallback for now.

# Initial setup
I used AniKulkarn's repo (https://github.com/AniKulkarn/Hackintosh-ThinkPad-E14) as its specs are quite similar, then appended my changes to kexts and DSDT Patches.
