# Deep dive

See: <https://github.com/linuxcnc-ethercat/linuxcnc-ethercat/blob/master/documentation/configuration-reference.md>

## CiA 402

<https://github.com/linuxcnc-ethercat/linuxcnc-ethercat/blob/master/documentation/cia402.md>

To use a CiA 402 EtherCAT device with LinuxCNC, you need two things:

    A way to get the EtherCAT layer talking to your device. This can be done with a generic driver and a bunch of XML, or a device-specific driver that (at least potentially) makes everything much less trouble-prone. This document covers the device-specific driver case, but some of the details can be used with generic as well. See the LinuxCNC forums for help with generic configs.
    The CiA 402 HAL component for LinuxCNC, from https://github.com/dbraun1981/hal-cia402


## distributed clocks

<https://github.com/linuxcnc-ethercat/linuxcnc-ethercat/blob/master/documentation/distributed-clocks.md>

EtherCAT has a feature called "distributed clocks" that seems to confuse many people on the LinuxCNC forum. Distributed clocks (when enabled) allow devices to stay synchronized to each other with very small amounts of jitter (typically a few microseconds). The EtherCAT master needs to be told to synchronize LinuxCNC's servo thread to the distributed clock time. This allows LinuxCNC's motion planner to tell EtherCAT servo and stepper drives where to be at a specific time around 1 millisecond, and have them almost exactly match the plan provided by the software.

 Fixed PDO Mapping
This section describes the contents of fixed PDO mapping for 1S-series Servo Drives with Built-in
EtherCAT Communications. You cannot change these contents.
Use Sync Manager 2 PDO Assignment (1C12 hex) and Sync Manager 3 PDO Assignment (1C13
hex) to specify the PDO mapping you use.
Some typical examples of RxPDO and TxPDO combinations are provided below.

RxPDO: 259th receive
PDO Mapping
(1702 hex)

Controlword (6040 hex), Target position (607A hex), Target velocity (60FF hex),
Target torque (6071 hex), Modes of operation (6060 hex), Touch probe function
(60B8 hex), and Max profile velocity (607F hex)

TxPDO: 261th transmit
PDO Mapping
(1B04 hex)

Error code (603F hex), Statusword (6041 hex), Position actual value (6064 hex),
Torque actual value (6077 hex), Modes of operation display (6061 hex), Touch
probe status (60B9 hex), Touch probe 1 positive edge (60BA hex), Touch probe 2
positive edge (60BC hex), Digital inputs (60FD hex), and Velocity actual value
(606C hex)


## Modes of Operationcn1

| **Mode Code** | **Mode Name**                     | **Description**                          |
|----------------|-----------------------------------|------------------------------------------|
| CSP            | Cyclic Synchronous Position Mode  | Controls position in synchronization with the network cycle. |
| CSV            | Cyclic Synchronous Velocity Mode  | Controls velocity in synchronization with the network cycle. |
| CST            | Cyclic Synchronous Torque Mode    | Controls torque in synchronization with the network cycle. |
| PP             | Profile Position Mode             | Moves the motor to a target position following a defined motion profile. |
| PV             | Profile Velocity Mode             | Controls velocity according to a defined motion profile. |
| HM             | Homing Mode                       | Performs homing to establish the motor’s reference position. |

## Object dictionary

CAN application protocol over EtherCAT (CoE) uses the object dictionary as its base. All objects are
assigned four-digit hexadecimal indexes in the areas shown in the following table.

| **Index (hex)** | **Area**                        | **Description** |
|------------------|----------------------------------|-----------------|
| 0000 to 0FFF     | Data Type Area                  | Definitions of data types. |
| 1000 to 1FFF     | CoE Communications Area         | Definitions of objects that can be used by all servers for designated communications. |
| 2000 to 2FFF     | Manufacturer-Specific Area 1    | Objects with common definitions for all OMRON products. |
| 3000 to 5FFF     | Manufacturer-Specific Area 2    | Objects with common definitions for all 1S-series Servo Drives (servo parameters). |
| 6000 to DFFF     | Device Profile Area             | Objects defined in the Servo Drive’s CiA402 drive profile. |
| E000 to EFFF     | Device Profile Area 2           | Objects defined in the Servo Drive’s FSoE CiA402 slave connection. |
| F000 to FFFF     | Device Area                     | Objects defined in a device. |
