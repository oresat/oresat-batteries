---
title: Batteries
layout: default
nav_data:
  - name: Power System
    link: /github_pages/OreSat_Status/Design/Power_System/
    repo: /oresat-design
    defcolor: red
---
# OreSat Battery Board V2.0

## The Jeremey Clarkson Methodology to Power Management (MORE POWAAHH)
The OreSat Power Domain (OPD) is designed with modularity as a key factor in design decisions.  We are confident that our system will behave spectacularly in flight (whether that is spectacularly well or spectacularly bad remains to be seen).

## Battery Pack Topology
Each OreSat battery card has 2 battery packs, each pack consisting of 2S 18650 LiPo cells.  We will be flying two battery cards connected to the OPD, both to keep our center of mass balanced around our geometric center, and for the added capacity.  Despite this, each card is independent and the OPD is fully capable of functioning with only one active card.

We decided to separate the packs instead of combining them into a single 2S2P pack for the sake of redundancy, we can lose one cell and the other pack can still provide power to the OPD, as opposed to losing an entire card from a single cell failure.  This design does come with the added drawbacks of extra overhead in the form of requiring more charge protection and fuel gauge circuitry.  

## Charge/Discharge Protection

We are using Ablic S8209 to monitor our cell voltage levels and disable charging if the cell voltage goes above the predetermined overcharging threshold or disable discharging if the cell voltage goes below a predetermined discharging threshold.  

The S8209 has two inputs that, if activated, will trigger the either the over-charge or under-charge protection to activate.  This allows us to daisy chain these chips together, so that each cell has its own charge protection IC.  It also allows us to connect them to a GPIO on an onboard microcontroller so that we can manually disable charging or discharging functionality if needed.  Below is a state diagram that depicts the desired behavior of our charging and discharging protection circuit

![Battery Disable State Diagram](https://github.com/oresat/oresat-batteries/blob/master/docs/OreSat%20Battery%20State%20Diagram.png)

The output of the S8209 uses an internal N-channel MOSFET, which has the potential to latch from radiation exposure.   To combat this, we have P-channel MOSFETS in series with the VDD of each S8209, the gate of the MOSFET is connected to our onboard microcontroller, so that we can periodically power cycle the S8209 and clear the N-channel latch.  


## Fuel Gauge and Cell Health Monitoring




# Current Design Documentation
- [Google Docs](https://docs.google.com/document/d/1m6FopMepRnWKUnyaSLvasRHum4sx9E_MLduOtIjwnw8/pub)
- If you need editing permission ask either Austin or Andrew










