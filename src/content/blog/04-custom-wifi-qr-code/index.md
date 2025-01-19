---
title: "Create a Custom Wifi QR-Code"
description: "Create your own sticker with a wifi QR-code."
date: "2025-01-19"
draft: false
---

To make it easier for friends to access your home wifi network it can
be beneficial to have a QR Code on hand. Many routers will generate one for you. But maybe you don't want to be locked into the way your router creates the QR-code and create your own. In this case we're going to have a look at the definition of how a wifi SSID and password are encoded in a QR-code.

Interestingly, no official standard exists for the encoding of such data. But the documentation of ZXing (a library for 2D codes) states [here](https://github.com/zxing/zxing/wiki/Barcode-Contents#wi-fi-network-config-android-ios-11) the convention most often used and compatible with Android and iOS. Your own wifi QR-code needs to encode this string:

```bash
WIFI:T:<auth_type>;S:<network_ssid>;P:<password>;;
```

Here `<auth_type>` is most often `WPA`, and `<network_ssid>` and `<password>` are your network name and password respectively.
There are some more options regarding hidden SSIDs and some WPA2 settings. But those are not needed for most applications.

Now you can for example create a sticker with a label printer like this:

<img src="/blog-4/nelko-qr-code.png" alt="Nelko label printer QR-code" width="300"/>

In this case I am using a Nelko label printer which works quite well.
