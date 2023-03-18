# Engine speed control

Functional definition – The device detects the difference between the given engine speed and the speed desired by the user.
Capabilities:
- Has the ability to receive data using switches and a speed sensor.
- Has the ability to display data to the end user using 4 seven segments modules and an additional seven segment module to display the amount of speed needed to be added/deducted.
Specifications:
- Voltage to speed ratio = 3.2 [kmH/1V]
- The system is built to test speeds up to 15 [km/h]
- The system consists of a speed sensor and electronic components, D/A converter with Rd/a=0.3125[V] and an A/D converter with RA/d=0.019[V].
- The system will display the speed difference with a deviation of up to +1 [km/h].
- System supply voltage of 15[V].
- Output voltage according to the requirements of the circuit components (listed).
- Form factor: 4*16.4*5.3*8 [cm] (matrices).

![image](https://user-images.githubusercontent.com/105777016/226122122-2368739d-70df-4e79-a174-07e431123a67.png)

Subject matter – What am I about to solve:
Assume we have a small engine with output voltage up to 5[V], and I would like to know what speed the Controller/Main computer will have to add/subtract from the engine speed in order to reach the speed requested by the end user.
Since it is a small engine that produces low output voltages, the system will have to handle low speeds up to 15[km/h].
The solutions offered today are portable tachometers that measure rpm and speed using a sensor mounted on the wheel or using a laser meter that measures the engine RPM.
Sine I do not have the possibility to this in my room and I do not have the option a control system, I’ll assume that the output voltage of the engine is a known variable.
Note – The system will function as part of a complete engine speed control system. Therefore, I can have the assumption that the engine speed at any given moment is known.
Planning considerations:
The system will consist of a digital components without any traditional memory (RAM, ROM). Since I work in an office and I’m limited in terms of components, I’ll not be able to use a laser sensor or a sensor mounted on an experimental wheel, so I’ll assume that the output voltage of the motor is variable voltage.
This voltage will enter the system to the second ASSEMBLY and will be converted to the speed in the ratio determined.
The incoming voltage from the engine is an analog voltage. The voltage that the end user enter is a digital voltage that is converted to analog voltage in the first ASSEMBLY. The subtraction is being made between the two analog voltages.
In the fifth ASSEMBLY, the result of the analog subtraction is being converted to a digital voltage using an A/D converter with a resolution that I determined according to the conversion ratio.
The user will enter a voltage in the first ASSEMBLY and will be able to see the speed that voltage represents on the 7-Segment displays in the first ASSEMBLY.
In the second ASSEMBLY, the engine voltage is being converted to speed and compared to the speed that the end user requested and entered in the first ASSEMBLY.
The incoming voltages in the first and second ASSEMBLY will be at relatively low values (up to 15[V]), according to the supply voltages of the components being used and the data on the respective manufacturer’s pages.
If the incoming voltage from the engine is different from the voltage requested by the end user, the system will show the user the difference between the voltages converted to the parameter in the fifth ASSEMBLY on the 7-Segment displays.
By the difference that will be shown to the end user in the fifth ASSEMBLY, he will be able to know whether the engine is working properly or, (if it is a normal engine) whether he should increase of decrease the speed of the engine (meaning – increase or decrease the voltage level leaving the engine and entering the system) in order to reach the desired value.
The system will display both negative values in which the engine reaches a speed higher than the speed the user wanted and positive values in which the engine reaches a speed lower than the speed the user wanted and the difference that will be shown to the end user will show the values of the speed that he must add (no sign or lower (a minus sign will be displayed).
This examination of whether the difference is negative is carried out in the third ASSEMBLY – the voltage selection ASSEMBLY.
Some Justifications for thee way I chose to design this system functionality:
I could have assume that the system measures the RPM of the engine, but I did not do so – because, if the system measured RPM I could not use it as a speed control system in addition and then the system would only be used as a speedometer, having a low to none benefit to the potential customer since those kind of products are already available.
In addition, the values of the RPM required as significant resolution change in the system, which in terms of cost Vs. benefit was simply not feasible for me.
If the system measured RPM only, I would have required more components and a change in the D/A converter resolutions. From a careful examination I carried I conducted the cost-benefit ratio would have decreased and it would not be worthwhile to use such a system.
Moreover, I chose to build this system according to the engine voltage out of considerations of expediency and versatility. The device can be also used as a speedometer for normal tools and help the end user reach the desired speed, thus solving the mentioned problem.
Flowchart:
The system is taking as input two speeds:
1) Engine speed desired by the end user.
2) Given engine current speed.
The system will calculate the difference between those inputs, same difference will be displayed on the 7-Segment displays in addition to the end user’s desired speed.

![image](https://user-images.githubusercontent.com/105777016/226122138-6bbc5424-5c0d-4eba-a676-c8d162dd132c.png)



System Parts:
1) D/A Converter:

![image](https://user-images.githubusercontent.com/105777016/226122144-b2c29dc1-8b4e-4730-a854-a3891c134e38.png)


The first part is responsible for receiving the desired voltage from the end user. I used the “Digital to Analog convertor” a.k.a D/A which I implemented myself.
The resolution of my conveter is: Vref =5[V]/(2^4)=0.3125[V]
The converter was built from regular switches so that the end user can use them to enter his desired speed. The voltage received at the D/A output will be an analog voltage that represents the digital bits received from the end user, this voltage will vary between 0[V]-5[V].
Below you can see that truth-table according to which this part was designed:

![image](https://user-images.githubusercontent.com/105777016/226122149-87d215fc-d571-49b0-978b-80847ba17cff.png)


Implementation:


![image](https://user-images.githubusercontent.com/105777016/226122160-46c03652-aaf6-4f49-bfc4-4ddba2a8d44a.png)


2)  Difference Calculator

![image](https://user-images.githubusercontent.com/105777016/226122166-fdb1d0c0-da88-408f-953a-9fedc246fbf6.png)


The second part is responsible for finding the difference between the speed desired by the end user and the existing engine speed.
The structure of this part goes as following:
a. I used inverting operational amplifier (inverter) in order to get the desired voltage as positive because the D/A converter output always receives a negative voltage.
b. A differential operational amplifier into which the engine voltage enters the negative trigger and the positive trigger through the inverter whose purpose is to give us the difference.

![image](https://user-images.githubusercontent.com/105777016/226122175-17c6e3b7-e700-4da8-ab4b-f94340be75f6.png)

3) "Selector and fixer”

![image](https://user-images.githubusercontent.com/105777016/226122182-4eeaabda-ec90-4122-8092-1043224e51d0.png)

The third part is responsible for selecting the voltage obtained from part 2 (the differential voltage). The voltage obtained can be negative/positive/
I’ll perform a number of manipulations which in the end will provide a proper input voltage to the A/D converter which represents the differential voltage.
The structure of this part is as follows:
1) A comparator amplifier whose purpose is to find out which way to transfer the voltage, the positive terminal is fed that voltage from part 2 (difference calculation) and the negative terminal is ground.
2) Using two relays (S7,S10) that open and close depending on the voltage at the output of the comparator.
3) From there, depending on the chosen route, a reversal is performed by a inverter or a direct transition.
4) After that, a correction is made by a voltage divider calculated in advance to a value of 0.3[V] which in it’s end the voltage enters the A/D converter.

![image](https://user-images.githubusercontent.com/105777016/226122186-e7730b0d-2954-40af-b8fd-4738afbd0c3f.png)

This mechanism can be seen in the flowchart below:

![image](https://user-images.githubusercontent.com/105777016/226122192-a28545b8-3025-41a4-96f4-90825300eb5c.png)

4)Analog to Digital Converter:

![image](https://user-images.githubusercontent.com/105777016/226122196-19b5cc49-973f-4682-b53e-e2063f456d4b.png)

The fourth part is responsible for converting the analog voltage from the output of part 3 (“ Selector and fixer”) to it’s representation in digital voltage. Converter specifications:
- Vref=5[V]
- Clock frequency=1[kHz]

5)Sub system-decoder

![image](https://user-images.githubusercontent.com/105777016/226122200-68c0b031-c855-4397-b3ee-f2114818fbcc.png)

This part is responsible for transferring the digital voltage in an orderly manner according to the 7-Segment logic. This part exists twice :
1) Subsystem 1 – Connected to our D/A and shows us the desired speed from the end user in [km/h].
2) Subsystem 2 - Receives a digital voltage from the A/D output and shows us the correction (difference) that we will have to perform on the engine, in order to reach the desired speed and the sign.
When the sign bit is off – it means that the voltage must be added to the engine to reach the desired voltage.
When the sign bit is on – it means that the voltage must be subtracted from the engine to reach the desired voltage.
Subsystem 1:

![image](https://user-images.githubusercontent.com/105777016/226122215-1c638de7-72f1-45be-84a3-278356328e4b.png)

![image](https://user-images.githubusercontent.com/105777016/226122220-6ccef23c-cece-41fc-a5a0-812cc16b37bd.png)

Subsystem 2:

![image](https://user-images.githubusercontent.com/105777016/226122223-9649be91-c480-46c5-bff9-e60971657458.png)

![image](https://user-images.githubusercontent.com/105777016/226122226-d42b5596-5d4e-4b65-b1bc-730eed1d4c22.png)

According to the functions received, I implemented logic gates that faithfully represented them.

Simulations & Errors fixing:

During the simulation due to the assumption that all the components are ideal, I found out that the result of the voltage difference is slightly lower compared to what I expected to get in theory, which caused an error in the output result of the A/D converter.
The solution: to correct that difference by adding a voltage source (0.3V) to the system.

*When I added the voltage source, we did not know how to add a voltage source to the middle of the circuit in the lab.
The solution: for this was to make a voltage divider that we calculated...

*When we connected in the simulation the two voltage outputs from the switches to the input of the A/D together, we discovered that the voltage from the negative switch
