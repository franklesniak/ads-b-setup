# ADS-B Receiver Setup Instructions

Step-by-step instructions for setting up a "passive aircraft radar" that receives ADS-B signals from aircraft and displays their positions and other information on a map

## Supported Hardware and Software

This guide covers the following hardware-software combinations:

### Supported ADS-B/Aircraft Tracking Standards

This guide focuses on Mode S-ES-based ADS-B transmissions, which are transmitted at 1090 MHz.
Accordingly, the bill of materials and instructions assume the reader is building an ADS-B receiver that is solely focused on receipt of that frequency.
If you are building a US-based receiver that should receive 978 MHz UAT, then you will need to deviate from the bill of materials and instructions below accordingly.

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
- Optional, but recommended: a **case for the Raspberry Pi Zero 2 W**: [Pimoroni Pibow Zero 2 W case](https://www.pishop.us/product/pibow-zero-2-w/)

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

### Option 3: Portable ADS-B Receiver - Vehicle, Camping, or Other Travel Usage

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
As you read the rest of the bill of materials below, keep in mind that this markdown page does not specify equipment for bonding or surge-arresting.
- **Power supply** for micro USB Raspberry Pis: [CanaKit 5V 2.5A Raspberry Pi power supply - affiliate link](https://amzn.to/3UC7fkd)
  - **Note:** it may be desirable to consider a solar system to power the Raspberry Pi and antenna system.
  While the antenna would still need to be bonded to a suitable ground, electrically isolating the antenna system with an "off-grid" solar array and using a Wi-Fi connection for the Raspberry Pi provides a literal "air gap" between the antenna system and your electrical system and network, which gives you additional protection from a lightning strike.
  The design specifics of an off-grid solar system are outside the scope of this document, but the project [OffGridSolarPS](https://github.com/franklesniak/OffGridSolarPS) might help you plan the requirements for such a system.
  Based on the authors' direct observations, the specified Raspberry Pi setup with a single software-defined radio (i.e., only receiving 1090 MHz) uses about 2.4 watts of power at steady state.
- **Software-defined radio** without an included bandpass filter: [FlightAware Pro Stick](https://flightaware.store/products/pro-stick)
- **Band-pass signal filter**: [FlightAware 1090 MHz band-pass filter](https://flightaware.store/products/band-pass-signal-filter-1090-only-mhz)
- **SMA male to SMA male coupler**, to connect the band-pass signal filter to the bias tee [SMA male to SMA male adapter, 2 pack - affiliate link](https://amzn.to/4dtF9is)
  - **Note:** if you would rather have a more flexible setup (compared to the linear, rigid coupling of the above modules), you may substitute the following cable: **SMA male to SMA male LMR-240 jumper cable** [6 in./15 cm SMA male to SMA male LMR-240, "ultra-flexible" cable](https://www.fairviewmicrowave.com/sma-male-sma-male-cable-lmr-240-uf-coax-fmc0202245-06-p.aspx)
- **Bias tee** to supply power to the filtered pre-amp: [USB bias tee](https://gpio.com/collections/bias-tees/products/usb-bias-tee-operates-from-10mhz-7000mhz)
  - **Note:** The bias tee supplies power to the filtered pre-amp because the filtered pre-amp should be placed as close to the antenna as possible, whereas it is assumed that the Raspberry Pi device is further away.
  For example, the Raspberry Pi may be near the ground, and the antenna may be mounted on a rooftop antenna mast.
  In this example, it may be desirable to address power needs at the ground level rather than running power to the roof line - this is what the bias tee allows us to do.
- **microSD power supply** the bias tee: [CanaKit 5V 2.5A Raspberry Pi power supply - affiliate link](https://amzn.to/3UC7fkd)
  **Note:** you may use another micro USB power supply if you have one!
- **Weather/dust-proof enclosure** to encase the above equipment (not specified)
- **SMA male to SMA male** LMR-400 cable, to run from the enclosure up to the antenna area:
  - 4 ft. (1.22 m): [Low Loss SMA Male to SMA Male Cable LMR-400 Coax in 48 Inch with Times Microwave Connectors with LF Solder](https://www.fairviewmicrowave.com/standard-sma-male-sma-male-cable-lmr400-coax-fmc0202400lf-48-p.aspx)
  - 5 ft (1.52 m): [Low Loss SMA Male to SMA Male Cable LMR-400 Coax in 60 Inch with Times Microwave Connectors](https://www.fairviewmicrowave.com/sma-male-sma-male-cable-times-lmr-400-coax-fmc0202400-60-p.aspx)
  - 6 ft (1.83 m): [Low Loss SMA Male to SMA Male Cable LMR-400 Coax in 72 Inch with Times Microwave Connectors](https://www.fairviewmicrowave.com/sma-male-sma-male-cable-times-lmr-400-coax-fmc0202400-72-p.aspx)
  - 10 ft (3.05 m): [Low Loss SMA Male to SMA Male Cable LMR-400 Coax in 120 Inch with Times Microwave Connectors](https://www.fairviewmicrowave.com/sma-male-sma-male-cable-times-lmr-400-coax-fmc0202400-120-p.aspx)
  - **Note**: If you need more than 10 ft. (3.05 m) of cable, you may need to contact Fairview Microwave for a custom order.
  - **Note 2:** the first link above (4 ft./1.22 m) is a RoHS-compliant product; if you want/desire a RoHS-compliant product but need a longer length than 4 ft./1.22 m, you may want to contact Fairview Microwave to place a custom order.
- **1090 MHz filtered pre-amp**: [Uputronics 1090 MHz ADS-B filtered preamp](https://store.uputronics.com/index.php?route=product/product&path=59&product_id=50)
  - **Note:** Purchase the filtered preamp with the lug kit included.
  Lugs may make the device easier to mount in an enclosure and are easy enough to remove later if needed.
- Another **weather/dust-proof enclosure** to encase the pre-amp and the antenna connections on it (not specified)
- A **SMA male to N male** jumper cable, to connect the filtered pre-amp to the antenna: [Low Loss N Male to SMA Male Cable LMR-400 Coax in 12 Inch with Times Microwave Connectors with LF Solder](https://www.fairviewmicrowave.com/standard-n-male-sma-male-cable-lmr400-coax-fmc0102400lf-12-p.aspx)
- A premium **antenna** suitable for outdoor usage, like this large vertically-mounted one from DPD Productions: [ADS-B Vertical Outdoor Base Antenna (Standard 1090 MHz ES)](https://dpdproductions.com/collections/aviation-base-mobile-antennas/products/ads-b-vertical-outdoor-base-antenna)

## Required Hardware Accessories/Tools

In addition to the above bill of materials, you will need some additional hardware to support the build of your ADS-B receiver.

- A **microSD card reader** is required. We recommend a USB 3.0 / USB 3.1 Gen 1 / USB 3.2 Gen 1x1 (or better) microSD reader like this inexpensive one from SanDisk: [SanDisk MobileMate USB 3.0 microSD Card Reader - affiliate link](https://amzn.to/48i3Ew3).
  - **Note**: if the technician's computer only has USB-C ports, you will need an adapter or a different microSD card reader.
Here are some options:
    - USB A to C adapter: [Amazon Basics USB A (female) to USB-C (male) Adapter - affiliate link](https://amzn.to/41JMg11)
    - USB C microSD card reader: [Anker SD and microSD Card Reader, USB-C Connector - affiliate link](https://amzn.to/41LLKj4)
    - USB A and C microSD card reader (supports both!): [Anker SD and microSD Card Reader, USB-A and USB-C Connectors - affiliate link](https://amzn.to/3H40SyD)
- A **USB keyboard** may be needed if you can't determine your Raspberry Pi's IP address or cannot connect to it.
If you need to purchase one, it's nothing fancy, but we like this simple, compact, all-in-one unit from Logitech: [Logitech K400 Plus Wireless Keyboard with Touchpad Mouse](https://www.amazon.com/Logitech-Wireless-Keyboard-Touchpad-PC-connected/dp/B014EUQOGK)
- You may want a **precision, slotted screwdriver** for the assembly of the Pimoroni Pibow case.
We like this electrostatic discharge-resistant one from Wiha: [Wiha 27224 Slotted Screwdriver with Precision ESD Safe Dissipative Handle, 2.0 x 150mm - affiliate link](https://amzn.to/44r49mk).
However, you may be able to use your finger nail to hold the nylon bolt and hand-tighten its nut.
- Additionally, if you are building a portable ADS-B receiver (e.g., for in-vehicle, camping, or other travel usage), you will need a **precision Phillips screwdriver** to attach the GPS HAT to the Raspberry Pi/Pibow case using the nylon nuts and bolts.
If you can find it, we like this electrostatic discharge-resistant one from Wiha: [Wiha 27324 Phillips Screwdriver with Precision ESD Safe Dissipative Handle, 1 x 100mm](https://amzn.to/3UJPzDn)
- You may need a **mini HDMI (male) to full-size HDMI (female) adapter** if you have a problem connecting to the Raspberry Pi over the network: [Cable Matters 2-Pack Mini HDMI to HDMI Adapter (HDMI to Mini HDMI Adapter) 6 Inches with 4K and HDR Support for Raspberry Pi Zero and More (2 pack) - affiliate link](https://amzn.to/3wlDlr5)
- If you have a problem and need to connect the Raspberry Pi to a monitor or TV, an **HDMI cable** is required.
If you have one, use what you have.
If you need one, we recommend one of the following:
  - 3 Ft.: [Monoprice Certified Ultra High Speed HDMI 2.1 Cable, 3 Ft. - affiliate link](https://amzn.to/3H4vTmc)
  - 6 Ft.: [Monoprice Certified Ultra High Speed HDMI 2.1 Cable, 6 Ft. - affiliate link](https://amzn.to/41HNrxI)
  - 10 Ft.: [Monoprice Certified Ultra High Speed HDMI 2.1 Cable, 10 Ft. - affiliate link](https://amzn.to/4aJqSNt)
- If you have a problem connecting to the Raspberry Pi over the network, you're going to need **a TV or computer monitor** that supports an HDMI connection.
I hope that you already have one of these!
But, if you need a recommendation for a TV, you can't go wrong with an 83-inch LG C4 OLED: [LG C4 Series 83-Inch OLED TV - affiliate link](https://amzn.to/4bGStyz) - **be sure to purchase one shipped and sold by Amazon**.

## Prepare the "Technician's Computer"

Before beginning the ADS-B receiver setup process, you will need to download and install some software.

### Download and Install Required Software

1. Download and install **Raspberry Pi Imager**.
This software is used to write the Raspberry Pi OS image to a microSD card.

    Link: [Raspberry Pi Imager](https://www.raspberrypi.com/software/)

1. Confirm your system has **OpenSSH** installed.
If it doesn't, install it. This software is used to remote into the Raspberry Pi system once it is online.
It allows us to copy-paste commands to expedite the setup process and make it less error-prone.
    - On the newest versions of Windows, OpenSSH is installed by default.
    - To confirm it's installed, open `Settings` > `System` > `Optional features`
        - If you can't find `Optional Features` under settings: try opening `Settings` > `Apps` > `Optional Features`.
        - Once you are in `Optional Features`, review the list of installed optional features and see if `OpenSSH Client` is installed.
    - If it isn't installed:
        - At the top, next to `Add an optional feature`, click `View features`.
        - Check the checkbox for `OpenSSH Client`. Click `Next`.
        - Continue with the installation.

    **Note**: you can use a different SSH client if you prefer

## Prepare the Raspberry Pi Before Loading FlightAware's Software

### Solder a GPIO header onto the Raspberry Pi

If you are building a portable ADS-B receiver (e.g., for in-vehicle, camping, or other travel usage), and if your Raspberry Pi did not come with a GPIO header installed, please solder a GPIO header on the Raspberry Pi.

The GPIO is a 20 x 2 pin array along the long side of the Raspberry Pi.

**Note:** you can skip this step if you are not building a portable ADS-B receiver.

### Prepare the microSD Card with a Raspberry Pi OS Lite Image

1. On the technician's computer, start Raspberry Pi Imager and prepare to write the image to the microSD card:
    - Insert your microSD card into the technician's computer's microSD card reader and open Raspberry Pi Imager.
    - In Raspberry Pi Imager, select `Choose Device`, then choose `Raspberry Pi Zero 2 W`.
    - Select `Choose OS`, select `Raspberry Pi OS (other)`, then choose `Raspberry Pi OS Lite (32-bit)`.
    - Select `Choose Storage`, then choose the attached microSD card.
    - Click `Next`.
1. Apply operating system customizations:
    - When asked, `Would you like to apply OS customisation settings?`, choose `Edit Settings`.
    - Check the checkbox for `Set hostname`.
    Change the `raspberrypi` to another name, e.g., `adsbreceiver`.
    - Check the checkbox for `Set username and password`.
      - The default username is `pi` by convention; the authors recommend using `pi`.
      However, you may use something different if you wish.
      - Enter a strong password that you will remember
    - Check the checkbox for `Configure wireless LAN`.
    Then, enter the SSID (network name) and password for the wireless network.
    For `Wireless LAN country`, backspace `GB` and type the two-letter ISO country code for the country where the Raspberry Pi will be used.
    For the United States, type `US`.
    - Check the checkbox for `Set locale settings`.
    Select the dropdown for `Time zone` and choose the best time zone for your location (if it isn't already).
    Select the dropdown for `Keyboard layout` and choose the best keyboard layout.
    For US usage, the keyboard layout should be `us`.
    - Click the `Services` tab at the top.
    - Check the checkbox for `Enable SSH` and leave `Use password authentication` selected.
    - Click `Save`
    - Back on the `Would you like to apply OS customisation settings?`, choose `Yes`.
1. Start writing the image to the microSD card:
    - Select `Yes` to confirm that all data on the microSD card will be erased.
    - Wait for the process to complete.
    - When it completes and the `Write Successful` dialog appears, click `Continue`.
    - Close Raspberry Pi Imager when done.

### Assemble the Raspberry Pi Into Its Case

1. Remove the microSD card from the microSD card reader and insert it into the Raspberry Pi.
1. Remove the Pimoroni Pibow from its paper bag.
1. Each piece of acrylic that comprises the Pimoroni Pibow case will have a plastic wrapping attached to it.
Peel the plastic wrapping away from each piece and discard it.
1. Three of the acrylic pieces will be numbered: 0, 1, and 2.
Starting with piece 0, stack piece 1 on top such that the 1 is right over the 0.
The numbers should be in the upper left.
1. Place the thinner piece of clear acrylic in the middle of piece 1 such that it is laying on top of piece 0.
The long rectangular cut-away should be at the top, and the lower right should have two smaller rectangles.
1. Place the Raspberry Pi Zero 2 W on top of the thin piece of clear acrylic such that it fits into the center of piece 1.
The top edge of the Raspberry Pi Zero 2 W should be even with the top edge of piece 1
1. Place piece 2 on top of piece 1 such that the 2 is on top of the 1.
1. Place the thicker, clear piece of acrylic over piece 2 such that the white USB and power symbols are over the micro USB ports in the lower right.
1. Insert the nylon bolts into the outermost four corners of the Pibow.
1. Carefully flip the Pibow over.
Thread the nuts onto each bolt and hand-tighten them.

### Determine the Current List of IP Addresses

1. Ensure that the technician's computer is connected to the same wireless network that the Raspberry Pi will be.
1. Start PowerShell, then run the following commands:

```powershell
Invoke-Expression (Invoke-RestMethod 'https://raw.githubusercontent.com/proxb/AsyncFunctions/master/Test-ConnectionAsync.ps1')
$results1 = Ping-Subnet
```

Keep PowerShell open while you complete the following steps.

### Power On the Raspberry Pi

1. Connect a power cable to the Raspberry Pi.
After a few moments, it will power on. The Raspberry Pi will take a few minutes to complete its initialization process.

### Connect to the Raspberry Pi Using SSH

1. If you haven't already waited a few minutes, please do!
The Raspberry Pi will take at least three minutes to boot up and connect to the wireless network - maybe more!
1. Once you believe the Raspberry Pi is connected, try pinging it using its hostname.
For example, try: `ping adsbreceiver.local` and see if you get a response.
    - If you get a `Ping request could not find host <hostname>. Please check the name and try again.`, then the Raspberry Pi is not yet booted up - or it failed to connect to the network.
    Wait a bit and try again!
1. If you have **not** successfully pinged the Raspberry Pi using its name after waiting 5-10 minutes, try running the following PowerShell command on the technician's computer: `$results2 = Ping-Subnet`
    - Compare `$results1` and `$results2` to find the new IP address that appeared in `$results2`.
The new IP address should be the Raspberry Pi. To display, for example, `$results2`, simply type `$results2` in PowerShell.
1. At the PowerShell prompt or a Command Prompt, type:

    `ssh -l pi adsbreceiver.local`

    where `pi` is the user name that you entered during the customization process and `adsbreceiver.local` is the hostname of the Raspberry Pi or its IP address.
1. You should receive a warning message that the authenticity of the host can't be established.
Type `yes` and press Enter.
1. Enter the password that you assigned during the customization process.

You should now be connected to the Raspberry Pi via a terminal window!

### Enable Automatic Operating System Updates

1. At the terminal prompt, type the following commands:

    ```bash
    sudo apt update
    sleep 2
    sudo apt -y install unattended-upgrades
    ```

    The update may take a few minutes to complete.
1. At the terminal prompt, type:

    `sudo dpkg-reconfigure -plow unattended-upgrades`

1. When asked if you want to `Automatically download and install stable updates?`, use the arrow key to highlight `Yes`, then press `Enter`.
1. If you receive a message that a new version of the 50unattended-upgrades file is available, use the keyboard's arrow keys to highlight the option to install the package maintainer's version, then press `Enter`.
1. At the terminal prompt, type:

    `sudo editor /etc/apt/apt.conf.d/50unattended-upgrades`

1. Use the arrow keys to navigate downward to the section `Unattended-Upgrade::Origins-Pattern`
1. If it exists, comment out the line:

    `"origin=Debian,codename=${distro_codename},label=Debian";`

    by inserting `//` in front of it.

1. Next, if it exists, comment out the line:

    `"origin=Debian,codename=${distro_codename},label=Debian-Security";`

    by inserting `//` in front of it.
1. As needed, add the following lines above the line with the closing brace (`};`):

    ```text
    "origin=Raspbian,codename=${distro_codename},label=Raspbian";
    "origin=Raspberry Pi Foundation,codename=${distro_codename},label=Raspberry Pi Foundation";
    ```

1. Next, use the arrow keys to scroll down to find the line that contains
`Unattended-Upgrade:Automatic-Reboot`.
Modify it to remove the `//`, which uncomments the line.
Change the `false` to a `true`.
The line should now read:

    `Unattended-Upgrade:Automatic-Reboot "true";`

1. Use the arrow keys to scroll down to find the line that contains
`Unattended-Upgrade:Automatic-Reboot-WithUsers`.
Modify it to remove the `//`, which uncomments the line.
Ensure that the setting is configured to `true`.
The line should now read:

    `Unattended-Upgrade::Automatic-Reboot-WithUsers "true";`

1. Use the arrow keys to scroll down to find the line that contains
`Unattended-Upgrade::Automatic-Reboot-Time`.
Modify it to remove the `//`, which uncomments the line.
If desired, change the time to something other than 2:00 AM.
For example, to reboot at 4:00 AM, modify the line to look like:

    `Unattended-Upgrade::Automatic-Reboot-Time "04:00";`

1. If the Raspberry Pi will primarily be used on cellular Internet, use the arrow keys to scroll down to find the line that contains
`Unattended-Upgrade::Skip-Updates-On-Metered-Connections`.
Modify it to remove the `//`, which uncomments the line.
Change the `true` to a `false`.
The line should now read:

    `Unattended-Upgrade::Skip-Updates-On-Metered-Connections "false";`

1. Press `Ctrl` + `O` to save the file.
Press `Enter` to confirm the file name.
Finally, press `Ctrl` + `X` to exit.

### Run an Initial Check for Updates and Verify That It's Working

1. At the terminal prompt, type:

    ```bash
    sudo apt-get update
    sleep 2
    sudo unattended-upgrade --dry-run
    ```

    The command will take several minutes to complete.
1. Finally, type:

    `cat /var/log/unattended-upgrades/unattended-upgrades.log`

    And review the log output to ensure that packages are listed for update/upgrade

### Update the Raspberry Pi, Including its Firmware

**Note**: these steps assume that you are installing the latest stable firmware release (recommended).
If, for some reason, you need to install a beta firmware instead of the latest stable release, check out [the rpi-update project](https://github.com/raspberrypi/rpi-update).

1. At the terminal prompt, type the following commands:

    ```bash
    sudo apt update
    sleep 2
    sudo apt -y full-upgrade
    ```

    The update may take a long time to complete.

1. When the process completes, type the following command:

    `sudo reboot now`

### Reconnect to the Raspberry Pi Using SSH

1. Wait a few minutes for the Raspberry Pi to reboot.
1. At the PowerShell prompt or a Command Prompt, type:

    `ssh -l pi adsbreceiver.local`

    where `pi` is the user name that you entered during the customization process and `adsbreceiver.local` is the hostname of the Raspberry Pi or its IP address.
1. Enter the password that you assigned during the customization process.

## Install and Configure the FlightAware Software

### Install PiAware

1. Download and install the FlightAware APT repository package, which tells apt (Raspberry Pi OS's package manager) how to find FlightAware's software packages in addition to the packages provided by Raspberry Pi OS/Debian.
Do this by running the following two commands at the terminal prompt:

    ```bash
    wget https://www.flightaware.com/adsb/piaware/files/packages/pool/piaware/f/flightaware-apt-repository/flightaware-apt-repository_1.2_all.deb

    sudo dpkg -i flightaware-apt-repository_1.2_all.deb
    ```

1. Install PiAware by running the following commands at the terminal prompt:

    ```bash
    sudo apt update
    sleep 2
    sudo apt -y install piaware
    ```

### Configure PiAware to Update Automatically and Allow Manual Updates, Too

At the terminal, run the following commands:

```bash
sudo piaware-config allow-auto-updates yes
sudo piaware-config allow-manual-updates yes
```

### Install dump1090 and/or dump978

1. At the terminal, run the following command:

    `sudo apt -y install dump1090-fa`

1. If you intend to use a dual software-defined radio setup to also receive 978 MHz, install dump978 by running the following command:

    `sudo apt install dump978-fa`

    **Note:** if you are not located in the United States, or if do not intend to support 978 MHz UAT, skip this step.

1. Shut down the Raspberry Pi by running the following command:

    `sudo shutdown now`

    Wait approximately one minute for the shutdown to complete, then remove the power from the Raspberry Pi.

## For Portable ADS-B Receiver Setups: Add the GPS HAT

**Note:** the steps in this section only apply to people building a portable ADS-B receiver (e.g., for in-vehicle, camping, or other travel usage). If you are not building a portable receiver, skip this section!

### Install the GPS HAT

The following are the "rough" steps to physically assemble the Ozzmaker BerryGPS HAT.
Please refer to the Ozzmaker documentation for a more complete set of instructions.

1. Solder the 10-pin GPIO header to the bottom of the BerryGPS board.
1. Insert the 25mm nylon screws (purchased separately from the BerryGPS as noted in the bill of materials) into the top of the BerryGPS HAT.
1. Carefully flip the BerryGPS HAT upside-down, laying it on top of a static-proof bag/mat.
1. Carefully insert exactly nine nylon washers onto each bolt.
1. Carefully install the BerryGPS HAT onto the Raspberry Pi Zero 2 W, ensuring that the HAT's GPIO header inserts itself correctly onto the ten left-most pins of the GPIO header on the Raspberry Pi.
1. Once done, carefully thread the nylon nuts onto the bolts.
1. Use a precision Phillips screwdriver to tighten the nylon bolts onto the nuts, being careful not to overtighten them.

### Connect the GPS Antenna

1. On the uFL to SMA "pigtail" that came with the GPS antenna, connect the uFL end (small connector) to the GPS HAT.
1. Flip the switch from `Int` to `Ext`.
1. Connect the GPS antenna to the SMA connector on the "pigtail".

## Connect the Software-Defined Radio, Antenna, and Related Equipment

The steps in this section depend on the "option" that you selected in the Bill of Materials section, above.

### Connect Option 1: Simplest Setup - Stationary, Only Outdoor As-Needed, Proof of Concept

1. Connect the Raspberry Pi to the software-defined radio:
    - Connect the micro USB end of the **"OTG" adapter cable** (micro USB (male) to USB (female)) to the USB port on the Raspberry Pi Zero 2 W.
    - Connect the **software-defined radio** to the full-size USB port.
1. Connect the antenna to the software-defined radio:
    - Connect the SMA end of the **N (male) to SMA (male)** to the software-defined radio.
    - Connect the antenna to the N connector on the cable.
1. Connect the power to the Raspberry Pi to boot it up.

### Connect Option 2: Indoor ADS-B Receiver

Foo

### Connect Option 3: Portable ADS-B Receiver - Vehicle, Camping, or Other Travel Usage

Foo

### Connect Option 4: Permanent Outdoor Setup

Foo
