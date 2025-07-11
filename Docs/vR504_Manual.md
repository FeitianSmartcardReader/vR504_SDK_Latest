# VHBR Dual Interface Reader – vR504 Manual
## 0.1 Product Introduction

This product is mainly used in e-government, bank counters, border security, and integration by passport reader manufacturers. Its main working scenario is to read high-speed eID cards or passports. As international passport standards become more sophisticated and more secure, the requirements for reader devices are also increasing. Based on the release of the latest VHBR (Very High Bit Rate) specification and the support for high-speed frequency bands in ISO14443, we have launched the vR504 product to meet market demands. The difference between this product and the traditional R502 standard card reader is the addition of the VHBR protocol, with the communication rate increased to 3.2M/6.8M, enabling faster reading of large data. The product mainly includes the following modules:

The main functional modules of the product are as follows:

1) Contactless card reader module: Completes the basic functions of ISO 14443 Type A and Type B cards, supports extended APDU, with a rate ranging from 106kbps to 6.8M, and supports automatic frequency conversion, that is, the card reader can negotiate according to the maximum rate supported by the card, and communicates at the highest rate supported by the card by default.

2) USB module – Connects to a PC, enumerates devices, and establishes communication.

3) Buzzer module: Provides user-interactive reminders.

4) Supports firmware upgrade.

5) Supports the general functions of Feitian standard card readers, UID function, and built-in globally unique serial number.

## 0.2 Operating Environment

Hardware online environment:

- Supports USB connection to the host. The host needs to support Type A port. If the host has a Type-C female port, an adapter is required to use the card reader.

Software environment:

- The product supports Windows, Linux, and macOS, and appears as a CCID-driven PC/SC device on these platforms.

- The product software supports Windows 7, 8, 8.1, 10, 2003 server, 2008 server, and server 2012 (all versions), and also supports Android with an OTG cable.

- The Linux platform needs to support 64-bit Linux, Red Hat Enterprise Linux 7, CentOS 7, Debian 8, and the latest versions.

- The macOS platform supports 10.12 and the latest versions.

## 0.3 Features

Full-speed USB

Compliant with CCID specifications, driver-free, plug-and-play.

Built-in antenna module.

Compliant with ISO 14443 Type A and Type B specifications.

Data rate:

Type A card: PCD to PICC up to 106-848kbps, PICC to PCD up to 106-848kbps.

Type B card: PCD to PICC up to 106kbps-6.8M (fc/2), PICC to PCD up to 106kbps-3.4M (fc/4).

Card communication distance: 0-3 cm.

Clock frequency: 13.56MHz.

Supports automatic PPS, can automatically convert frequency according to the card rate, and modulate the corresponding rate.

Supported communication rates: 106kbps, 212kbps, 424kbps, 848kbps, 3.4M, and 6.8M.

Supports encrypted firmware upgrade.

Supported operating systems:

Windows 2000/XP/2003/Vista/2008/7/8.

Linux Kernel 2.6+.

macOS.

## 0.4 Specifications

|Driver Specification|CCID 1.1|
|---|---|
|Communication Interface|USB Type A – high speed|
|Power Supply|USB port on PC|
|Operating Current|<100mA (no card inserted)|
|Operating Temperature|0~+60°C|
|Storage Temperature|-20°C~85°C|
|Humidity|≤90% (No condensation)|
|Card Slot|100,000 plug times|
|Contactless Communication Distance|VHBR Card, Type A/B card (0-30mm)|
|Physical Security|Short-circuit protection|
|Contactless|Build-in antenna|
| |Support ISO 14443 standard (Type A and B)|
| |Support Mifare contactless card|
| |Type A:<br/>PCD to PICC up to 106-848kbps,<br/>PICC to PCD up to 106-848kbps<br/>Type B:<br/>PCD to PICC up to 106kbps-6.8M (fc/2)<br/>PICC to PCD up to 106kbps-3.4M (fc/4)|
|Certificates|CE<br/>FCC<br/>RoHS<br/>EMV 2000 level 1|
|Shell Model|C9|
|Dimensions|120*80*25.6mm|
|Weight|136g|
|Standards|ISO/IEC 14443 TYPE A/B, CCID, USB, VHBR, CE, FCC, RoHS|
|Operating Systems|Windows 2000/XP/2003/Vista/2008/7/8<br/>Linux<br/>macOS|

## 0.5 Development Guide

The vR504 card reader is based on the CCID protocol. No driver installation is required on the Windows side. On Linux and macOS, the CCID driver (open-source driver) needs to be installed. Product development is based on Winscard/PC/SC Lite API. Developers can call these interfaces to operate the card reader. For detailed sample codes, please refer to the demo folder in the SDK, which provides development examples in different languages.

No additional settings are required for VHBR cards. The card reader will automatically perform PPS according to the card rate, and determine the maximum communication rate of the card reader through the returned S block.

| |Receive|Transmit|Maximum Frame Length|
|---|---|---|---|
|Type A card communication|106-848kbps|106-848kbps|4096|
|Type B card communication|106-3.4MHz|106-6.8MHz|4096|

Note: The receive and transmit rates of the card reader must be the same. For example, if the card receives at 848kbps and transmits at 1.7M, the card reader will communicate at 848kbps by default.

Private command description:

Set delay time: The card reader powers off and turns off the field strength within the specified time.

|CLA (1byte)|INS (1byte)|P1 (1byte)|P2 (1byte)|LEN (1byte)|DATA (3byte)|
|---|---|---|---|---|---|
|FF|AA|71|30|03|Time (ms)|

DATA is 3 bytes, with the high byte first and the low byte last. Example: FF AA 71 30 03 00 00 80 ------> Delay 128ms.

Response:

|Status|SW1|SW2|
|---|---|---|
|Success|90|00|

Start delay time: After the delay is enabled, the card reader will turn off the field and power off. After power on, the card reader will automatically turn on the field strength.

|CLA (1byte)|INS (1byte)|P1 (1byte)|P2 (1byte)|
|---|---|---|---|
|FF|AA|71|31|

Response:

|Status|SW1|SW2|
|---|---|---|
|Success|90|00|
|Error|67|00|

## 0.6 Other Tools

Testing tool: Refer to R502 Demo in the Tool directory of the SDK.

UID function software: This tool is designed to control the distribution of card readers, and the UID will also serve as a key for card reader firmware encryption to protect the firmware.

Firmware upgrade: Refer to the Update file in the SDK, which is mainly used for firmware upgrade. Currently, it only supports upgrade on the Windows side.
