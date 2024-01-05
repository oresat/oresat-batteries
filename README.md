# OreSat Battery Card

This is the battery card for the OreSat system. You can have up to four of these cards in any OreSat system, ID is set by resistors.

## Version History

- v1.0: We don't talk about v1 (EAGLE).
- v2.0: First true pack with 3D printed cell storage. Many small issues (EAGLE).
- v3.0: Flight ready pack, flown 2021-03-15 on OreSat0. Worked! (EAGLE).
- v3.1: Imported into KiCAD, small tweaks in process (KiCAD).

## Bill of Materials

- [PCBA](https://docs.google.com/spreadsheets/d/11vG1kWyrAjmbF5QJM-EhXmvQoF6O47japwYlHF802do/edit#gid=576852939)
- [Assembly](https://docs.google.com/spreadsheets/d/1GmZE2MR5q2XFSR2Y-65dwwPd7tem3i0CEjZBlCLeJBU/edit?usp=sharing)

## Technical Information

- 2 independent 2S1P 18650 battery packs (4 batteries total, in 2 packs). Each pack has:
   - Battery protection circuitry for charge and discharge control for each 2S1P pack
   - MAX17500 fuel gauge with cell balancing and cell temperature monitoring
- 3D printed battery holder, designed for CRP Windform SLS Nylon material (OK with Form3 Flame Retardant Resin).
- Positive battery lead disconnection switches (with room for plunger assemblies that go through OreSat +/-X rails).
- Satellite shutdown switches (!SHUTDOWN) (with room for plunger assemblies that go through OreSat +/-X rails).
- Flexible polyimide heater tapes to keep batteries above 0 C
- STM32F091 microcontroller (controlled by the OreSat Power Domain) to interface everything to the CAN bus

![OreSat Battery Card v3.0 Picture](https://github.com/oresat/oresat-batteries/blob/master/oresat-battery-card.png)

## TRANSITION DOCUMENT
Documentation is moving to [Google Docs](https://docs.google.com/document/d/1yaPGtJSM5RQG5k7sVpsDxBkVu4CHtm4LMNnisgOaNRg/edit#heading=h.gjdgxs). The rest of this readme will be integrated later.

## OreSat Vbus
OreSat main power bus is just our 2S Li Ion battery pack, which we've defined as 6.0 V minimum, 7.2 V typical, and 8.4 V maximum. Everything is directly connected to that bus, including:

- Other Oreat battery cards. For >= 2U CubeSats, you'll probably have at least two battery cards.
- All solar panels. Thus the solar panels see the direct battery voltage and can dump directly into the battery packs, thus doing _actual_ MPPT.
- All loads.


## Battery Card Topology
Confusingly, Each OreSat battery card has 2 independent battery packs on it. Each pack is made of a 2S1P 18650 LiPo cell pack. So it's a 2x2S1P if you will. Despite this, each card is independent and the OPD is fully capable of functioning with only one active card.

We decided to separate the packs instead of combining them into a single 2S2P pack for the sake of redundancy, we can lose one cell and the other pack can still provide power to the OPD, as opposed to losing an entire card from a single cell failure.  This design does come with the added drawbacks of extra overhead in the form of requiring more charge protection and fuel gauge circuitry.


## Charge/Discharge Protection
We are using Ablic S8209 to monitor our cell voltage levels and disable charging if the cell voltage goes above the predetermined overcharging threshold or disable discharging if the cell voltage goes below a predetermined discharging threshold.

The S8209 has two inputs that, if activated, will trigger the either the over-charge or under-charge protection to activate.  This allows us to daisy chain these chips together, so that each cell has its own charge protection IC.  It also allows us to connect them to a GPIO on an onboard microcontroller so that we can manually disable charging or discharging functionality if needed.  Below is a state diagram that depicts the desired behavior of our charging and discharging protection circuit

![Battery Disable State Diagram](https://github.com/oresat/oresat-batteries/blob/master/docs/OreSat%20Battery%20State%20Diagram.png)

The output of the S8209 uses an internal N-channel MOSFET, which has the potential to latch from radiation exposure.   To combat this, we have P-channel MOSFETS in series with the VDD of each S8209, the gate of the MOSFET is connected to our onboard microcontroller, so that we can periodically power cycle the S8209 and clear the N-channel latch.

The actual control of charge and discharge are done using Linear LTC4412 ideal diode controllers. Totally bizarre, but indepdenently controllable!


## Fuel Gauge and Cell Health Monitoring
Each 2S1P battery back will have two MAX17205 fuel gauges to monitor the packs current state of charge, and the long term health of the battery.  Each MAX17205 will be reporting both the direct State of charge (in mAh) and the percentage over I2C to our M0, compensating for factors such as cell degradation and temperature ranges.  We are still testing the accuracy of the MAX17205's compensation algorithm's for extreme charge levels and temperature in our lab.  At this point we don't know how much we should be relying on these compensation algorithms, they could be amazing! Or horrible! We don't know! Yay science!

Each fuel gauge is also monitoring the battery temperatures with a single internal sensor and two external thermistors (we are still determining the best position for these thermistors).  This gives us a wackton of data which is good because we are data hoarders.

Finally the MAX17205 also does cell balancing, which we'll enable while charging.


## Heaters
Thermal models of both 1U and 2U CubeSats show an average "passive" battery temperature of ~ -10 C). That's not good. We can't charge the battery below 0 C. So we've added heaters to heat up the batteries when charging if necessary.

We're using two Minco HK6903 heaters in parallel. They have "vacuum OK" (apparently?) urethane PSA that we'll use to attach them to the batteries. Note that they're 12.7 x 100 mm, and each heater goes across both batteries. At 49 ohms +/- 10% each, that's 25 ohms in parallel. Unclear on the heater's tempco. We'll ignore for now.

- At 8.4V, that's 336 mA = 2.8 W (at 100% DC)
- At 6.0V, that's 240 mA = 1.4 W (at 100% DC)
- We can of course PWM them and reduce the power as necessary

With that much current draw, we're going to need to modify the OPD circuit breaker to allow more current.


## Batteries

- Tenergy model "30005-0 (ICR 18650-2600, LR1865SK)"
- Rated at 2600 mAh
- Max voltage: 4.2 V
- Typical voltage: 3.6 V
- Minimum voltage: 2.75 V
- Maximum charge current: 1.25 A
   - 0 - 45 C only
- Maximum discharge Current:
   - -20 - 5 C = 1.25 A
   -  5 - 45 C = 5 A
   - 45 - 60 C = 3.75 A

### Temperature

Just to summarize the battery temperature operational range:

- Cell temperature < -20 C -- DEAD
- Cell temperature < 0 C -- DISCHARGE ONLY
- Cell temperature < 45 C -- NORMAL OPERATION
- Cell temperature > 45 C-- DISCHARGE ONLY
- Cell temperature > 60 C-- DEAD

### Cell failure (short)

- Short will plummet battery to < 2.5V (Vlvco) and S-8209 will disconnect the battery from discharging but not charging


## Battery pack behaviors (circuitry + firmware)

- Automatic discharge if > 6.0 V
- Automatic slow charge if < 8.4 V
- Fast charge if battery pack within ~ 0.5 V of other batteries

## Hacking the OPD Circuit Breaker

The OPD circuit breaker is supposed to trip at 300 mA without any division at the current output. Since the headers could pull up to 336, we need to probably go up to that plus 50 mA. So we put in a 10K and 20K to make a 0.66 divider and an N CH MOSFET to turn on the divider on the signal "MOARPWR". Asserting MOARPWR should give you up to 400 mA.

## LICENSE

Copyright Portland State Aerospace Society 2020.

This documentation describes Open Hardware and is licensed under the CERN OHL v. 1.2.

You may redistribute and modify this documentation under the terms of the CERN OHL v.1.2. [http://ohwr.org/cernohl](http://ohwr.org/cernohl). This documentation is distributed WITHOUT ANY EXPRESS OR IMPLIED WARRANTY, INCLUDING OF MERCHANTABILITY, SATISFACTORY QUALITY AND FITNESS FOR A PARTICULAR PURPOSE. Please see the CERN OHL v.1.2 for applicable conditions.

