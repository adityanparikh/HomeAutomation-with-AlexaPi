### AC Voltage Regulator using MOSFET

#### Components Used:
- IRF 830B Power MOSFET: It is an N Channel Power MOSFET which has low switching losses, low input capacitance and at the same time a low-cost device. It can sustain peak voltages of over 500V.It’s industrially used in LCD Displays, SMPS, Induction cooking tops and Battery chargers.
- Power Diode Bridge Rectifier: This contains 4 power diodes which can sustain peak voltages of over 500 V. Only one pair of diodes function for each half cycle. This is especially useful for operating the MOSFET in both the half cycles. 
- Resistors of value 1k ohm and 10k ohm

#### Operation of the Circuitry
In order to control a lamp load, there needs to be an intermediary between using voice-controlled GPIO pin usage and lamp load. So, to control the AC load, a MOSFET is used. The only problem with MOSFET is that it works only in a unidirectional sense. So, to overcome this problem, the circuitry which allows the control of AC load using a bridge rectifier circuit is to be used. This makes it possible to utilize the MOSFET as well as allows the PWM operation which makes it possible for intensity control.

![alt text](https://github.com/adityanparikh/HomeAutomation-using-AlexaPi/blob/master/Regulator.png)

The load used is of resistive type and is supplied with regular AC Supply (230 V 50 Hz). When the positive phase of AC supply is utilized, the current flows through one pair of diodes using direction mentioned in the figure below.

![alt text](https://github.com/adityanparikh/HomeAutomation-using-AlexaPi/blob/master/Positive.png)

As the positive half cycle ends the negative half cycle occurs which causes the other pair of diodes to come into action which makes it appear as DC voltage supply across the MOSFET (Q1). Thus, MOSFET always ‘sees’ a positive voltage as the drain is always positive with respect to the source.

![alt text](https://github.com/adityanparikh/HomeAutomation-using-AlexaPi/blob/master/Negative.png)

Therefore, the use of MOSFET for controlling the AC load which can be effectively done with help of PWM Signal generated by Raspberry Pi’s GPIO Pin.

#### Need for Gating Mechanism

The Raspberry Pi’s GPIO pins work on a 3.3V Logic level which is insufficient to drive the MOSFET’s Gate. So, an additional circuitry is employed which as gate driver circuit and provides electrical isolation between the MOSFET and Raspberry Pi.


### Gate Driver Circuit

#### Components used

The gating mechanism can be achieved using a simple circuitry of a step-down transformer (230/15 V), a single-phase rectifier, a voltage regulator (LM7814) and an opto-isolated gate driver IC (TLP 250).

#### Power Circuitry

Since MOSFET requires a gating voltage of 15 V to drive it, a step-down transformer (230/15 V) is used to power the circuitry. The stepped down voltage is then supplied to a bridge rectifier since the TLP 250 circuitry utilizes a DC voltage rather than readily available AC voltage supply from the inlet.

There is extra protection provided to TLP250 by installing a voltage regulator which is responsible for not allowing voltage to rise above a 15V level which could potentially damage the TLP circuitry and thus make the gate driver unusable.

#### TLP Circuitry

TLP Circuit takes in the 15 V supplied via the power circuitry as VCC which is used to drive the IC. The main input taken by the IC is gating signal via GPIO Pin from Raspberry PI at the anode of the TLP while virtual ground of the Raspberry Pi is attached to the Cathode. The output of TLP is connected across the Gate and Source of the MOSFET.  

![alt text](https://github.com/adityanparikh/HomeAutomation-using-AlexaPi/blob/master/GDC.png)

The IC also provides electrical isolation between the voltages driving the MOSFET and voltages at Raspberry PI for controlling the output by providing a method of optical isolation.

By this method, the gating signal at anode and ground signal at Anode turns on the LED which has its intensity fall straight on LDR which allows the output to be driven.

The output is taken from pin 6 and ground from pin 5, this way output and input side have no physical connection between them and hence are electrically isolated and thus there will be no damage to Raspberry Pi or MOSFET in case of any fault occurring.
