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

The authors are based in the United States of America, speak English (US), use an English (US) keyboard layout, and are in the Central US time zone.
Accordingly, the products listed and the instructions in this repository will be US and English-centric.
However, it is possible to complete these steps using alternative languages, locales, etc.

## Bill of Materials

### Base Materials

The following materials apply to all builds:

- A **Raspberry Pi Zero 2 W** with pre-soldered headers: [Raspberry Pi Zero 2 W with pre-soldered headers from PiShop.us](https://www.pishop.us/product/raspberry-pi-zero-2-w-with-pre-soldered-headers/)
  - **Note:** A GPIO header is useful if you are constructing a portable setup (e.g., for use in vehicles or while camping).
  You may also purchase a GPIO header and solder it yourself if needed.
  Most Raspberry Pi Zero 2 W retailers do not sell them with the header pre-soldered.
- An **"OTG" adapter cable** (micro USB (male) to USB (female)): [UGREEN Micro USB 2.0 OTG Cable On The Go Adapter Male Micro USB to Female USB - Affiliate Link](https://amzn.to/3y5Z6LX)
- A **microSD card** is used to store the software of the Raspberry Pi and boot the operating system. Example: [SanDisk Extreme 64GB microSDXC UHS-I Card with A2 Performance Rating - Affiliate Link](https://amzn.to/49TMk0w)
  - Pretty much any microSD card will do as long as the capacity is greater or equal to 8 GB.
  - If you purchase from Amazon, we highly recommend ensuring that the microSD card is **shipped and sold by Amazon**.
  At the time of writing, the one linked above is the least expensive reputable microSD card that is.
  So, even though 64GB is overkill, this would be our current recommendation!
  - Some models of Raspberry Pi also support USB storage or even NVMe storage.
  However, faster storage is completely unnecessary for this build---it costs more and uses more power, which could be problematic for some use cases.
- Optional, but recommended: a **case** for the Raspberry Pi Zero 2 W: [Pimoroni Pibow Zero 2 W case](https://www.pishop.us/product/pibow-zero-2-w/)

**You're not done!**
**More materials are required!**
Please add items from options 1, 2, 3, or 4 below.

### Option 1: Simplest Setup - Stationary, Only Outdoor As-Needed, Proof of Concept

- **Power supply** for micro USB Raspberry Pis: [CanaKit 5V 2.5A Raspberry Pi power supply - affiliate link](https://amzn.to/3UC7fkd)
- **Software-defined radio w/ included bandpass filter**: [FlightAware Pro Stick Plus](https://flightaware.store/products/pro-stick-plus)
  - **Note:** if you plan to "upgrade" to a more sophisticated setup in the future, you may want to purchase a simpler software-defined radio *without* an included bandpass filter.
  You would instead purchase a separate bandpass filter (see below).
  Separating the software-defined radio and the bandpass filter should theoretically improve signal quality/range, although it does so at an additional cost.
- **Antenna**: [26 in./66 cm, 1090 MHz and 978 MHz combined antenna, 5.5 dB gain - affiliate link](https://amzn.to/4bhtWjm)
  - **Note:** if you plan to build a portable ADS-B receiver, e.g., for a vehicle, then you may want to purchase an alternative antenna (see the magnetic option below).
- One N (male) to SMA (male) **cable**, like the following:
  - 1 ft. (30 cm) cable length: [1 ft. N (male) to SMA (male) RG8 cable - affiliate link](https://amzn.to/44maWxI)
  - 2 ft. (61 cm) cable length: [2 ft. N (male) to SMA (male) "LMR400-equivalent" cable - affiliate link](https://amzn.to/4dndM9W)
  - 2.87 ft. (85 cm) cable length: [85 cm N (male) to SMA (male) 5D-FB cable - affiliate link](https://amzn.to/3WmA9Gs)
  - 6 ft. (1.83 m) cable length: [6 ft. N (male) to SMA (male) KMR240 cable - affiliate link](https://amzn.to/4di4ZG9)
  - 15 ft. (4.57 m) cable length: [15 ft. N (male) to SMA (male) KMR400 cable - affiliate link](https://amzn.to/3Uq7b5M)
  - **Note:** choose the shortest cable length that works for your application!
  - **Note 2:** the cables listed are not necessarily suitable for in-wall or riser applications, and they should not be run through HVAC vents/plenums.
  - **Note 3:** the antenna listed above has an "N" type connector, and the software-defined radio has a "SMA" type connector.
  If you change the antenna or add other equipment between the antenna and the software-defined radio, you may need a cable with alternative connector types or genders!
  - **Note 4:** the authors believe the above cables are good quality choices for non-permanent installations.
  If you have specific needs for cable routing/flexibility, or if you are planning a very long cable run, we recommend doing some research on the different types of cable (e.g., KMR400 vs. KMR240) and using that information to find a suitable cable for your application.

### Option 2: Indoor ADS-B Receiver

- **Power supply** for micro USB Raspberry Pis: [CanaKit 5V 2.5A Raspberry Pi power supply - affiliate link](https://amzn.to/3UC7fkd)
- **Software-defined radio** without an included bandpass filter: [FlightAware Pro Stick](https://flightaware.store/products/pro-stick)
- **1090 MHz filtered pre-amp**: [Uputronics 1090 MHz ADS-B filtered preamp](https://store.uputronics.com/index.php?route=product/product&path=59&product_id=50)
  - **Note:** if you are mounting the equipment into a box, you may want to purchase the filtered preamp with the lug kit included.
  Lugs are easy to remove later if needed.
  - **Note 2:** the filtered pre-amp will be connected directly to the software-defined radio, so be sure to purchase it with an SMA coupler (male to male)
- If you don't already have one, a **USB-C power supply** for the filtered pre-amp: [Anker 511 (Nano Pro) USB-C Charger](https://amzn.to/44m7HGG)
  - **Note:** any USB charger will do!
  You can even use a charger with USB-A ports with a USB-A to C cable (not listed).
- A **USB-C charging cable** for the filtered pre-amp:
  - 3 ft. (91 cm): [Anker Powerline III USB-C to USB-C charging cable, 3 ft. - affiliate link](https://amzn.to/3xYWm3h)
  - 6 ft. (1.83 m): [Anker Powerline III USB-C to USB-C charging cable, 6 ft. - affiliate link](https://amzn.to/3WntEDb)
  - 10 ft. (3.05 m): [Anker Powerline III USB-C to USB-C charging cable, 10 ft. - affiliate link](https://amzn.to/3JGNUZ1)
  - **Note:** choose the shortest cable length that works for your application!
  - **Note 2:** any USB-C to USB-C cable will do; or, if you're using a USB-A charger, use a USB-A to USB-C cable
- A premium **antenna** suitable for indoor usage, like this small magnetic-base one from DPD Productions: [ADS-B double 1/2 wave mobile antenna](https://dpdproductions.com/collections/aviation-base-mobile-antennas/products/ads-b-double-1-2-wave-mobile-antenna)
  - **Note:** DPD Productions also sells an "indoor antenna" - however, the authors have had better indoor performance with the mobile antenna.
  Plus, the mobile antenna is less expensive!

### Option 3: Indoor or Portable ADS-B Receiver - Vehicle, Camping, or Other Travel Usage

- A portable **power bank** that allows simultaneous charging and discharging (i.e., you can charge it while also powering other devices).
This is a rare feature on power banks, so choose wisely!
For example, the Goal Zero Sherpa 100AC supports simultaneous charge/discharge, and it has many other use cases: [Goal Zero Sherpa 100AC - affiliate link](https://amzn.to/3QsSVb8)
  - **Note:** be mindful of the temperature rating of the power bank that you purchase.
  For the Goal Zero Sherpa 100AC, it is not advised to use it outside of its temperature range of 32-104 degrees F (0-40 degrees C).
  Operating a power bank outside of its temperature rating can have serious consequences such as an explosion, fire, or loss of property/life.
  Or, at best, it will reduce the efficacy of the charger.
- A **vehicle charger** that is compatible with the power bank that you choose.
For many power banks, this may be a standard vehicle USB-C charger.
However, for the Goal Zero Sherpa 100AC power bank mentioned above, there's a specific charger: the [Goal Zero Regulated Lithium Yeti Car Charger - affiliate link](https://amzn.to/3JGBOii)
- A USB-A to **micro-USB cable** to connect the power bank to the Raspberry Pi:
  - 6 in. (15 cm): [CableCreation micro USB cable 0.5ft/6 inch - affiliate link](https://amzn.to/3Ug9rfW)
  - 1 ft. (30 cm): [StarTech.com 1 ft USB to micro USB cable - affiliate link](https://amzn.to/44qx0Hn)
  - 3 ft. (91 cm): [Anker PowerLine 3 ft. (3 pack) - affiliate link](https://amzn.to/4b0Q9lK)
  - 6 ft. (1.83 m): [Anker PowerLine+ 6 ft. cable - affiliate link](https://amzn.to/3UpFtWJ)
  - **Note:** choose the shortest cable length that works for your application!
  - **Note 2:** any standard USB to micro USB cable will work - use what you have!
- **Software-defined radio** without an included bandpass filter: [FlightAware Pro Stick](https://flightaware.store/products/pro-stick)
- **1090 MHz filtered pre-amp**: [Uputronics 1090 MHz ADS-B filtered preamp](https://store.uputronics.com/index.php?route=product/product&path=59&product_id=50)
  - **Note:** if you are mounting the equipment into a box, you may want to purchase the filtered preamp with the lug kit included.
  Lugs are easy to remove later if needed.
  - **Note 2:** the filtered pre-amp will be connected directly to the software-defined radio, so be sure to purchase it with an SMA coupler (male to male)
- A **USB-C charging cable** to connect the power bank to the filtered pre-amp:
  - 6 in. (15 cm): [CableCreation USB-C to USB-C cable - affiliate link](https://amzn.to/4bjJMKd)
  - 1 ft. (30 cm): [Anker USB-C to USB-C charging cable, 1 ft. - affiliate link](https://amzn.to/4bhfqIe)
  - 3 ft. (91 cm): [Anker Powerline III USB-C to USB-C charging cable, 3 ft. - affiliate link](https://amzn.to/3xYWm3h)
  - 6 ft. (1.83 m): [Anker Powerline III USB-C to USB-C charging cable, 6 ft. - affiliate link](https://amzn.to/3WntEDb)
  - 10 ft. (3.05 m): [Anker Powerline III USB-C to USB-C charging cable, 10 ft. - affiliate link](https://amzn.to/3JGNUZ1)
  - **Note:** choose the shortest cable length that works for your application!
  - **Note 2:** any USB-C to USB-C cable will do; or, you may also use a USB-A to USB-C charging cable if you prefer
- A premium **antenna** with a magnetic mount, suitable for vehicle or outdoor usage, like this small magnetic-base one from DPD Productions: [ADS-B double 1/2 wave mobile antenna](https://dpdproductions.com/collections/aviation-base-mobile-antennas/products/ads-b-double-1-2-wave-mobile-antenna)
- A **GPS "HAT"** that fits the Raspberry Pi Zero 2 W: [Ozzmaker BerryGPS-IMU GPS and 10DOF for the Raspberry Pi - affiliate link](https://amzn.to/3JIZuCM)
  - **Note:** the Ozzmaker BerryGPS-IMU requires its GPIO header to be soldered to its board.
  While it's a relatively simple soldering operation, you should be comfortable with the basics of soldering before purchasing this GPS HAT.
- Assuming you are using the Pibow Raspberry Pi Zero 2 W case, you'll need **fasteners to secure the GPS HAT to the Pi/case** because the ones included with the Ozzmaker BerryGPS-IMU will not fit with the Pibow in the mix:
  - Qty 4: [M2.5 x 25mm pan head machine screws - nylon](https://www.accu.co.uk/phillips-pan-head-screws/497640-SIP-M2-5-25-V2-N), -**and**-
  - Qty 36: [2.7mm x 6.5mm x 0.5mm flat washers - nylon](https://www.accu.co.uk/metric-flat-washers/465680-HPW-2-7-6-5-0-5-N), -**and**-
  - Qty 4: [M2.5 hexagon nut - nylon](https://www.accu.co.uk/hexagon-nuts/422469-HPN-M2-5-N)
  - **Note:** it would be wise to get several extra washers, as these are very small and easy to lose!
  Additionally, an extra hexagon nut or two can't hurt.
- Optional, but recommended: an external, magnetic **GPS antenna**: [toyshi waterproof GPS antenna with magnetic base and SMA to uFL converter cable - referral link](https://amzn.to/44lMlct)
- A note for **permanent vehicle installations**: it may be a good idea to bond the antenna system to a suitable ground point on the vehicle's chassis.
For more information on this concept, check out: [Grounding and Bonding for the Radio Amateur - affiliate link](https://amzn.to/3JJkWI0).

### Option 4: Permanent Outdoor Setup

- **Grounding** connection: while the specifics of this are out of scope for this guide, you must ensure that the system is bonded to a suitable ground for lightning protection.
The National Electrical Code standard 810 covers commonly accepted requirements for the grounding and bonding of antenna systems.
For more information on this concept, check out: [Grounding and Bonding for the Radio Amateur - affiliate link](https://amzn.to/3JJkWI0).
This guide does not specify equipment for bonding or surge-arresting.
- **Power supply** for micro USB Raspberry Pis: [CanaKit 5V 2.5A Raspberry Pi power supply - affiliate link](https://amzn.to/3UC7fkd)
  - **Note:** it may be desirable to consider a solar system to power the Raspberry Pi and antenna system.
  While the antenna would still need to be bonded to a suitable ground, electrically isolating the antenna system with an "off-grid" solar array and using a Wi-Fi connection for the Raspberry Pi provides a literal "air gap" between the antenna system and your electrical system and network, which gives you additional protection from a lightning strike.
  The design specifics of an off-grid solar system are outside the scope of this document, but the project [OffGridSolarPS](https://github.com/franklesniak/OffGridSolarPS) might help you plan the requirements for such a system.
  Based on the m
- **Software-defined radio** without an included bandpass filter: [FlightAware Pro Stick](https://flightaware.store/products/pro-stick)
- **Band-pass signal filter**: [FlightAware 1090 MHz band-pass filter](https://flightaware.store/products/band-pass-signal-filter-1090-only-mhz)
- SMA male to SMA male LMR-240 jumper cable [6 in./15 cm SMA male to SMA male LMR-240, "ultra-flexible" cable](https://www.fairviewmicrowave.com/sma-male-sma-male-cable-lmr-240-uf-coax-fmc0202245-06-p.aspx)
- **1090 MHz filtered pre-amp**: [Uputronics 1090 MHz ADS-B filtered preamp](https://store.uputronics.com/index.php?route=product/product&path=59&product_id=50)
  - **Note:** if you are mounting the equipment into a box, you may want to purchase the filtered preamp with the lug kit included.
  Lugs are easy to remove later if needed.
  - **Note 2:** the filtered pre-amp will be connected directly to the software-defined radio, so be sure to purchase it with an SMA coupler (male to male)
- If you don't already have one, a **USB-C power supply** for the filtered pre-amp: [Anker 511 (Nano Pro) USB-C Charger](https://amzn.to/44m7HGG)
  - **Note:** any USB charger will do!
  You can even use a charger with USB-A ports with a USB-A to C cable (not listed).
- A **USB-C charging cable** for the filtered pre-amp:
  - 3 ft. (91 cm): [Anker Powerline III USB-C to USB-C charging cable, 3 ft. - affiliate link](https://amzn.to/3xYWm3h)
  - 6 ft. (1.83 m): [Anker Powerline III USB-C to USB-C charging cable, 6 ft. - affiliate link](https://amzn.to/3WntEDb)
  - 10 ft. (3.05 m): [Anker Powerline III USB-C to USB-C charging cable, 10 ft. - affiliate link](https://amzn.to/3JGNUZ1)
  - **Note:** choose the shortest cable length that works for your application!
  - **Note 2:** any USB-C to USB-C cable will do; or, if you're using a USB-A charger, use a USB-A to USB-C cable
- A premium **antenna** suitable for indoor usage, like this small magnetic-base one from DPD Productions: [ADS-B double 1/2 wave mobile antenna](https://dpdproductions.com/collections/aviation-base-mobile-antennas/products/ads-b-double-1-2-wave-mobile-antenna)
  - **Note:** DPD Productions also sells an "indoor antenna" - however, the authors have had better indoor performance with the mobile antenna.
  Plus, the mobile antenna is less expensive!




## Required Hardware Accessories/Tools

**This is a placeholder!**

In addition to the core bill of materials, you will need some additional hardware to support your RetroPie build.

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