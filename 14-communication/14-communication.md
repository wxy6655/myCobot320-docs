# Communication and Message Commands

> **Notice:** Before the direct communication via communication protocol, `Transponder` is required to be burnt on M5Stack-basic, and the latest atomMain on Atom.



## 1 Settings of USB Communication

***Make sure the following settings are prepared:***

* Interface of mainline: USB Type-C connection
* Port ratio: 11520
* Data bit: 8
* Parity bit: none
* Stop bit: 1



## 2 Introduction to Command Frame & Sole Instruction

The main PC transmit data via M5Stakc-basic to peripheral PC. The peripheral PC decodes the data like commands with return values and then send the results back to the main PC within 500ms.



## 3 Formats of Message Commands' Sending and Receiving

Both sending and receiving should be represented in hexadecimal. Each command should contain 5 parts as shown below. Part 3 and 4 can be left a blank.

* 1 Pin of command: 0xFE 0xFE
  * Invariable
  * Indispensable
* 2 Effective lengthen:
  * Aggregated length including pin, serial number, functional codes and end
  * Indispensable
* 3 Serial number: 00 ~ 8F
  * Corresponding number of developed commands 
  * You may leave it blank.
* 4 Functional codes:
  * Purpose-oriented
  * You may leave it blank.
* 5 End: 0XFA
  * Invariable
  * Indispensable



## 4 Explanation for Commands

The main PC transmit data via M5Stakc-basic to peripheral PC. The peripheral PC decodes the data like commands with return values and then send the results back to the main PC within 500ms.

| Type          | Data               | Length | Function                                               |
| ------------- | ------------------ | ------ | ------------------------------------------------------ |
| Command Frame | start bit: 0       | 1      | Head frame identification, 0XFE                        |
|               | start bit: 1       | 1      | Head frame identification, 0XFE                        |
|               | bit of data length | 1      | Different commands correspond to different data length |
|               | command bit        | 1      | depending on different commands                        |
| Command Frame | data               | 0-16   | commands with data, depending on different commands    |
| End Frame     | end bit            | 1      | stop bit, 0XFA                                         |



## 5 Explanation for Sole-Instruction Commands

### Read master version

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X02 |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 02 FA

Return value: data structure


| Domain  | Explanation                        | Data      |
| ------- | ---------------------------------- | --------- |
| Data[0] | return value: identification frame | 0XFE      |
| Data[1] | return value: identification frame | 0XFE      |
| Data[2] | return value: data-length frame    | 0X03      |
| Data[3] | return value: command frame        | 0X02      |
| Data[4] | version number                    | version numbe |
| Data[5] | end frame                          | 0XFA      |

Example:

For example, the version is 1.4:

port return: FE FE 03 02 0E FA

******

### Powering up

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X10 |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 10 FA

no return value

******

### Power Decreasing and Connection Breaking up

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X11 |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 11 FA

no return value

******

### Checking the Status of Atom

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X12 |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 12 FA

Return value: data structure



| Domain  | Explanation                        | Data      |
| ------- | ---------------------------------- | --------- |
| Data[0] | return value: identification frame | 0XFE      |
| Data[1] | return value: identification frame | 0XFE      |
| Data[2] | return value: data-length frame    | 0X03      |
| Data[3] | return value: command frame        | 0X12      |
| Data[4] | power on/off                       | 0X01/0X00 |
| Data[5] | end frame                          | 0XFA      |

Example:

Given that Atom is powered on:

port return: FE FE 03 12 01 FA

******

### Power Decreasing

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X13 |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 13 FA

no return value

### Robot System Checking: Normal

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X14 |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 14 FA

Return value: data structure



| Domain  | Explanation                        | Data      |
| ------- | ---------------------------------- | --------- |
| Data[0] | return value: identification frame | 0XFE      |
| Data[1] | return value: identification frame | 0XFE      |
| Data[2] | return value: data-length frame    | 0X03      |
| Data[3] | return value: command frame        | 0X14      |
| Data[4] | connection/connection breaking up  | 0X01/0X00 |
| Data[5] | end frame                          | 0XFA      |

Example:

Given that Atom is successfully connected:

port return: FE FE 03 14 01 FA

******
#### Robot error detection

| Domain  | Explanation       |Data |
|---------|------------|------|
| Data[0] | identification frame     | 0XFE |
| Data[1] | identification frame     | 0XFE |
| Data[2] | data-length frame | 0X02 |
| Data[3] | command frame     | 0X15 |
| Data[4] | end frame     | 0XFA |
Example:

   Port transmission：FE FE 02 15 FA

   Return value: data structure

| Domain   | Explanation           |  Data         |
|----------|-----------------|--------------|
| Data[0]  | return value: identification frame  | 0XFE |
| Data[1]  | return value: identification frame  | 0XFE |
| Data[2]  | return value: data-length frame  | 0X09    |
| Data[3]  | return value: command frame   | 0X15         |
| Data[4]  | Joint 1 status      | status_code  |
| Data[5]  | Joint 2 status     | status_code  |
| Data[6]  | Joint 3 status     | status_code |
| Data[7]  | Joint 4 status     | status_code  |
| Data[8]  | Joint 5 status      | status_code |
| Data[9]  | Joint 6 status      | status_code |
| Data[9]  | Terminal state    | status_code |
| Data[10] | end frame           | 0XFA         |
The status code returned by each joint and the status code returned by the end means something:

status code：0--Nothing unusual；
1--Emergency stop is pressed；

Below is the status code for the communication problem:

16-31：Communication problems：
16-22：Communication is lost，j1：16，j2:17；
23-28：Unstable communication；

Below is the status code of the servo problem:

32-47：Over/under voltage
48-63：Magnetic coding anomaly
64-79：Overheating of temperature
80-95：Current overcurrent
96-111：Overload of load

******
### Command Updating Mode (Interpolation Setting/Motion Mode Updating)

| Domain  | Explanation                       | Data      |
| ------- | --------------------------------- | --------- |
| Data[0] | identification frame              | 0XFE      |
| Data[1] | identification frame              | 0XFE      |
| Data[2] | data-length frame                 | 0X03      |
| Data[3] | command frame                     | 0X16      |
| Data[4] | connection/connection breaking up | 0X01/0X00 |
| Data[5] | end frame                         | 0XFA      |

Example:

1. Setting updating motion mode:

   Port transmission: FE FE 03 16 01 FA

2. Setting interpolation motion mode:

   Port transmission: FE FE 03 16 00 FA

******
#### Query how commands are updated

| Domain  | Explanation       |Data |
|---------|------------|------|
| Data[0] | identification frame     | 0XFE |
| Data[1] | identification frame     | 0XFE |
| Data[2] | data-length frame        | 0X02 |
| Data[3] | command frame            | 0X17 |
| Data[4] | end frame                | 0XFA |
Example:

   Port transmission：FE FE 02 17 FA

   Return value: data structure

| Domain   | Explanation           |  Data         |
|----------|-----------------|--------------|
| Data[0]  | return value: identification frame  | 0XFE |
| Data[1]  | return value: identification frame  | 0XFE |
| Data[2]  | return value: data-length frame  | 0X09    |
| Data[3]  | return value: command frame   | 0X15         |
| Data[4]  | Interpolation mode/Refresh mode| 0X00/0X01  |
| Data[5] | end frame           | 0XFA         |

Example:

Assume that the current motion mode is refreshed:

port return: FE FE 03 17 01 FA

******
### Free Mode (Versions after 1.4 apply)

| Domain  | Explanation          | Data  |
| ------- | -------------------- | ----- |
| Data[0] | identification frame | 0XFE  |
| Data[1] | identification frame | 0XFE  |
| Data[2] | data-length frame    | 0X03  |
| Data[3] | command frame        | 0X1A  |
| Data[4] | turn on/off          | 01/00 |
| Data[5] | end frame            | 0XFA  |

Example:

Setting free motion mode:

Port transmission: FE FE 03 1A 01 FA

### Checking whether free mode is set

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X1B |
| Data[5] | end frame            | 0XFA |

Example:

Port return: FE FE 02 1B FA

Return value: data structure



| Domain  | Explanation                        | Data      |
| ------- | ---------------------------------- | --------- |
| Data[0] | return value: identification frame | 0XFE      |
| Data[1] | return value: identification frame | 0XFE      |
| Data[2] | return value: data-length frame    | 0X03      |
| Data[3] | command frame                      | 0X1B      |
| Data[4] | turn on/off                        | 0X01/0X00 |
| Data[5] | end frame                          | 0XFA      |

Example:

Given that Atom is in free mode:

port return: FE FE 03 1B 01 FA

******

### Reading Angles (blocking information)

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X20 |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 20 FA

Return value: data structure



| Domain   | Explanation                      | Data        |
| -------- | -------------------------------- | ----------- |
| Data[0]  | return value: head frame         | 0XFE        |
| Data[1]  | return value: head frame         | 0XFE        |
| Data[2]  | return value: data-length frame  | 0X0E        |
| Data[3]  | return value: command frame      | 0X20        |
| Data[4]  | high angle of No.1 steering gear | Angle1_high |
| Data[5]  | low angle of No.1 steering gear  | Angle1_low  |
| Data[6]  | high angle of No.2 steering gear | Angle2_high |
| Data[7]  | low angle of No.2 steering gear  | Angle2_low  |
| Data[8]  | high angle of No.3 steering gear | Angle3_high |
| Data[9]  | low angle of No.3 steering gear  | Angle3_low  |
| Data[10] | high angle of No.4 steering gear | Angle4_high |
| Data[11] | low angle of No.4 steering gear  | Angle4_low  |
| Data[12] | high angle of No.5 steering gear | Angle5_high |
| Data[13] | low angle of No.5 steering gear  | Angle5_low  |
| Data[14] | high angle of No.6 steering gear | Angle6_high |
| Data[15] | low angle of No.6 steering gear  | Angle6_low  |
| Data[16] | end frame                        | 0XFA        |

Example:

Return value of port: FE FE 0E 20 00 8C 00 3D FF E6 FF 3F 00 AF FF 51 FA

How to get the angle of joint 1:

temp = angle1_low+angle1_high*256

Angle1=（temp \ 33000 ?(temp – 65536) : temp）/100

*Explanation: if temp is greater than 33000, temp minus 65536, and then is divided by 100; if temp is less than 33000, then temp is divided by 100 directly.*

*Other joint angles are counted in a similar way.*



### Sending Sole Angle 

| Domain  | Explanation                    | Data       |
| ------- | ------------------------------ | ---------- |
| Data[0] | identification frame           | 0XFE       |
| Data[1] | identification frame           | 0XFE       |
| Data[2] | data-length frame              | 0X06       |
| Data[3] | command frame                  | 0X21       |
| Data[4] | serial number of steering gear | joint_no   |
| Data[5] | high angle                     | angle_high |
| Data[6] | low angle                      | angle_low  |
| Data[7] | specified speed                | sp         |
| Data[8] | end frame                      | 0XFA       |

Example:

Let the No.1 steering gear move to zero position

Port transmission: FE FE 06 21 01 00 00 14 FA

joint number: 1-6

angle_high: byte

counting method: angle value is multiplied by 100 and converted into integral form to get the hexadecimal highbyte

angle_low: byte

counting method: angle value is multiplied by 100 and converted into integral form to get the hexadecimal lowbyte

no return value

******

### Sending Entire Angles

| Domain   | Explanation                      | Data        |
| -------- | -------------------------------- | ----------- |
| Data[0]  | identification frame             | 0XFE        |
| Data[1]  | identification frame             | 0XFE        |
| Data[2]  | data-length frame                | 0X0F        |
| Data[3]  | command frame                    | 0X22        |
| Data[4]  | high angle of No.1 steering gear | Angle1_high |
| Data[5]  | low angle of No.1 steering gear  | Angle1_low  |
| Data[6]  | high angle of No.2 steering gear | Angle2_high |
| Data[7]  | low angle of No.2 steering gear  | Angle2_low  |
| Data[8]  | high angle of No.3 steering gear | Angle3_high |
| Data[9]  | low angle of No.3 steering gear  | Angle3_low  |
| Data[10] | high angle of No.4 steering gear | Angle4_high |
| Data[11] | low angle of No.4 steering gear  | Angle4_low  |
| Data[12] | high angle of No.5 steering gear | Angle5_high |
| Data[13] | low angle of No.5 steering gear  | Angle5_low  |
| Data[14] | high angle of No.6 steering gear | Angle6_high |
| Data[15] | low angle of No.6 steering gear  | Angle6_low  |
| Data[16] | specified speed                  | Sp          |
| Data[17] | end frame                        | 0XFA        |

Example:

Send 0 angle to entire joint, and let the steering gear move to zero position.

Port transmission: FE FE 0F 22 00 00 00 00 00 00 00 00 00 00 00 00 1E FA

angle1_high: byte

counting method: angle value is multiplied by 100 and converted into integral form to get the hexadecimal highbyte

angle1_low: byte

counting method: angle value is multiplied by 100 and converted into integral form to get the hexadecimal lowbyte

*Other angles are counted in a similar way.*

no return value

******

### Reading the Entire Coordinates

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X23 |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 23 FA

Return value: data structure

| Domain   | Explanation                         | Data        |
| -------- | ----------------------------------- | ----------- |
| Data[0]  | return value: head frame            | 0XFE        |
| Data[1]  | return value: head frame            | 0XFE        |
| Data[2]  | return value: data-length frame     | 0X0E        |
| Data[3]  | return value: command frame         | 0X23        |
| Data[4]  | specify high status of x coordinate | x_high      |
| Data[5]  | specify low status of x coordinate  | x_low       |
| Data[6]  | specify high status of y coordinate | y_high      |
| Data[7]  | specify high status of z coordinate | z_high      |
| Data[8]  | specify low status of z coordinate  | z_low       |
| Data[9]  | low angle of No.3 steering gear     | Angle3_low  |
| Data[10] | high angle of No.4 steering gear    | Angle4_high |
| Data[11] | low angle of No.4 steering gear     | Angle4_low  |
| Data[12] | high angle of No.5 steering gear    | Angle5_high |
| Data[13] | low angle of No.5 steering gear     | Angle5_low  |
| Data[14] | high angle of No.6 steering gear    | Angle6_high |
| Data[15] | low angle of No.6 steering gear     | Angle6_low  |
| Data[16] | end frame                           | 0XFA        |

Example:

Port return value: FE FE 0E 23 01 BC FD A0 10 15 DC 66 FF 54 DE 21 FA

How to get x coordinate:

temp = x_low + x_high*256

x coordinate =（temp\33000 ?(temp – 65536) : temp)/10

*Explanation: if temp is greater than 33000, temp minus 65536, and then is divided by 100; if temp is less than 33000, then temp is divided by 100 directly.*

*y coordinate is counted in a similar way.*

How to get rx coordinate:

temp = rx_low + rx_high*256

x coordinate =（temp \ 33000 ?(temp – 65536) : temp）/100

*Explanation: if temp is greater than 33000, temp minus 65536, and then is divided by 100; if temp is less than 33000, then temp is divided by 100 directly.*

*y coordinate is counted in a similar way.*

******

### Sending Parameters of Sole Coordinate

| Domain  | Explanation                                         | Data             |
| ------- | --------------------------------------------------- | ---------------- |
| Data[0] | identification frame                                | 0XFE             |
| Data[1] | identification frame                                | 0XFE             |
| Data[2] | data-length frame                                   | 0X06             |
| Data[3] | command frame                                       | 0X24             |
| Data[4] | axis                                                | x/y/z/rx/ry/rz   |
| Data[5] | specify high status of the parameters of xyz/rxryrz | xyz/ rxryrz_high |
| Data[6] | specify low status of the parameters of xyz/rxryrz  | xyz/rxryrz_low   |
| Data[7] | specified speed                                     | Sp               |
| Data[8] | end frame                                           | 0XFA             |

Example:

Given that the x coordinate is 200 and target speed is 20,

port transmission: FE FE 06 24 01 07 D0 14 FA

specified axis coordinate: byte

range: 1-6

data type of xyz_high: byte

counting method: value of x/y/z coordinate is multiplied by 100 and converted into integral form to get the hexadecimal highbyte

data type of xyz_low: byte

counting method: value of x/y/z coordinate is multiplied by 100 and converted into integral form to get the hexadecimal lowbyte

data type of rxryrz_high: byte

counting method: value of rx/ry/rz coordinate is multiplied by 100 and converted into integral form to get the hexadecimal highbyte

data type of rxryrz_low: byte

counting method: value of rx/ry/rz coordinate is multiplied by 100 and converted into integral form to get the hexadecimal lowbyte

no Return Value

******

### Sending Parameters of Entire Coordinate

| Domain   | Explanation                          | Data    |
| -------- | ------------------------------------ | ------- |
| Data[0]  | identification frame                 | 0XFE    |
| Data[1]  | identification frame                 | 0XFE    |
| Data[2]  | data-length frame                    | 0X10    |
| Data[3]  | command frame                        | 0X25    |
| Data[4]  | specify high status of x coordinate  | x_high  |
| Data[5]  | specify low status of x coordinate   | x_low   |
| Data[6]  | specify high status of y coordinate  | y_high  |
| Data[7]  | specify low status of y coordinate   | y_low   |
| Data[8]  | specify high status of z coordinate  | z_high  |
| Data[9]  | specify low status of z coordinate   | z_low   |
| Data[10] | specify high status of rx coordinate | rx_high |
| Data[11] | specify low status of rx coordinate  | rx_low  |
| Data[12] | specify high status of ry coordinate | ry_high |
| Data[13] | specify low status of ry coordinate  | ry_low  |
| Data[14] | specify high status of rz coordinate | rz_high |
| Data[15] | specify low status of rz coordinate  | rz_low  |
| Data[16] | specifyd speed                       | Sp      |
| Data[17] | mode                                 | 0X01    |
| Data[17] | end frame                            | 0XFA    |

Example:

Given that the coordinate of the end of robotic arm is （150.3，-68.7，101.8，10.18，0，-90）, and speed is 10,

port transmission: FE FE 10 25 05 DF FD 51 03 FA BC 30 00 00 DC D8 0A 01 FA

data type of x_high: byte

counting method: value of x coordinate is multiplied by 10 and get the hexadecimal highbyte

data type of x_low: byte

counting method: value of x coordinate is multiplied by 10 and get the hexadecimal highbyte

*y coordinate is counted in a similar way.*

data type of rx_high: byte

counting method: value of rx coordinate is multiplied by 10 and get the hexadecimal highbyte

data type of rx_low: byte

counting method: value of rx coordinate is multiplied by 100 and get the hexadecimal highbyte

*ry coordinate is counted in a similar way.*

no return value

******
### Program halt

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X26 |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 26 FA

no return value

******

### Judging Whether Program Stops

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X27 |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 27 FA

Return value: data structure

| Domain  | Explanation          | Data      |
| ------- | -------------------- | --------- |
| Data[0] | identification frame | 0XFE      |
| Data[1] | identification frame | 0XFE      |
| Data[2] | data-length frame    | 0X02      |
| Data[3] | command frame        | 0X27      |
| Data[4] | suspend/not suspend  | 0X01/0X00 |
| Data[5] | end frame            | 0XFA      |

Example:

Given that program suspends,

port return value: FE FE 03 12 01 FA

******

### Program Restoration

| Domain  | Explanation          | Data      |
| ------- | -------------------- | --------- |
| Data[0] | identification frame | 0XFE      |
| Data[1] | identification frame | 0XFE      |
| Data[2] | data-length frame    | 0X02      |
| Data[3] | command frame        | 0X28      |
| Data[4] | end frame            | 0XFA      |

Example:

Port transmission: FE FE 02 28 FA

no return value

******

### Program Stops

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X29 |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 29 FA

no return value

### Whether Reaching Specified Coordinate

| Domain   | Explanation                                                  | Data                |
| -------- | ------------------------------------------------------------ | ------------------- |
| Data[0]  | identification frame                                         | 0XFE                |
| Data[1]  | identification frame                                         | 0XFE                |
| Data[2]  | data-length frame                                            | 0X0E/0X0F           |
| Data[3]  | command frame                                                | 0X2A                |
| Data[4]  | high status of x coordinate/highbyte of angle of No.1 steering gear | x_high/Angle1_high  |
| Data[5]  | low status of x coordinate/lowbyte of angle of No.1 steering gear | x_low/Angle1_low    |
| Data[6]  | high status of y coordinate/highbyte of angle of No.2 steering gear | y_high/Angle2_high  |
| Data[7]  | low status of y coordinate/lowbyte of angle of No.2 steering gear | y_low/Angle2_low    |
| Data[8]  | high status of y coordinate/highbyte of angle of No.3 steering gear | z_high/Angle3_high  |
| Data[9]  | low status of z coordinate/lowbyte of angle of No.3 steering gear | z_low/Angle3_low    |
| Data[10] | high status of rx coordinate/highbyte of angle of No.4 steering gear | rx_high/Angle4_high |
| Data[11] | low status of rx coordinate/lowbyte of angle of No.4 steering gear | rx_low/Angle4_low   |
| Data[12] | high status of ry coordinate/highbyte of angle of No.5 steering gear | ry_high/Angle5_high |
| Data[13] | low status of ry coordinate/lowbyte of angle of No.5 steering gear | ry_low/Angle5_low   |
| Data[14] | high status of rz coordinate/highbyte of angle of No.6 steering gear | rz_high/Angle6_high |
| Data[15] | low status of rz coordinate/lowbyte of angle of No.6 steering gear | rz_low/Angle6_low   |
| Data[16] | coordinate/angle                                             | 0X01/0X00           |
| Data[17] | end frame                                                    | 0XFA                |

Example:

Judging whether robotic arm move to zero position:

port transmission: FE FE 0F 2A 00 00 00 00 00 00 00 00 00 00 00 00 00 FA

data type of x_high: byte

counting method: value of x coordinate is multiplied by 10 and converted into integral form to get the hexadecimal highbyte

data type of x_low: byte

counting method: value of x coordinate is multiplied by 10 and converted into integral form to get the hexadecimal lowbyte

*y coordinate is counted in a similar way.*

data type of rx_high: byte

counting method: value of rx coordinate is multiplied by 100 and converted into integral form to get the hexadecimal highbyte

data type of rx_low: byte

counting method: value of rx coordinate is multiplied by 100 and converted into integral form to get the hexadecimal lowbyte

*ry coordinate is counted in a similar way.*

data type of angle_high: byte

counting method: value angle is multiplied by 100 and converted into integral form to get the hexadecimal highbyte

data type of angle_low: byte

counting method: value of angle is multiplied by 100 and converted into integral form to get the hexadecimal lowbyte

*Type: byte (temporarily unavailable)*

Return Value: data structure

| Domain  | Explanation                               | Data      |
| ------- | ----------------------------------------- | --------- |
| Data[0] | return value: head frame                  | 0XFE      |
| Data[1] | return value: head frame                  | 0XFE      |
| Data[2] | return value: data-length frame           | 0X03      |
| Data[3] | return value: command frame               | 0X2a      |
| Data[4] | reaching the point/not reaching the point | 0X01/0X00 |
| Data[5] | end frame                                 | 0XFA      |

Example:

Given that the robotic arm doesn't reach the point:

port return value: FE FE 03 2A 00 FA

******

### Movement Check

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X2B |
| Data[4] | end frame            | 0XFA |

Example:

port transmission: FE FE 02 2B FA

Return Value: data structure

| Domain  | Explanation                     | Data      |
| ------- | ------------------------------- | --------- |
| Data[0] | return value: head frame        | 0XFE      |
| Data[1] | return value: head frame        | 0XFE      |
| Data[2] | return value: data-length frame | 0X03      |
| Data[3] | return value: command frame     | 0X2B      |
| Data[4] | in motion/not in motion         | 0X01/0X00 |
| Data[5] | end frame                       | 0XFA      |

Example:

Given that the program is running:

port return value: FE FE 03 2B 01 FA

******

### jog-Joint-Oriented Movement

| Domain  | Explanation                    | Data      |
| ------- | ------------------------------ | --------- |
| Data[0] | identification frame           | 0XFE      |
| Data[1] | identification frame           | 0XFE      |
| Data[2] | data-length frame              | 0X05      |
| Data[3] | command frame                  | 0X30      |
| Data[4] | serial number of steering gear | Joint     |
| Data[5] | direction of steering gear     | direction |
| Data[6] | specified speed                | sp        |
| Data[7] | end frame                      | 0XFA      |

Example:

Given that NO.1 steering gear is revolving clockwise at the speed of 20%:

port return value: FE FE 05 30 01 01 14 FA

range of serial number of joint: 1-6

di: data type of byte, either 0 or 1

sp: data type of byte, ranging from 0 to 100

no return value.

******

### jod-Absolute Control(Versions after 1.4 appl)

| Domain  | Explanation                        | Data       |
| ------- | ---------------------------------- | ---------- |
| Data[0] | identification frame               | 0XFE       |
| Data[1] | identification frame               | 0XFE       |
| Data[2] | data-length frame                  | 0X06       |
| Data[3] | command frame                      | 0X31       |
| Data[4] | serial number of steering gear     | Joint      |
| Data[5] | highbyte of angle of steering gear | Angle_high |
| Data[6] | lowbyte of angle of steering gear  | Angle_low  |
| Data[7] | specified speed                    | sp         |
| Data[8] | end frame                          | 0XFA       |

Example:

Given that No.1 steering gear moves to 45° at the speed of 20

port transmission: FE FE 06 31 01 11 94 14 FA

range of serial number of joint: 1-6

data type of Angle_high: byte

counting method: value of Angle_high is multiplied by 100 and converted into integral form to get the hexadecimal highbyte

data type of Angle_low: byte

counting method: value of Angle_low is multiplied by 100 and converted into integral form to get the hexadecimal lowbyte

sp: data type of byte, ranging from 0 to 100

no return value

### jog-Coordinate-Oriented Movement

| Domain  | Explanation                | Data |
| ------- | -------------------------- | ---- |
| Data[0] | identification frame       | 0XFE |
| Data[1] | identification frame       | 0XFE |
| Data[2] | data-length frame          | 0X05 |
| Data[3] | command frame              | 0X32 |
| Data[4] | specified coordinate       | axis |
| Data[5] | direction of steering gear | di   |
| Data[6] | specified speed            | sp   |
| Data[7] | end frame                  | 0XFA |

Example:

Given that robotic arm moves towards x coordinate at the speed of 20

port transmission: FE FE 06 32 01 01 14 FA

axis ranges from 1 to 6, representing x, y, z,rx, ry, rz

di: data type of byte, either 0 or 1

sp: data type of byte, ranging from 0 to 100

no return value

******

### jog-Stop(Versions after 1.4 appl)

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X34 |
| Data[4] | end frame            | 0XFA |

Example:

Stop moving

port transmission: FE FE 02 34 FA

no return value

******

### Sending Potential

| Domain  | Explanation                    | Data         |
| ------- | ------------------------------ | ------------ |
| Data[0] | identification frame           | 0XFE         |
| Data[1] | identification frame           | 0XFE         |
| Data[2] | data-length frame              | 0X05         |
| Data[3] | command frame                  | 0X3A         |
| Data[4] | serial number of steering gear | Joint        |
| Data[5] | high status of potential       | Encoder_high |
| Data[6] | low status of potential        | Encoder_low  |
| Data[7] | end frame                      | 0XFA         |

Example:

Given that No.5 joint is set to 2048

port transmission: FE FE 05 3A 05 08 00 FA

serial number of joint ranges from 0 to 5

data type of Joint: byte

data type of Encoder_high: byte

counting method: get the high status of potential (in hexadecimal form)

data type of Encoder_low: byte

counting method: get the low status of potential (in hexadecimal form)

no return value

******

### Get the Potential

| Domain  | Explanation            | Data  |
| ------- | ---------------------- | ----- |
| Data[0] | identification frame   | 0XFE  |
| Data[1] | identification frame   | 0XFE  |
| Data[2] | data-length frame      | 0X03  |
| Data[3] | command frame          | 0X3B  |
| Data[4] | serial number of joint | joint |
| Data[5] | end frame              | 0XFA  |

Example:

get the potential of NO.2 steering gear

serial number of joint ranges from 1 to 6

Return Value: data structure

| Domain  | Explanation                        | Data         |
| ------- | ---------------------------------- | ------------ |
| Data[0] | return value: identification frame | 0XFE         |
| Data[1] | return value: identification frame | 0XFE         |
| Data[2] | return value: data-length frame    | 0X04         |
| Data[3] | return value: command frame        | 0X3B         |
| Data[4] | high status of potential           | Encoder_high |
| Data[5] | low status of potential            | Encoders_low |
| Data[6] | end frame                          | 0XFA         |

Example:

port transmission: FE FE 04 3B 08 07 FA

How potentials are counted:

potential = low status of potential + high status of potential * 256

******

### Sending Potential of Six Steering Gears

| Domain   | Explanation                    | Data           |
| -------- | ------------------------------ | -------------- |
| Data[0]  | identification frame           | 0XFE           |
| Data[1]  | identification frame           | 0XFE           |
| Data[2]  | data-length frame              | 0X0E/0X0F      |
| Data[3]  | command frame                  | 0X3C           |
| Data[4]  | highbyte of No.1 steering gear | encoder_1_high |
| Data[5]  | lowbyte of No.1 steering gear  | encoder_1_low  |
| Data[6]  | highbyte of No.2 steering gear | encoder_2_high |
| Data[7]  | lowbyte of No.2 steering gear  | encoder_2_low  |
| Data[8]  | highbyte of No.3 steering gear | encoder_3_high |
| Data[9]  | lowbyte of No.3 steering gear  | encoder_3_low  |
| Data[10] | highbyte of No.4 steering gear | encoder_4_high |
| Data[11] | lowbyte of No.4 steering gear  | encoder_4_low  |
| Data[12] | highbyte of No.5 steering gear | encoder_5_high |
| Data[13] | lowbyte of No.5 steering gear  | encoder_5_low  |
| Data[14] | highbyte of No.6 steering gear | encoder_6_high |
| Data[15] | lowbyte of No.6 steering gear  | encoder_6_low  |
| Data[16] | specified speed                | Sp             |
| Data[17] | end frame                      | 0XFA           |

Example:

Given that the potential of all is 2048, and the speed is 20

port transmission: FE FE 0F 3C 08 00 08 00 08 00 08 00 08 00 08 00 14 FA

*Refer to the chart above for each potential.*

data type of encoder_1_high: byte

counting method: convert the potential of No.1 steering gear into integral form and get the highbyte hexadecimal

data type of encoder_1_low: byte

counting method: convert the potential of No.1 steering gear into integral form and get the lowbyte hexadecimal

sp: data type of byte, ranging from 0 to 100

no return value

******

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X3D |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 3D FA

Return value: data structure

| Domain   | Explanation                    | Data           |
| -------- | ------------------------------ | -------------- |
| Data[0]  | identification frame           | 0XFE           |
| Data[1]  | identification frame           | 0XFE           |
| Data[2]  | data-length frame              | 0X0E           |
| Data[3]  | command frame                  | 0X3D           |
| Data[4]  | highbyte of No.1 steering gear | encoder_1_high |
| Data[5]  | lowbyte of No.1 steering gear  | encoder_1_low  |
| Data[6]  | highbyte of No.2 steering gear | encoder_2_high |
| Data[7]  | lowbyte of No.2 steering gear  | encoder_2_low  |
| Data[8]  | highbyte of No.3 steering gear | encoder_3_high |
| Data[9]  | lowbyte of No.3 steering gear  | encoder_3_low  |
| Data[10] | highbyte of No.4 steering gear | encoder_4_high |
| Data[11] | lowbyte of No.4 steering gear  | encoder_4_low  |
| Data[12] | highbyte of No.5 steering gear | encoder_5_high |
| Data[13] | lowbyte of No.5 steering gear  | encoder_5_low  |
| Data[14] | highbyte of No.6 steering gear | encoder_6_high |
| Data[15] | lowbyte of No.6 steering gear  | encoder_6_low  |
| Data[17] | end frame                      | 0XFA           |

Example:

Given that all robotic arms are set in zero position,

port return value: FE FE 0E 3D 08 00 08 00 08 00 08 00 08 00 08 00 FA

How to count potential

How potentials are counted:

potential = low status of potential + high status of potential * 256

******

### Reading Potential of Six Steering Gears

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X3D |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 3D FA

Return value: data structure

| Domain   | Explanation                    | Data           |
| -------- | ------------------------------ | -------------- |
| Data[0]  | identification frame           | 0XFE           |
| Data[1]  | identification frame           | 0XFE           |
| Data[2]  | data-length frame              | 0X0E           |
| Data[3]  | command frame                  | 0X3D           |
| Data[4]  | highbyte of No.1 steering gear | encoder_1_high |
| Data[5]  | lowbyte of No.1 steering gear  | encoder_1_low  |
| Data[6]  | highbyte of No.2 steering gear | encoder_2_high |
| Data[7]  | lowbyte of No.2 steering gear  | encoder_2_low  |
| Data[8]  | highbyte of No.3 steering gear | encoder_3_high |
| Data[9]  | lowbyte of No.3 steering gear  | encoder_3_low  |
| Data[10] | highbyte of No.4 steering gear | encoder_4_high |
| Data[11] | lowbyte of No.4 steering gear  | encoder_4_low  |
| Data[12] | highbyte of No.5 steering gear | encoder_5_high |
| Data[13] | lowbyte of No.5 steering gear  | encoder_5_low  |
| Data[14] | highbyte of No.6 steering gear | encoder_6_high |
| Data[15] | lowbyte of No.6 steering gear  | encoder_6_low  |
| Data[17] | end frame                      | 0XFA           |

Example:

Given that all robotic arms are set in zero position,

port return value: FE FE 0E 3D 08 00 08 00 08 00 08 00 08 00 08 00 FA

How potentials are counted:

potential = low status of potential + high status of potential * 256

******

### Reading the Smallest Angle

| Domain  | Explanation                    | Data         |
| ------- | ------------------------------ | ------------ |
| Data[0] | identification frame           | 0XFE         |
| Data[1] | identification frame           | 0XFE         |
| Data[2] | data-length frame              | 0X03         |
| Data[3] | command frame                  | 0X4A         |
| Data[4] | serial number of steering gear | Joint_number |
| Data[5] | end frame                      | 0XFA         |

Serial number of joint ranges from 1 to 6

Example:

Reading the largest angle of joint 2

Port transmission: FE FE 03 4A 02 FA

Return value: data structure

| Domain  | Explanation                        | Data         |
| ------- | ---------------------------------- | ------------ |
| Data[0] | return value: identification frame | 0XFE         |
| Data[1] | return value: identification frame | 0XFE         |
| Data[2] | return value: data-length frame    | 0X04         |
| Data[3] | return value: command frame        | 0X4A         |
| Data[4] | serial number of joint             | Joint_number |
| Data[5] | high angle of steering gear        | Angle_high   |
| Data[6] | low angle of steering gear         | Angle_low    |
| Data[7] | end frame                          | 0XFA         |

Example:

port return value: FE FE 05 4A 02 06 72 FA

How to calculate the minimum Angle:

temp = angle1_low+angle1_high*256

Angle1=（temp \ 33000 ?(temp – 65536) : temp）/10

*Explanation: if temp is greater than 33000, temp minus 65536, and then is divided by 10; if temp is less than 33000, then temp is divided by 10 directly.*

******

### Reading the maximum Angle

| Domain  | Explanation                    | Data         |
| ------- | ------------------------------ | ------------ |
| Data[0] | identification frame           | 0XFE         |
| Data[1] | identification frame           | 0XFE         |
| Data[2] | data-length frame              | 0X03         |
| Data[3] | command frame                  | 0X4B         |
| Data[4] | serial number of steering gear | Joint_number |
| Data[5] | end frame                      | 0XFA         |

Serial number of joint ranges from 1 to 6

Example:

Reading the largest angle of joint 2

Port transmission: FE FE 03 4B 02 FA

Return value: data structure

| Domain  | Explanation                        | Data         |
| ------- | ---------------------------------- | ------------ |
| Data[0] | return value: identification frame | 0XFE         |
| Data[1] | return value: identification frame | 0XFE         |
| Data[2] | return value: data-length frame    | 0X04         |
| Data[3] | return value: command frame        | 0X4A         |
| Data[4] | serial number of joint             | Joint_number |
| Data[5] | high angle of steering gear        | Angle_high   |
| Data[6] | low angle of steering gear         | Angle_low    |
| Data[7] | end frame                          | 0XFA         |

Example:

port return value: FE FE 05 4A 02 06 72 FA

How the largest angles are counted:

temp = angle1_low+angle1_high*256

Angle1=（temp \ 33000 ?(temp – 65536) : temp）/10

*Explanation: if temp is greater than 33000, temp minus 65536, and then is divided by 10; if temp is less than 33000, then temp is divided by 10 directly.*

******

### Setting the Smallest Angle

| Domain  | Explanation                        | Data         |
| ------- | ---------------------------------- | ------------ |
| Data[0] | return value: identification frame | 0XFE         |
| Data[1] | return value: identification frame | 0XFE         |
| Data[2] | return value: data-length frame    | 0X05         |
| Data[3] | return value: command frame        | 0X4C         |
| Data[4] | serial number of joint             | Joint_number |
| Data[5] | high angle of steering gear        | Angle_high   |
| Data[6] | low angle of steering gear         | Angle_low    |
| Data[7] | end frame                          | 0XFA         |

Example:

Given that the smallest angle of joint 2 is 0

Serial number of joint ranges from 1 to 6

port return value: FE FE 03 41 32 FA

data type of angle1_high: byte

counting method: angleis multiplied by 100 and converted into integral form to get the hexadecimal highbyte

data type of angle1_low: byte

counting method: angleis multiplied by 100 and converted into integral form to get the hexadecimal lowbyte

Port transmission: FE FE 05 4C 02 00 00 FA

no return value

******

### Setting the Largest Angle

| Domain  | Explanation               | Data         |
| ------- | ------------------------- | ------------ |
| Data[0] | identification frame      | 0XFE         |
| Data[1] | identification frame      | 0XFE         |
| Data[2] | data-length frame         | 0X05         |
| Data[3] | command frame             | 0X4D         |
| Data[4] | serial number of joint    | Joint_number |
| Data[5] | highbyte of steering gear | Angle_high   |
| Data[6] | lowbyte of steering gear  | Angle_low    |
| Data[7] | end frame                 | 0XFA         |

Example:

Given that the largest angle of joint 2 is 45

Serial number of joint ranges from 1 to 6

port return value: FE FE 03 41 32 FA

data type of angle1_high: byte

counting method: angleis multiplied by 100 and converted into integral form to get the hexadecimal highbyte

data type of angle1_low: byte

counting method: angleis multiplied by 100 and converted into integral form to get the hexadecimal lowbyte

Port transmission: FE FE 05 4C 02 11 94 FA

no return value

******

### Checking Connection

| Domain  | Explanation                    | Data         |
| ------- | ------------------------------ | ------------ |
| Data[0] | identification frame           | 0XFE         |
| Data[1] | identification frame           | 0XFE         |
| Data[2] | data-length frame              | 0X03         |
| Data[3] | command frame                  | 0X50         |
| Data[4] | serial number of steering gear | Joint_number |
| Data[5] | end frame                      | 0XFA         |

Serial number of joint ranges from 1 to 6

Example:

Checking whether No.1 steering gear is connected

Port transmission: FE FE 03 50 01 FA

Return value: data structure

| Domain  | Explanation                        | Data         |
| ------- | ---------------------------------- | ------------ |
| Data[0] | return value: identification frame | 0XFE         |
| Data[1] | return value: identification frame | 0XFE         |
| Data[2] | return value: data-length frame    | 0X03         |
| Data[3] | command frame                      | 0X50         |
| Data[4] | serial number of joint             | Joint_number |
| Data[5] | connected/not connected            | 0X01/0X00    |
| Data[6] | end frame                          | 0XFA         |

Example:

No.1 steering gear is connected

Port return value: FE FE 04 50 01 01 FA

### Checking Whether All Steering Gears Are Powered On

| Domain  | Explanation          | Explanation |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X51 |
| Data[5] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 51 FA

Return value: data structure

| Domain  | Explanation                        | Data      |
| ------- | ---------------------------------- | --------- |
| Data[0] | return value: identification frame | 0XFE      |
| Data[1] | return value: identification frame | 0XFE      |
| Data[2] | return value: data-length frame    | 0X03      |
| Data[3] | return value: command frame        | 0X51      |
| Data[4] | power on/off                       | 0X01/0X00 |
| Data[5] | end frame                          | 0XFA      |

Example:

Not all steering gears are powered on

Port transmission: FE FE 03 51 01 FA

******

### Setting Servo Parameters

| Domain  | Explanation            | Data     |
| ------- | ---------------------- | -------- |
| Data[0] | identification frame   | 0XFE     |
| Data[1] | identification frame   | 0XFE     |
| Data[2] | data-length frame      | 0X05     |
| Data[3] | command frame          | 0X52     |
| Data[4] | serial number of joint | Joint_no |
| Data[5] | data address           | data_id  |
| Data[6] | end frame              | 0XFA     |

Example:

Reading ratio parameter of P position of No.1 steering gear

Port transmission: FE FE 05 52 01 15 01 FA

Serial number of joint ranges from 1 to 6

no return value

Data_id: data type byte, referring to the chart below for specific value:

| Address | Function               | Range  | Initial Value | Explanation                                           |
| ------- | ---------------------- | ------ | ------------- | ----------------------------------------------------- |
| 20      | LED siren              | 0-254  | 0             | 1\0: turn on/off LED siren                            |
| 21      | position loop P        | 0-254  | 10            | control the ratio coefficient                         |
| 22      | position loop I        | 0-254  | 0             | control the differential coefficient                  |
| 23      | position loop D        | 0-254  | 1             | control the integral-action coefficient               |
| 24      | minimum starting force | 0-1000 | 0             | set the smallest torque output capability 1000 = 100% |

******

### Reading Servo Parameters

| Domain  | Explanation            | Data         |
| ------- | ---------------------- | ------------ |
| Data[0] | identification frame   | 0XFE         |
| Data[1] | identification frame   | 0XFE         |
| Data[2] | data-length frame      | 0X04         |
| Data[3] | command frame          | 0X53         |
| Data[4] | serial number of joint | Joint_number |
| Data[5] | data address           | data_id      |
| Data[6] | end frame              | 0XFA         |

Example:

Reading ratio parameter of P position of No.1 steering gear

Port transmission: FE FE 03 51 01 FA

Serial number of joint ranges from 1 to 6

Data_id: data type byte, referring to the chart below for specific value:

| Address | Function               | Range  | Initial Value | Explanation                                           |
| ------- | ---------------------- | ------ | ------------- | ----------------------------------------------------- |
| 20      | LED siren              | 0-254  | 0             | 1\0: turn on/off LED siren                            |
| 21      | position loop P        | 0-254  | 10            | control the ratio coefficient                         |
| 22      | position loop I        | 0-254  | 0             | control the differential coefficient                  |
| 23      | position loop D        | 0-254  | 1             | control the integral-action coefficient               |
| 24      | minimum starting force | 0-1000 | 0             | set the smallest torque output capability 1000 = 100% |

Return value: data structure

| Domain  | Explanation                        | Data |
| ------- | ---------------------------------- | ---- |
| Data[0] | return value: identification frame | 0XFE |
| Data[1] | return value: identification frame | 0XFE |
| Data[2] | return value: data-length frame    | 0X03 |
| Data[3] | return value: command frame        | 0X53 |
| Data[4] | return value: data                 | data |
| Data[5] | end frame                          | 0XFA |



### Setting Zero Position of Steering Gear

| Domain  | Explanation            | Data     |
| ------- | ---------------------- | -------- |
| Data[0] | identification frame   | 0XFE     |
| Data[1] | identification frame   | 0XFE     |
| Data[2] | data-length frame      | 0X03     |
| Data[3] | command frame          | 0X54     |
| Data[4] | serial number of joint | Joint_no |
| Data[6] | end frame              | 0XFA     |

Example:

Reading zero position of No.1 steering gear

Port transmission: FE FE 03 54 01 FA

Serial number of joint ranges from 1 to 6

no return value

******

### Braking Single Motor

| Domain  | Explanation            | Data     |
| ------- | ---------------------- | -------- |
| Data[0] | identification frame   | 0XFE     |
| Data[1] | identification frame   | 0XFE     |
| Data[2] | data-length frame      | 0X02     |
| Data[3] | command frame          | 0X55     |
| Data[4] | serial number of joint | Joint_no |
| Data[5] | end frame              | 0XFA     |

Example:

Serial number of joint ranges from 1 to 6

Braking No.1 steering gear,

Port transmission: FE FE 03 55 01 FA

no return value

******

### Power Failure of Single Motor

| Domain  | Explanation                    | Data     |
| ------- | ------------------------------ | -------- |
| Data[0] | identification frame           | 0XFE     |
| Data[1] | identification frame           | 0XFE     |
| Data[2] | data-length frame              | 0X03     |
| Data[3] | command frame                  | 0X56     |
| Data[4] | serial number of steering gear | Servo_no |
| Data[5] | end frame                      | 0XFA     |

Example:

Let No.3 steering gear reducing power

Serial number of servo ranges from 1 to 6

no return value

******

### Powering On a Single Motor

| Domain  | Explanation                    | Data     |
| ------- | ------------------------------ | -------- |
| Data[0] | identification frame           | 0XFE     |
| Data[1] | identification frame           | 0XFE     |
| Data[2] | data-length frame              | 0X03     |
| Data[3] | command frame                  | 0X57     |
| Data[4] | serial number of steering gear | Servo_no |
| Data[5] | end frame                      | 0XFA     |

Example:

power on No.1 steering gear

Port transmission: FE FE 03 57 01 FA

Serial number of servo ranges from 1 to 6

no return value

******

### Setting Atom IO (setDigitalOutput)

| Domain  | Explanation                | Data        |
| ------- | -------------------------- | ----------- |
| Data[0] | identification frame       | 0XFE        |
| Data[1] | identification frame       | 0XFE        |
| Data[2] | data-length frame          | 0X04        |
| Data[3] | command frame              | 0X61        |
| Data[4] | serial number of pin       | pin_no      |
| Data[5] | signal of electrical level | 00X00/00X01 |
| Data[6] | end frame                  | 0XFA        |

Example:

setting atom pin23 as high electrical level 

Port transmission: FE FE 04 61 17 01 FA

no return value

******

### Reading Atom IO(getDigitalInput)

| Domain  | Explanation          | Data   |
| ------- | -------------------- | ------ |
| Data[0] | identification frame | 0XFE   |
| Data[1] | identification frame | 0XFE   |
| Data[2] | data-length frame    | 0X04   |
| Data[3] | command frame        | 0X62   |
| Data[4] | serial number of pin | pin_no |
| Data[5] | end frame            | 0XFA   |

Example:

reading signal of electrical level of pin23

Port transmission: FE FE 03 62 16 FA

Return value: data structure

| Domain  | Explanation                        | Data      |
| ------- | ---------------------------------- | --------- |
| Data[0] | return value: identification frame | 0XFE      |
| Data[1] | return value: identification frame | 0XFE      |
| Data[2] | return value: data-length frame    | 0X03      |
| Data[3] | return value: command frame        | 0X62      |
| Data[4] | pin number                         | pin_no    |
| Data[5] | signal of electrical level         | 0X00/0X01 |
| Data[6] | end frame                          | 0XFA      |

Example:

Given that pin 22 is of high electrical level 

Port return value: FE FE 04 62 16 01 FA

******

### Setting the Mode of Gripper

| Domain  | Explanation             | Data      |
| ------- | ----------------------- | --------- |
| Data[0] | identification frame    | 0XFE      |
| Data[1] | identification frame    | 0XFE      |
| Data[2] | data-length frame       | 0X04      |
| Data[3] | command frame           | 0X66      |
| Data[4] | gripper opening/closing | 0X00/0X01 |
| Data[5] | speed                   | Sp        |
| Data[6] | end frame               | 0XFA      |

Example:

making gripper open at the speed of 50

Port transmission: FE FE 04 66 00 32 FA

no return value

******

### Setting Angles of Gripper

| Domain  | Explanation                       | Data  |
| ------- | --------------------------------- | ----- |
| Data[0] | identification frame              | 0XFE  |
| Data[1] | identification frame              | 0XFE  |
| Data[2] | data-length frame                 | 0X04  |
| Data[3] | command frame                     | 0X67  |
| Data[4] | span of gripper's opening/closing | value |
| Data[5] | speed                             | Sp    |
| Data[6] | end frame                         | 0XFA  |

Example:

Given that gripper opens by 50% at the speed of 30

Port transmission: FE FE 04 67 32 14 FA

The value can be converted into hexadecimal

no return value

******

### Setting the Gripper to Zero Position

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X68 |
| Data[4] | end frame            | 0XFA |

Example:

Setting the Gripper to Zero Position

Port transmission: FE FE 02 68 FA

******

### Setting RGB of Atom

| Domain  | Explanation          | Data      |
| ------- | -------------------- | --------- |
| Data[0] | identification frame | 0XFE      |
| Data[1] | identification frame | 0XFE      |
| Data[2] | data-length frame    | 0X05      |
| Data[3] | command frame        | 0X6A      |
| Data[4] | R                    | 0X00/0XFF |
| Data[5] | G                    | 0X00/0XFF |
| Data[6] | B                    | 0X00/0XFF |
| Data[7] | end frame            | 0XFA      |

Example:

Setting RGB color as blue

Port transmission: FE FE 05 6A 00 00 FF FA

no return value

*****

#### Set electric gripper mode

| Domain  | Explanation       | Data |
|---------|------------|------|
| Data[0] | identification frame  | 0XFE |
| Data[1] | identification frame  | 0XFE |
| Data[2] | data-length frame  | 0X03 |
| Data[3] | command frame    | 0X6B |
| Data[4] | Gripper switch   | 0X00/0X01 |
| Data[5] | end frame     | 0XFA |

   Set electric gripper to open mode

Example:

   Port transmission：FE FE 03 6B 01 FA

   no return value

*****

#### Electric gripper initialization

| Domain  | Explanation       | Data |
|---------|------------|------|
| Data[0] | identification frame    | 0XFE |
| Data[1] | identification frame   | 0XFE |
| Data[2] | data-length frame | 0X02 |
| Data[3] | command frame   | 0X6C |
| Data[4] | end frame   | 0XFA |

   Note: The gripper needs to be initialized once after insertion and removal.

Example:

   Port transmission：FE FE 02 6C FA

   no return value

*****

#### Set the gripper mode

|  Domain  | Explanation       | Data |
|---------|------------|------|
| Data[0] | identification frame     | 0XFE |
| Data[1] | identification frame     | 0XFE |
| Data[2] | data-length frame | 0X03 |
| Data[3] | command frame    | 0X6D |
| Data[4] | unvarnished transmission/port | 0X00/0X01 |
| Data[5] | end frame     | 0XFA |
   
   Set to pass-through mode

   Port transmission：FE FE 03 6D 00 FA

   no return value
*****
#### Get the gripper pattern

| Domain  | Explanation       | Data |
|---------|------------|------|
| Data[0] | identification frame     | 0XFE |
| Data[1] | identification frame     | 0XFE |
| Data[2] | data-length frame  | 0X03 |
| Data[3] | command frame   | 0X6E |
| Data[4] | end frame     | 0XFA |

   Port transmission：FE FE 03 6E FA
   
   Return value: data structure

| Domain  | Explanation       | Data |
|---------|------------|------|
| Data[0] | return value: identification frame     | 0XFE |
| Data[1] | return value: identification frame     | 0XFE |
| Data[2] | return value: data-length frame  | 0X03 |
| Data[3] | return value: command frame   | 0X6E |
| Data[4] | Current mode   | 0X00/0X01 |
| Data[5] | end frame     | 0XFA |

   The pass-through mode is assumed

   Port transmission：FE FE 04 6E 01 FA

*****

### Setting Tool Coordinate

| Domain   | Explanation                          | Data    |
| -------- | ------------------------------------ | ------- |
| Data[0]  | identification frame                 | 0XFE    |
| Data[1]  | identification frame                 | 0XFE    |
| Data[2]  | data-length frame                    | 0X0E    |
| Data[3]  | command frame                        | 0X81    |
| Data[4]  | specify high status of x coordinate  | x_high  |
| Data[5]  | specify low status of x coordinate   | x_low   |
| Data[6]  | specify high status of y coordinate  | y_high  |
| Data[7]  | specify low status of y coordinate   | y_low   |
| Data[8]  | specify high status of z coordinate  | z_high  |
| Data[9]  | specify low status of z coordinate   | z_low   |
| Data[10] | specify high status of rx coordinate | rx_high |
| Data[11] | specify low status of rx coordinate  | rx_low  |
| Data[12] | specify high status of ry coordinate | ry_high |
| Data[13] | specify low status of ry coordinate  | ry_low  |
| Data[14] | specify high status of rz coordinate | rz_high |
| Data[15] | specify low status of rz coordinate  | rz_low  |
| Data[16] | end frame                            | 0XFA    |

Example:

Given that （0，0，50，0，0，0）is tool coordinate, 

Port transmission: FE FE 0E 81 00 00 00 00 13 88 00 00 00 00 00 00 FA

no return value

******

### Acquiring Tool Coordinate

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X82 |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 04 b2 1b 58 FA

Return value: data structure

| Domain   | Explanation                          | Data    |
| -------- | ------------------------------------ | ------- |
| Data[0]  | return value: identification frame   | 0XFE    |
| Data[1]  | return value: identification frame   | 0XFE    |
| Data[2]  | return value: data-length frame      | 0X0E    |
| Data[3]  | return value: command frame      | 0X82    |
| Data[4]  | specify high status of x coordinate  | x_high  |
| Data[5]  | specify low status of x coordinate   | x_low   |
| Data[6]  | specify high status of y coordinate  | y_high  |
| Data[7]  | specify low status of y coordinate   | y_low   |
| Data[8]  | specify high status of z coordinate  | z_high  |
| Data[9]  | specify low status of z coordinate   | z_low   |
| Data[10] | specify high status of rx coordinate | rx_high |
| Data[11] | specify low status of rx coordinate  | rx_low  |
| Data[12] | specify high status of ry coordinate | ry_high |
| Data[13] | specify low status of ry coordinate  | ry_low  |
| Data[14] | specify high status of rz coordinate | rz_high |
| Data[15] | specify low status of rz coordinate  | rz_low  |
| Data[16] | end frame                            | 0XFA    |

Port return value: FE FE 0E 82 00 00 00 00 13 88 00 00 00 00 00 00 FA

How to get x coordinate:

temp = x_low + x_high*256

x coordinate =（temp\33000 ?(temp – 65536) : temp)/10

*Explanation: if temp is greater than 33000, temp minus 65536, and then is divided by 100; if temp is less than 33000, then temp is divided by 100 directly.*

*y coordinate is counted in a similar way.*

How to get rx coordinate:

temp = rx_low + rx_high*256

x coordinate =（temp \ 33000 ?(temp – 65536) : temp）/100

*Explanation: if temp is greater than 33000, temp minus 65536, and then is divided by 100; if temp is less than 33000, then temp is divided by 100 directly.*

*ry coordinate is counted in a similar way.*

******

### Setting World Coordinate

| Domain   | Explanation                          | Data    |
| -------- | ------------------------------------ | ------- |
| Data[0]  | identification frame                 | 0XFE    |
| Data[1]  | identification frame                 | 0XFE    |
| Data[2]  | data-length frame                    | 0X0E    |
| Data[3]  | command frame                        | 0X83    |
| Data[4]  | specify high status of x coordinate  | x_high  |
| Data[5]  | specify low status of x coordinate   | x_low   |
| Data[6]  | specify high status of y coordinate  | y_high  |
| Data[7]  | specify low status of y coordinate   | y_low   |
| Data[8]  | specify high status of z coordinate  | z_high  |
| Data[9]  | specify low status of z coordinate   | z_low   |
| Data[10] | specify high status of rx coordinate | rx_high |
| Data[11] | specify low status of rx coordinate  | rx_low  |
| Data[12] | specify high status of ry coordinate | ry_high |
| Data[13] | specify low status of ry coordinate  | ry_low  |
| Data[14] | specify high status of rz coordinate | rz_high |
| Data[15] | specify low status of rz coordinate  | rz_low  |
| Data[16] | end frame                            | 0XFA    |

Port transmission: FE FE 0E 83 00 00 00 00 13 88 00 00 00 00 00 00 FA

How to get x coordinate:

temp = x_low + x_high*256

x coordinate =（temp\33000 ?(temp – 65536) : temp)/10

*Explanation: if temp is greater than 33000, temp minus 65536, and then is divided by 100; if temp is less than 33000, then temp is divided by 100 directly.*

*y coordinate is counted in a similar way.*

How to get rx coordinate:

temp = rx_low + rx_high*256

x coordinate =（temp \ 33000 ?(temp – 65536) : temp）/100

*Explanation: if temp is greater than 33000, temp minus 65536, and then is divided by 100; if temp is less than 33000, then temp is divided by 100 directly.*

*ry coordinate is counted in a similar way.*

******

#### Acquiring World Coordinate

| Domain  | Explanation       | Data |
|---------|------------|------|
| Data[0] | identification frame     | 0XFE |
| Data[1] | identification frame     | 0XFE |
| Data[2] | data-length frame  | 0X03 |
| Data[3] | command frame   | 0X6E |
| Data[4] | end frame     | 0XFA |

   Port transmission：FE FE 02 82 FA

   Return value: data structure

| Domain   | Explanation      | Data     |
|----------|----------------|----------|
| Data[0]  | return value: identification frame   | 0XFE     |
| Data[1]  | return value: identification frame   | 0XFE     |
| Data[2]  | return value: data-length frame  | 0X0E     |
| Data[3]  | return value: command frame   | 0X84     |
| Data[4]  | specify high status of x coordinate  | x_high  |
| Data[5]  | specify low status of x coordinate   | x_low   |
| Data[6]  | specify high status of y coordinate  | y_high  |
| Data[7]  | specify low status of y coordinate   | y_low   |
| Data[8]  | specify high status of z coordinate  | z_high  |
| Data[9]  | specify low status of z coordinate   | z_low   |
| Data[10] | specify high status of rx coordinate | rx_high |
| Data[11] | specify low status of rx coordinate  | rx_low  |
| Data[12] | specify high status of ry coordinate | ry_high |
| Data[13] | specify low status of ry coordinate  | ry_low  |
| Data[14] | specify high status of rz coordinate | rz_high |
| Data[15] | specify low status of rz coordinate  | rz_low  |
| Data[16] | end frame        | 0XFA     |

Port return value：FE FE 0E 84 00 00 00 00 13 88 00 00 00 00 00 00 FA 

How to get x coordinate:

temp = x_low + x_high*256

x coordinate =（temp\33000 ?(temp – 65536) : temp)/10

*Explanation: if temp is greater than 33000, temp minus 65536, and then is divided by 100; if temp is less than 33000, then temp is divided by 100 directly.*

*y coordinate is counted in a similar way.*

How to get rx coordinate:

temp = rx_low + rx_high*256

x coordinate =（temp \ 33000 ?(temp – 65536) : temp）/100

*Explanation: if temp is greater than 33000, temp minus 65536, and then is divided by 100; if temp is less than 33000, then temp is divided by 100 directly.*

*ry coordinate is counted in a similar way.*

******

###  Setting Base Coordinate

| Domain  | Explanation                      | Data  |
| ------- | -------------------------------- | ----- |
| Data[0] | identification frame             | 0XFE  |
| Data[1] | identification frame             | 0XFE  |
| Data[2] | data-length frame                | 0X03  |
| Data[3] | command frame                    | 0X85  |
| Data[4] | base coordinate/world coordiante | 00/01 |
| Data[5] | end frame                        | 0XFA  |

Example:

Given that the coordinate should be set as world coordinate

port transmission: FE FE 03 85 01 FA

no return value

******

### Acquiring Base Coordinate

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X86 |
| Data[4] | end frame            | 0XFA |

Example:

port transmission: FE FE 02 86 FA

Return value: data structure

| Domain  | Explanation                        | Data  |
| ------- | ---------------------------------- | ----- |
| Data[0] | return value: identification frame | 0XFE  |
| Data[1] | return value: identification frame | 0XFE  |
| Data[2] | return value: data-length frame    | 0X03  |
| Data[3] | return value: command frame        | 0X86  |
| Data[4] | base coordinate/world coordiante   | 00/01 |
| Data[5] | end frame                          | 0XFA  |

Example:

port transmission: FE FE 03 86 01 FA

*******

### Setting End Coordinate

| Domain  | Explanation          | Data  |
| ------- | -------------------- | ----- |
| Data[0] | identification frame | 0XFE  |
| Data[1] | identification frame | 0XFE  |
| Data[2] | data-length frame    | 0X03  |
| Data[3] | command frame        | 0X89  |
| Data[4] | flange               | 00/01 |
| Data[5] | end frame            | 0XFA  |

Example:

Given that end coordinate are set as tool

port transmission: FE FE 03 89 01 FA

no return value

******

### Acquiring End Coordinate

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0X8a |
| Data[4] | end frame            | 0XFA |

Example:

port transmission: FE FE 02 8a FA

Return value: data structure

| Domain  | Explanation          | Data  |
| ------- | -------------------- | ----- |
| Data[0] | identification frame | 0XFE  |
| Data[1] | identification frame | 0XFE  |
| Data[2] | data-length frame    | 0X03  |
| Data[3] | command frame        | 0X8a  |
| Data[4] | flange               | 00/01 |
| Data[5] | end frame            | 0XFA  |

Example:

port return value: FE FE 03 86 01 FA


### Setting IO Output

| Domain  | Explanation                | Data      |
| ------- | -------------------------- | --------- |
| Data[0] | identification frame       | 0XFE      |
| Data[1] | identification frame       | 0XFE      |
| Data[2] | data-length frame          | 0X04      |
| Data[3] | command frame              | 0Xa0      |
| Data[4] | pin number                 | Pin_no    |
| Data[5] | signal of electrical level | 0X00/0X01 |
| Data[4] | end frame                  | 0XFA      |

Example:

Setting high electrical level of pin 2

Port transmission: FE FE 02 a0 02 01 FA

******

### Reading IO Output

| Domain  | Explanation          | Data   |
| ------- | -------------------- | ------ |
| Data[0] | identification frame | 0XFE   |
| Data[1] | identification frame | 0XFE   |
| Data[2] | data-length frame    | 0X03   |
| Data[3] | command frame        | 0Xa1   |
| Data[4] | pin number           | Pin_no |
| Data[4] | end frame            | 0XFA   |

Port transmission: FE FE 02 a0 02 01 FA

Return value: data structure

| Domain  | Explanation                        | Data      |
| ------- | ---------------------------------- | --------- |
| Data[0] | return value: identification frame | 0XFE      |
| Data[1] | return value: identification frame | 0XFE      |
| Data[2] | return value: data-length frame    | 0X04      |
| Data[3] | return value: command frame        | 0Xa1      |
| Data[4] | pin number                         | Pin_no    |
| Data[5] | signal of electrical level         | 0X00/0X01 |
| Data[6] | end frame                          | 0XFA      |

Example:

Given that pin 2 has high electrical level

Port return value: FE FE 04 a1 02 01 FA

*****

### Acquirng WiFi Account & Password

| Domain  | Explanation          | Data |
| ------- | -------------------- | ---- |
| Data[0] | identification frame | 0XFE |
| Data[1] | identification frame | 0XFE |
| Data[2] | data-length frame    | 0X02 |
| Data[3] | command frame        | 0Xb1 |
| Data[4] | end frame            | 0XFA |

Example:

Port transmission: FE FE 02 b1 FA

port return value: ssid: MyCobotWiFi2.4G password: mycobot123

ssid: WiFi account

password: WiFi password

******

#### The joint running speed is obtained

| Domain  | Explanation       | Data |
|---------|------------|-----------|
| Data[0] | identification frame   | 0XFE |
| Data[1] | identification frame   | 0XFE |
| Data[2] | data-length frame      | 0X03 |
| Data[3] | command frame          | 0XE1 |
| Data[5] | end frame              | 0XFA |

   Port transmission：FE FE 02 E1 FA

   Return value: data structure

| Domain   | Explanation       --------------------| Data     |
|----------|----------------|----------------------|
| Data[0]  | return value: identification frame    | 0XFE     |
| Data[1]  | return value: identification frame    | 0XFE     |
| Data[2]  | return value: data-length frame       | 0X14     |
| Data[3]  |  return value: command frame          | 0XE1     |
| Data[4]  | The high bytes of the running speed of No. 1 servo| 1_speed_high   |
| Data[5]  | The low bytes of the running speed of No. 1 servo| 1_speed_low    |
| Data[6]  | The high bytes of the running speed of No. 2 servo  | 2_speed_high   |
| Data[7]  | The low bytes of the running speed of No. 2 servo  | 2_speed_low    |
| Data[8]  | The high bytes of the running speed of No. 3 servo  | 3_speed_high   |
| Data[9]  | The low bytes of the running speed of No. 3 servo  | 3_speed_low    |
| Data[10]  | The high bytes of the running speed of No. 4 servo  | 4_speed_high   |
| Data[11]  | The low bytes of the running speed of No. 4 servo  | 4_speed_low    |
| Data[12]  | The high bytes of the running speed of No. 5 servo  | 5_speed_high   |
| Data[13]  | The low bytes of the running speed of No. 5 servo  | 5_speed_low    |
| Data[14]  | The high bytes of the running speed of No. 6 servo  | 6_speed_high   |
| Data[15]  | The low bytes of the running speed of No. 6 servo  | 6_speed_low    |
| Data[16] | end frame       | 0XFA     |

range of speed:+-3000;

Suppose that the manipulator is not moving.

port return value:FE FE 0E E1 00 00 00 00 00 00 00 00 00 00 00 00 FA

******

#### Acquisition of joint current

| Domain  | Explanation              | Data |
|---------|--------------------------|------|
| Data[0] | identification frame     | 0XFE |
| Data[1] | identification frame     | 0XFE |
| Data[2] | data-length frame        | 0X02 |
| Data[3] | command frame            | 0XE2 |
| Data[5] | end frame                | 0XFA |

   Port transmission：FE FE 02 E2 FA

   Return value: data structure

| Domain   | Explanation       | Data     |
|----------|----------------------------------|------------------|
| Data[0]  |return value: identification frame| 0XFE             |
| Data[1]  |return value: identification frame| 0XFE             |
| Data[2]  | return value: data-length frame  | 0X14             |
| Data[3]  |  return value: command frame     | 0XE2             |
| Data[4]  |The current of No. 1 servo is high|1_currents_high   |
| Data[5]  | The current of No. 1 servo is low| 1_currents_low   |
| Data[6]  |The current of No. 2 servo is high| 2_currents_high  |
| Data[7]  | The current of No. 2 servo is low| 2_currents_low   |
| Data[8]  |The current of No. 3 servo is high| 3_currents_high  |
| Data[9]  | The current of No. 3 servo is low| 3_currents_low   |
| Data[10] |The current of No. 4 servo is high| 4_currents_high  |
| Data[11] | The current of No. 4 servo is low| 4_currents_low   |
| Data[12] |The current of No. 5 servo is high| 5_currents_high  |
| Data[13] | The current of No. 5 servo is low| 5_currents_low   |
| Data[14] |The current of No. 6 servo is high| 6_currents_high  |
| Data[15] | The current of No. 6 servo is low| 6_currents_low   |
| Data[16] | end frame                        | 0XFA             |

range of current:0-3250;

Suppose that the manipulator is not moving.

port return value:FE FE 0E E2 00 00 00 00 00 00 00 00 00 00 00 00 FA

******

------

#### Acquisition of joint voltage

| Domain  | Explanation       | Data |
|---------|------------|------|
| Data[0] | identification frame     | 0XFE |
| Data[1] | identification frame     | 0XFE |
| Data[2] | data-length frame        | 0X02 |
| Data[3] | command frame            | 0XE3 |
| Data[5] | end frame                | 0XFA |

   Port transmission：FE FE 02 E3 FA

   Return value: data structure

| Domain   | Explanation       | Data     |
|----------|----------------|----------|
| Data[0]  | return value: identification frame | 0XFE     |
| Data[1]  | return value: identification frame | 0XFE     |
| Data[2]  | return value: data-length frame    | 0X08     |
| Data[3]  |  return value: command frame       | 0XE3     |
| Data[4]  | Voltage of servo No. 1| 1_voltage  |
| Data[5]  | Voltage of servo No. 2| 2_voltage  |
| Data[6]  | Voltage of servo No. 3| 3_voltage  |
| Data[7]  | Voltage of servo No. 4| 4_voltage  |
| Data[8]  | Voltage of servo No. 5| 5_voltage  |
| Data[9]  | Voltage of servo No. 6| 6_voltage  |
| Data[16] | end frame             | 0XFA       |

range of voltage:0-240;

How to find the actual voltage:

Voltage calculation: actual voltage = returned voltage data /10

port return value:FE FE 08 E3 E9 E9 EA EB E5 E5 FA 

------

#### Get the joint state.

| Domain  | Explanation       | Data |
|---------|------------|------|
| Data[0] | identification frame     | 0XFE |
| Data[1] | identification frame     | 0XFE |
| Data[2] | data-length frame        | 0X02 |
| Data[3] | command frame            | 0XE4 |
| Data[5] | end frame                | 0XFA |

   Port transmission：FE FE 02 E4 FA

   Return value: data structure

| Domain   | Explanation                        | Data     |
|----------|------------------------------------|----------|
| Data[0]  | return value: identification frame | 0XFE     |
| Data[1]  | return value: identification frame | 0XFE     |
| Data[2]  | return value: data-length frame    | 0X08     |
| Data[3]  |  return value: command frame       | 0XE4     |
| Data[4]  | Status of servo No. 1              |1_voltage |
| Data[5]  | Status of servo No. 2              |2_voltage |
| Data[6]  | Status of servo No. 3              |3_voltage |
| Data[7]  | Status of servo No. 4              |4_voltage |
| Data[8]  | Status of servo No. 5              |5_voltage |
| Data[9]  | Status of servo No. 6              |6_voltage |
| Data[16] | end frame                          | 0XFA     |

All joints are assumed to be normal

port return value:FE FE 08 E4 00 00 00 00 00 00 FA

------

#### Getting joint temperature.

| Domain  | Explanation              | Data |
|---------|--------------------------|------|
| Data[0] | identification frame     | 0XFE |
| Data[1] | identification frame     | 0XFE |
| Data[2] | data-length frame        | 0X02 |
| Data[3] | command frame            | 0XE5 |
| Data[5] | end frame                | 0XFA |

   Port transmission：FE FE 02 E5 FA

   Return value: data structure

| Domain   | Explanation       | Data     |
|----------|----------------|----------|
| Data[0]  | return value: identification frame | 0XFE       |
| Data[1]  | return value: identification frame | 0XFE       |
| Data[2]  | return value: data-length frame    | 0X8        |
| Data[3]  |  return value: command frame       | 0XE5       |
| Data[4]  | Temperature of servo No. 1         | 1_voltage  |
| Data[5]  | Temperature of servo No. 2         | 2_voltage  |
| Data[6]  | Temperature of servo No. 3         | 3_voltage  |
| Data[7]  | Temperature of servo No. 4         | 4_voltage  |
| Data[8]  | Temperature of servo No. 5         | 5_voltage  |
| Data[9]  | Temperature of servo No. 6         | 6_voltage  |
| Data[16] | end frame                          | 0XFA       |

The temperature values returned are 0-255.

port return value:FE FE 08 E5 22 1A 23 22 1E 22 FA

------

#### Gets the pdi of a single servo before modification (used in drag demonstration).

| Domain  | Explanation       | Data |
|---------|------------|------|
| Data[0] | identification frame     | 0XFE |
| Data[1] | identification frame     | 0XFE |
| Data[2] | data-length frame        | 0X02 |
| Data[3] | command frame            | 0XE6 |
| Data[5] | end frame                | 0XFA |

   Port transmission：FE FE 02 E6 FA

   Return value: data structure

| Domain   | Explanation                        | Data     |
|----------|------------------------------------|----------|
| Data[0]  | return value: identification frame | 0XFE     |
| Data[1]  | return value: identification frame | 0XFE     |
| Data[2]  | return value: data-length frame    | 0X08     |
| Data[3]  |  return value: command frame       | 0XE6     |
| Data[4]  | Steering gear number               | id       |
| Data[5]  | proportion                         | P        |
| Data[6]  | differential                       | D        |
| Data[7]  | integration                        | I        |
| Data[8] | end frame                           | 0XFA     |

The pdi values returned are 0-255.

port return value:FE FE 06 E6 01 ff ff ff ff FA

------
## Appendix

The specific method of adding coordinate-exchanging programs from Atom repository and sports repository are listed as following.

1. Change end coordinate.

2. Set end coordinate by setEndType and getEndType. 

   *FLANGE: Setting EndType as FLANG; TOOL: Setting EndType as TOOL End.*

3. Read tool coordinate via setToolReference and getToolReference. (FLANGE serves as relative coordinate and information about tool end is relevant to FLANGE coordinate.)

4. After setting EndType as FLANGE, GetCoords and WriteCoords are counted depending on FLANG's position.

5. After setting EndType as TOOL, GetCoords and WriteCoords are counted depending on end's position.

6. Change base coordinate.

7. Set base coordinate by setReferenceFrame. 

   *RFType: Base means setting robotic pedestal as base coordinate; RFType:WORLD means make world coordinate as base coordinate; getReferenceFrame serves to read the type of base coordinate.*

8. setWorldReference and getWorldReference works to read information about base coordinate. World coordinate serves as relative coordinate to type pedestal information of robots that is relevant to world coordinate.
9. If base coordinate acts as pedestal, GetCoords and WriteCoords take base coordinate as reference coordinate.
10. If world coordinate acts as pedestal, GetCoords and WriteCoords take world coordinate as reference coordinate.

### Information Updating About Communication

These are newly-added functions: setting and reading of end coordinate, world coordinate, present coordinate, type of end, as well as moving method, and sending and receiving of information on robotic arms.

The communication is temporarily set as from 0x80 to 0x8A.

The roboticMessages space is specially added in ParameterList.h for more communication information. Now only `No Analytical Solution` signal has been added, while more will be added later.

The outline of MOVEL are listed as follows:

First, count the Euclidean Distance between starting point and target point. Second, interpolation points are inserted every 10mm, based on  the Euclidean Distance. If interpolation points do not have analytical solution, then search for plus or minus PI/30 from three unchangeable direction for analytical solution. Avoid singular value and some distinctive position that cannot get analytical solution.

Change the interval between MOVEL and JOG into dynamic time. And then, count the time spend moving through the maximum joint. And then, minus the moving time by specific period to take it as interval.



