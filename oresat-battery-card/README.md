# "oresat-proto-card"

This is the "prototype" card for OreSat, meant to pioneer future cards and test the backplane, and be an 
easy way to hack in functionality. NOT MEANT FOR FLIGHT.

## Features

- Populates the two 1.27 mm connectors from the backplane.
- Populates all three SMPM RF connectors from the backplane.
- Standard TPS63070-bsaed buck/boost SPS (Vin = 2.5 to 7 V, Vout = 3.3V)
- Has our standard/favorite STM32F042K6 microcontroller.
   - With SWD, UART (FTDI), and CAN (to the backplane).
- Has lots of breakouts, but DO NOT POPULATE THE 0.1 IN CONNECTORS, THEY'RE TOO TALL!!!

![OreSat ProtoCard Picture](https://github.com/oresat/oresat-proto-card/blob/master/oresat-proto-card.png)

## Connector information

- Main 1.27 mm connector ("Center" connector)
   - Samtec TFM-120-01-L-D-RA
   - https://www.digikey.com/products/en?lang=en&site=US&keywords=TFM-120-01-L-D-RA
   - Drawing: http://suddendocs.samtec.com/prints/tfm-1xx-xx-xxx-d-xxx-mkt.pdf
   - Brochure: http://suddendocs.samtec.com/catalog_english/tfm.pdf
   - 3D CAD: https://www.samtec.com/partnumber/tfm-120-01-l-d-ra?vendor=digikey#technical
   - Notes: the -RE1 model is better because of the solder stakes, but the female connector doesn't fit! The stakes get in the way :(

- Auxiliary 1.27 mm ("alt-left" connector)
   - Samtec TFM-110-01-L-D-RA

- RF (70 cm, L band, S band) conenctors
   - Card
      - Molex 73300-0030 (possible replacement is 73300-0020 but it has full detent that we don't want)
      - SMPM Connector Plug, Male Pin 50 Ohm Board Edge, Cutout; Surface Mount Solder, smooth bore
      - Molex w/3D: https://www.molex.com/molex/products/datasheet.jsp?part=active/0733000030_RF_COAX_CONNECTORS.xml
   - Barrell (connects card to backplane)
      - Molex 73300-0220 (alt but slightly different lengths: -0223, -0228)
      - SMPM female to female barrel adapter 
      - RF Adapters - In Series SMPM JACK/JACK BULLET .21IN
      - Molex w/3D: https://www.molex.com/molex/products/datasheet.jsp?part=active/0733000220_RF_COAX_CONNECTORS.xml

## OreSat Power Domain (OPD)

You can find more design information on the OPD [here](https://docs.google.com/document/d/1NOkO6gGEJUFRpOrkwClRlZlzWuTLteq6fP9LPf_Ez0M/edit?usp=sharing).


## LICENSE

Copyright Portland State Aerospace Society 2018.

This documentation describes Open Hardware and is licensed under the CERN OHL v. 1.2.

You may redistribute and modify this documentation under the terms of the CERN OHL v.1.2. [http://ohwr.org/cernohl](http://ohwr.org/cernohl). This documentation is distributed WITHOUT ANY EXPRESS OR IMPLIED WARRANTY, INCLUDING OF MERCHANTABILITY, SATISFACTORY QUALITY AND FITNESS FOR A PARTICULAR PURPOSE. Please see the CERN OHL v.1.2 for applicable conditions.

## Version Information

Version | Date       | Notes
--------|------------|-------------------------
3.0     | 2018/11/22 | Big changes in structure, mostly springs. Changed RF connectors to SMPM. Optimized layout for more proto area. 
2.0     | 2018/03/?? | New 1.27 mm connectors, again.
1.0     | ??         | New connectors.

