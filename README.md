# ADS-B Receiver Setup Instructions

Step-by-step instructions for setting up a "passive aircraft radar" that receives ADS-B signals from aircraft and displays their positions and other information on a map

## Supported Hardware and Software

This guide covers the following hardware-software combinations:

### Supported Computers

The following computers are supported by this guide:

- Raspberry Pi Zero 2 W

**Note:** Other Raspberry Pi computers should work just fine; however, the authors recommend a Raspberry Pi Zero 2 W because it is one of the more power-efficient models and it is sufficiently performant for everything we will be doing in this guide!

**Note 2:** If you are purchasing a Raspberry Pi Zero 2 W and plan to attach a GPS HAT now or in the future, you may want to purchase one with pre-soldered headers.
Alternatively, you may buy headers and solder it yourself!

### Supported Operating Systems

- Raspberry Pi OS Lite (32-bit), Debian version 12 (bookworm), deployed using Raspberry Pi Imager

**Note:** Other versions of Raspberry Pi OS such as the "with desktop" release should also work.
However, running the desktop environment is unnecessary and will use more resources.
You may also use another Debian-based release such as Ubuntu; however, doing so is not supported by this guide.

**Note 2:** Other deployment methods (outside of the Raspberry Pi Imager, e.g., Etcher) will work.
However, if you deploy using another method, you may need to connect a keyboard and monitor to the Raspberry Pi to complete its initial setup and get the device onto the network.
This is because Raspberry Pi transitioned away from its simple text-based configuration files and is in an interim state with sparse documentation where it is not as straightforward to automate the out-of-box configuration.
Raspberry Pi OS will support `cloud-init` in the future, which will streamline configuration.

### Supported "Technician's Computer" Operating Systems

The "technician's computer" is a computer different than the one that will be used to run emulation software.
It is used to bootstrap the overall setup process and it is where some configuration activities are performed.

- Windows 11

**Note:** Other technician's computer operating systems will certainly work; however, this guide is not written for them.

### Supported Locales

The authors are based in the United States of America, speak English (US) and use an English (US) keyboard layout, and are in the Central US time zone.
Accordingly, the products listed and the instructions in this repository will be US and English-centric.
However, it is possible to complete these steps using alternative languages, locales, etc.

## Bill of Materials

This is a work in progress!

- Raspberry Pi Zero 2 W with Pre-Soldered Headers: https://www.pishop.us/product/raspberry-pi-zero-2-w-with-pre-soldered-headers/
- Micro USB (male) to USB (female) adapter cable: https://amzn.to/3y5Z6LX
- Plus the items below:

### Simplest Setup - Stationary, Proof of Concept

- Software Defined Radio w/ included bandpass filter: https://flightaware.store/products/pro-stick-plus
  - **Note:** if you plan to "upgrade" to a more sophisticated setup in the future, you may want to purchase the simpler Software Defined Radio (without an included bandpass filter - see below).
  If you do, you should purchase a standalone bandpass filter.
  Separating the software defined radio and the bandpass filter should theoretically improve signal quality, although at an additional cost.
- 1090 MHz antenna: https://amzn.to/4bhtWjm
  - **Note:** if you plan to build a portable ADS-B receiver, e.g., for a vehicle, then you may want to purchase an alternative antenna (see the magnetic option below).

### Portable ADS-B Receiver - Vehicle or Camping Usage

Fill in details

### Roof-Mounted Setup

Fill in details

## Required Hardware Accessories/Tools

**This is a placeholder!**

In addition to the core bill of materials, you will need some additional hardware to support your RetroPie build.

- A **microSD card** is required even if you are planning a build that uses SSD or NVMe storage.
A microSD card is used to load Raspberry Pi OS Lite and update the firmware of your Raspberry Pi.
  - Pretty much any microSD card will do as long as the capacity is greater or equal to 8 GB.
  - If you need to purchase a smaller, lower-cost microSD card **for the sole purpose of updating the Raspberry Pi's firmware**, I recommend this 64 GB one from SanDisk: [SanDisk Ultra 64GB microSD UHS-I Card with A1 Performance Rating - Affiliate Link](https://amzn.to/47j14ET).
  - If you are building a RetroPie system and planning to use the microSD card for storage (as opposed to a SATA or NVMe SSD), you are going to want a larger card.
In this case, I recommend this SanDisk Ultra 1.5TB one: [SanDisk Ultra 1.5TB microSDXC UHS-I card with A1 Performance Rating - Affiliate Link](https://amzn.to/41JNZmH).
Or, for a slightly faster microSD card, get one with an A2 performance rating and slightly less capacity like this Lexar 1TB one: [Lexar PLAY 1TB microSDXC UHS-I card with A2 Performance Rating - Affiliate Link](https://amzn.to/3vqAqwo).
- Likewise, **a microSD card reader** is required. I recommend a USB 3.0 / USB 3.1 Gen 1 / USB 3.2 Gen 1x1 (or better) microSD reader like this inexpensive one from SanDisk: [SanDisk MobileMate USB 3.0 microSD Card Reader - Affiliate Link](https://amzn.to/48i3Ew3).
  - **Note**: if the technician's computer only has USB-C ports, you will need an adapter or a different microSD card reader.
Here are some options:
    - USB A to C adapter: [Amazon Basics USB A (female) to USB-C (male) Adapter - Affiliate Link](https://amzn.to/41JMg11)
    - USB C microSD card reader: [Anker SD and microSD Card Reader, USB-C Connector - Affiliate Link](https://amzn.to/41LLKj4)
    - USB A and C microSD card reader (supports both!): [Anker SD and microSD Card Reader, USB-A and USB-C Connectors - Affiliate Link](https://amzn.to/3H40SyD)
- A **USB keyboard and mouse** are required.
If you need to purchase one, it's nothing fancy, but I like this simple, compact, all-in-one unit from Logitech: [Logitech K400 Plus Wireless Keyboard with Touchpad Mouse](https://www.amazon.com/Logitech-Wireless-Keyboard-Touchpad-PC-connected/dp/B014EUQOGK)
- A **precision screwdriver** is required for assembly. If you can find it, I like this electrostatic discharge-resistant one from Wiha: [Wihi 27324 Phillips Screwdriver with Precision ESD-Safe Dissipative Handle - Affiliate Link](https://amzn.to/48xjNgS).
- For **Raspberry Pi 4B builds**, a **micro HDMI to full-size HDMI adapter** is needed.
This one from Monoprice works great: [Monoprice Micro HDMI (Male) to Full-Size HDMI (Female) Adapter Cable - Affiliate Link](https://amzn.to/4aDzpkR).
- For builds involving the **Argon ONE m.2 SATA Expansion Board**, a **USB-A to USB-A cable** is needed.
I have this one, and it's working great for me so far: [AINOPE USB 3.0 A (Male) to USB 3.0 A (Male) Cable, 6.6 Ft. - Affiliate Link](https://amzn.to/3S4CumV).
- For builds involving the **Argon ONE m.2 SATA Expansion Board**, a 5mm hex nut driver may be required.
If you can find it, I like this electrostatic discharge-resistant one from Wiha: [Wiha 27787 5.0mm Hex Nut Driver with Precision ESD-Safe Dissipative Handle - Affiliate Link](https://amzn.to/3S31877).
- For **all builds except handhelds**, a HDMI cable is required.
If you have one, use what you have.
If you need one, I recommend one of the following:
  - 3 Ft.: [Monoprice Certified Ultra High Speed HDMI 2.1 Cable, 3 Ft. - Affiliate Link](https://amzn.to/3H4vTmc)
  - 6 Ft.: [Monoprice Certified Ultra High Speed HDMI 2.1 Cable, 6 Ft. - Affiliate Link](https://amzn.to/41HNrxI)
  - 10 Ft.: [Monoprice Certified Ultra High Speed HDMI 2.1 Cable, 10 Ft. - Affiliate Link](https://amzn.to/4aJqSNt)
- For **all builds except handhelds**, you're also going to need **a TV or computer monitor** that supports an HDMI connection.
I hope that you already have one of these!
But, if you need a recommendation for a TV, you can't go wrong with an 83-inch LG C3 OLED: [LG C3 Series 83-Inch OLED TV - Affiliate Link](https://amzn.to/3tBKcLG) - **be sure to purchase one shipped and sold by Amazon**.
- If you are building a non-handheld build, and if your setup supports it, an Ethernet cable.
Use what you already have - but if you need to purchase one, I recommend one of the following:
  - 3 Ft: [C2G Legrand CAT6a Ethernet Cable, Snagless Unshielded, Black, 3 Ft. - Affiliate Link](https://amzn.to/3NM1OLD)
  - 6 Ft.: [C2G Legrand CAT6a Ethernet Cable, Snagless Unshielded, Black, 6 Ft. - Affiliate Link](https://amzn.to/3tBrGmO)
  - 10 Ft.: [C2G Legrand CAT6a Ethernet Cable, Snagless Unshielded, Black, 10 Ft. - Affiliate Link](https://amzn.to/3NHPtrN)
  - Other sizes available at the above links)