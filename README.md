# ThinkPad X230 MacOS with OpenCore

MacOS (Currently Big Sur) working on ThinkPad X230

**Status: Stable**

**DISCLAIMER:** Read the entire README before you start. I am not responsible for any damages you may cause.

## Introduction

<details>
<summary><strong>My hardware</strong></summary>

| Specifications      | Detail                                      |
| :------------------ | :------------------------------------------ |
| Computer model      | Lenovo ThinkPad X230 (Type: 2325)           |
| Processor           | Intel Core i5-3380M (2C4T, 2.9/3.6Ghz, 3MB) |
| Memory              | Crucial 8GB DDR3L 1600MHz, dual-channel     |
| Hard Disk           | SanDisk SSD Plus 120GB                      |
| Integrated Graphics | Intel HD Graphics 4000                      |
| Display             | 12.5" HD (1366x768) IPS                     |
| Audio               | Realtek ALC3202 (Layout-id: `18`)           |
| Ethernet            | Intel 82579LM Gigabit Network Connection    |
| WIFI+BT             | Dell DW1510 (BCM4322 in macOS)              |
| Keyboard            | 7-row classic, multimedia Fn keys,          |
| Dock                | ThinkPad Mini Dock Plus Series 3            |

</details>

<details>
<summary><strong>Hardware compatibility</strong></summary>

This EFI will suit any X230 regardless of CPU model, amount of RAM, display resolution, and internal storage.

  1. Optional custom CPU Power Management guide (see below post-install)
  1. Modified
      - 1440p display models should change `NVRAM>>Add>>7C436110-AB2A-4BBB-A880-FE41995C9F82>>UIScale`: 2

</details>

<details>
<summary><strong>Main software</strong></summary>

| Component      | Version |
| :------------- | :------ |
| MacOS Big Sur  | 11.2    |
| OpenCore       | 0.6.6   |

</details>

## Installation

<details>
<summary><strong>How to install macOS</strong></summary>

To install macOS follow the guides provided by [Dortania](https://dortania.github.io/getting-started/)

Useful tools by [CorpNewt](https://github.com/corpnewt) and [headkaze](https://github.com/headkaze/Hackintool)

Complete EFI is available in the [releases](https://github.com/banhbaoxamlan/X230-Hackintosh/releases/latest) page

</details>

<details>
<summary><strong>BIOS settings :100:</strong></summary>

A simple method to install a modified BIOS is available [here](https://github.com/n4ru/1vyrain/) (no external programmer required).

| Main | Sub #1                                 | Sub #2 | Sub #3 | Setting |
| :------------ | :----------- | ------------- | ------------- | ------------- |
| Config | Network | Wake On Lan |  | Disabled |
|  | Serial ATA (SATA) | Mode |  | AHCI |
| Security | Security Chip |  |  | Disabled |
|  | Memory Protection | Execution Prevention |  | Enabled |
|  | Anti-Theft | Current Setting |  | Disabled |
|  |  | Computrace | Current Setting | Disabled |
|  | Secure Boot |  |  | Disabled |
| Startup | UEFI/Legacy Boot |  |  | UEFI Only |
|  |  | CSM Support |  | Disabled |

</details>

## Post-install

<details>
<summary><strong>Generate your own SMBIOS</strong></summary>

For setting up the SMBIOS info, use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)

- Run GenSMBIOS, pick option 1 for downloading MacSerial and Option 3 for selecting out SMBIOS

  - MacBookPro9,2

- Open `Config.plist`, find PlatformInfo >> Generic

  - The `Serial` part gets copied to SystemSerialNumber.

  - The `Board Serial` part gets copied to MLB.

  - The `SmUUID` part gets copied to SystemUUID.

**Reminder that you want either an invalid serial or valid serial numbers but those not in use, you want to get a message back like: "Invalid Serial" or "Purchase Date not Validated"** [Apple Check Coverage](https://checkcoverage.apple.com/)

</details>

<details>
<summary><strong>CPU power management</strong></summary>

Recommended additional steps to improve battery life with optimized CPU power management:

- Open Config.plist, enable `ACPI>>Delete` : Drop CpuPm and Drop Cpu0Ist
- Open Terminal, copy and paste the following command:

  ```bash
  curl -o ~/ssdtPRGen.sh https://raw.githubusercontent.com/Piker-Alpha/ssdtPRGen.sh/master/ssdtPRGen.sh
  chmod +x ~/ssdtPRGen.sh
  ./ssdtPRGen.sh
  ```

- A customized `SSDT.aml` for your specific machine will now be in the directory **/Users/yourusername/Library/ssdtPRGen**

- Rename to `SSDT-PM.aml` , and copy to **EFI/OC/ACPI/**

- Open `Config.plist`, enable `ACPI>>Add>>SSDT-PM.aml`

- Reboot


</details>

<details>
<summary><strong>USB ports map</strong></summary>

If you are using different model and alternative kext from Other folder does not work for you. Try:

- [USBMap](https://github.com/corpnewt/USBMap)

- [Hackintool](https://github.com/headkaze/Hackintool)

</details>

<details>
<summary><strong>Fully functioning multimedia Fn keys</strong></summary>

- Download and install [ThinkpadAssistant](https://github.com/MSzturc/ThinkpadAssistant/releases)
- Open the app and check the `launch on login` option

</details>

<details>
<summary><strong>Use PrtSc key as Screenshot shortcut</strong></summary>

- Go under `SystemPreferences > Keyboard > Shortcuts > Screenshots`
- Click on `Screenshot and recording options` key map
- Press `PrtSc` on your keyboard (it should came out as `F13`)

</details>

<details>
<summary><strong>Use middle trackpoint button for scrolling</strong></summary>

- Download [Smart Scroll for macOS](https://www.marcmoini.com/sx_en.html)
- Install for all users
- Enable the `don't move cursor when scrolling` option

</details>

## Status

<details>
<summary><strong>What's working :white_check_mark:</strong></summary>

- [x] Battery Percentage
- [x] Bluetooth
- [x] Brightness
- [x] Camera
- [x] CPU Power Management
- [x] Dock Support `ThinkPad UltraSeries 3`
- [x] GPU Intel HD 4000 Graphics QE/CI
- [x] Intel Ethernet
- [x] Keyboard `Volume and brightness hotkeys`
- [x] Sleep/Wake
- [x] Sound `Automatic headphone detection, mute, volume controls fully working`
- [x] Touchpad `1-4 fingers swipe works`
- [x] TrackPoint  `Works perfectly. Just like on Windows or Linux`
- [x] eGPU  (Thanks [lese9855](https://github.com/lese9855) have confirmed it [#11](https://github.com/banhbaoxamlan/X230-Hackintosh/issues/11))
- [x] Brightness keys `In most configs your max brightness in macOS won't be the max brightness of the panel, this is fixed here`

</details>

<details>

<summary><strong>What's not working :warning:</strong></summary>

- [ ] Fingerprint Reader
- [ ] VGA
- [ ] SD Card Reader (Disable with `SSDT-SDC.aml`)

</details>

<details>

## Credits

[banhbaoxamlan](https://github.com/banhbaoxamlan) for the original x230 EFI

[SkyrilHD](https://github.com/SkyrilHD) for explaining OpenCore specifics to me

[Apple](https://www.apple.com) for macOS

[Acidanthera](https://github.com/acidanthera) for all the kexts/utilities that they made

[Rehabman](https://github.com/RehabMan) and [Daliansky](https://github.com/daliansky) for the patches and guides and kexts

[George Kushnir](https://github.com/n4ru) for modified BIOS

[Dortania](https://github.com/dortania) for for the OpenCore Install Guide

[MSzturc](https://github.com/MSzturc) for ThinkpadAssistant

[simprecicchiani](https://github.com/simprecicchiani) for inspirational ThinkPad configurations
