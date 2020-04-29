CAN J1939 Set Request Messages
*******************************

.. contents:: Contents
    :local:

.. toctree::
    :maxdepth: 1

**Set Commands**


The following Set requests have been implemented in J1939 based application examples.
Users can modify provided requests and/or implement their own unique commands.

.. table::  *Set Commands*
    :align: left

    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | **Request**               || **PF** || **PS**|| **PGN**     || **Payload** |  **Purpose**                            |
    |                           || (dec)  || (dec) ||             || **Length**  |                                         |
    |                           ||        ||       ||             || (bytes)     |                                         |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | *Save Configuration*      |  255    | 81     |  65361       |  3           || Save all configuration                 |
    |                           |         |        |              |              || data to non-volatile memory            |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | *Reset Algorithm*         |  255    | 80     |  65360       |  3           |  Reset algorithm to initial conditions  |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | *Mag Alignment*           |  255    | 94     |  65374       |  2           |  Mag alignment and status request       |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | *Set Packet Rate Divider* |  255    | 85     |  65365       |  2           || Set rate dividers to increase/decrease |
    |                           |         |        |              |              || rate packets are set                   |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | *Set Data Packet Type(s)* |  255    | 86     |  65366       |  2           || Select packet types to be sent         |
    |                           |         |        |              |              || periodically                           |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    || *Set Digital Filters*    |  255    | 87     |  65367       |  3           || Set LPF cutoff frequency for rate      |
    || *Cutoff Frequencies*     |         |        |              |              || sensors and accelerometers             |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | *Set Orientation*         |  255    | 88     |  65368       |  3           |  Set unit orientation                   |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    | *Set Lever Arm (TBD)*     |  255    | 95     |  65375       |  8           |  Set unit Lever Arm (where applicable)  |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    || *Set Bank of PS*         |  255    | 240    |  65520       |  8           || Reconfigure PS numbers for set         |
    || *Numbers for Bank0*      |         |        |              |              || requests                               |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
    || *Set Bank of PS*         |  255    | 241    |  65521       |  8           || Reconfigure PS numbers for set         |
    || *Numbers for Bank1*      |         |        |              |              || requests                               |
    +---------------------------+---------+--------+--------------+--------------+-----------------------------------------+
                                                 
.. note::

    PS values for all but the "Set Bank of PS Numbers for Bank0/Bank1" Set Commands can be changed by
    the the commands "Set Bank of PS Numbers" (see below). Updated values can be saved in nonvolatile memory and will be active upon
    following system restart/power-up. Provided in the table PS values are default values.



**Save Configuration**

    The next table provides the descriptions of the payload fields of the
    command and response messages.

    .. table::  *Save Configuration Request/Response Payload Fields*
        :align: left

        +----------+---------------------------------+
        | **Byte** | **Description/Values**          |
        +----------+---------------------------------+
        | 0        | Type: 0 = Request, 1 = Response |
        +----------+---------------------------------+
        | 1        | Destination Address             |
        +----------+---------------------------------+
        | 2        | Response: 0 = Fail, 1 = Succeed |
        +----------+---------------------------------+


**Reset Algorithm**

    The following table provides the descriptions of the payload fields of the
    command and response messages.

    .. table::  *Reset Algorithm Request/Response Payload Fields*
        :align: left

        +----------+---------------------------------+
        | **Byte** | **Description/Values**          |
        +----------+---------------------------------+
        | 0        | Type: 0 = Request, 1 = Response |
        +----------+---------------------------------+
        | 1        | Destination Address             |
        +----------+---------------------------------+
        | 2        | Response: 0 = Fail, 1 = Succeed |
        +----------+---------------------------------+


**Mag Alignment (INS Application Example)**

    The following tables provides the descriptions of the payload fields of the
    command and response messages.

    .. table::  *Mag Alignment Request Payload Fields*
        :align: left

        +----------+---------------------------+
        | **Byte** | **Description/Values**    |
        +----------+---------------------------+
        | 0        | Destination Address       |
        +----------+---------------------------+
        | 1        || Commands:                |
        |          || 0 - Status Request       |
        |          || 1 - Start Alignment      |
        |          || 5 - Confirm and Save     |
        +----------+---------------------------+

    .. table::    *Payload Fields of 64 bit Response*
        :align: left

        +--------------+------------------------------------+-------------------------------------------------------+
        |   **Bits**   |   **Description**                  |  Value                                                |
        +--------------+------------------------------------+-------------------------------------------------------+
        ||  bits 0:7   || Command                           || 0 - Status request,                                  |
        ||             ||                                   || 1 - Start alignment                                  |
        +--------------+------------------------------------+-------------------------------------------------------+
        ||  bit  8:15  || Alignment State                   || 0  - Idle                                            |
        ||             ||                                   || 12 - Alignment in process                            |
        ||             ||                                   || 11, 13 - Data Collection complete                    |
        +--------------+------------------------------------+-------------------------------------------------------+
        ||  bit  16:27 || Estimated Hard Iron X Bias, Gauss || -8 G to +8 G , scale 1/256 G/bit, offset -8G         |
        +--------------+------------------------------------+-------------------------------------------------------+
        ||  bits 28:39 || Estimated Hard ron Y Bias, Gauss  || -8 G to +8 G , scale 1/256 G/bit, offset -8G         |
        +--------------+------------------------------------+-------------------------------------------------------+
        ||  bits 40:49 || Estimated Soft Iron Ratio         || 0 to 1   1/1024 per Lsb                              |
        +--------------+------------------------------------+-------------------------------------------------------+
        ||  bits 50:63 || Estimated Soft Iron Angle         || -3.14 to 3.14 RAD, scale 0.0015339, offset -3.14159  |
        +--------------+------------------------------------+-------------------------------------------------------+


**Set Packet Rate Divider**

    The following table provides the values of the packet rate divider response payload

    .. table::  Set *Packet Rate Divider Request/Response Payload Fields*
        :align: left

        +--------+---------------+----------------------------------+
        | *Byte* | *Description* | **Byte Value**                   |
        +--------+---------------+----------------------------------+
        | 0      || Destination  | Unique                           |
        |        || Address      |                                  |
        +--------+---------------+----------------------------------+
        | 1      |               || **Byte Value** -                |
        |        |               | **Packet Broadcast Rate (Hz)**   |
        |        |               || 0  - Quiet Mode - no broadcast  |
        |        |  Packet       || 1  - 100 (default)              |
        |        |  Divider      || 2  - 50                         |
        |        |  Value        || 4  - 25                         |
        |        |               || 5  - 20                         |
        |        |               || 10 (0x0a) - 10                  |
        |        |               || 20 (0x14) -  5                  |
        |        |               || 25 (0x19) -  4                  |
        |        |               || 50 (0x32) -  2                  |
        +--------+---------------+----------------+-----------------+

**Set Periodic Data Packet Type(s)**

    The following table provides the *Set Data Packet Type(s)* payload. Each bit in the request payload enables specific data packet for periodic transmission.
    Any combination of data packets can be chosen.

    .. table::  *Set Data Packet Type(s) Field*
        :align: left


        +--------+----------------+-------------------------------+
        | *Byte* | *Description*  | **Byte Value**                |
        +--------+----------------+-------------------------------+
        | 0      |  Destination   | Unique                        |
        |        |  Address       |                               |
        +--------+----------------+-------------------------------+
        | 1      || Selected Data || Data Packet Type(s) Bitmask: |
        |        || Packet Type(s)|| Bit 0 - SSI2                 |
        |        || Bitmask (LSB) || Bit 1 - Angular Rate         |
        |        ||               || Bit 2 - Acceleration         |
        |        ||               || Bit 3 - Magnetometer         |
        +--------+----------------+-------------------------------+
        | 2      || Selected Data ||  Reserved                    |
        |        || Packet Type(s)||                              |
        |        || Bitmask (MSB) ||                              |
        +--------+----------------+-------------------------------+

**Set Digital Filter Cutoff Frequencies**

    The following table provides descriptions of the request payload

    .. table::  *Digital Filter Cutoff Frequencies Request Payload*
        :align: left

        +--------------+------------------------------+----------------------------+
        || **Payload** | **Description/Values**       | **Values**                 |
        || **Byte**    |                              |                            |
        +--------------+------------------------------+----------------------------+
        | 0            | Destination Address          | Unique                     |
        +--------------+------------------------------+----------------------------+
        | 1            || Cutoff Frequencies (Hz) for |                            |
        |              || Angular Rate Sensors        | 0, 2, 5, 10, 25, 40, or 50 |
        +--------------+------------------------------+----------------------------+
        | 2            || Cutoff Frequencies (Hz) for |                            |
        |              || Accelerometer Sensors       | 0, 2, 5, 10, 25, 40, or 50 |
        +--------------+------------------------------+----------------------------+


**Set Orientation**

    The following table shows the payload layout

    .. table:: *Set Orientation Payload Layout*
        :align: left

        +----------+------------------------+-------------------+
        | **Byte** | **Meaning**            | **Value**         |
        +----------+------------------------+-------------------+
        | 0        | Destination Address    | Unique            |
        +----------+------------------------+-------------------+
        | 1        | Orientation Value (MSB)|| see table below  |
        +----------+------------------------+-------------------+
        | 2        | Orientation VAlue (LSB)|| see table below  |
        +----------+------------------------+-------------------+

    The following table provides the orientation values and meanings:

    .. table:: *Set Orientation Field Descriptions*
        :align: left

        +------------------+-----------------------+------------------+----------------+
        || **Orientation** | **X/Y/Z Axis**        || **Orientation** | **X/Y/Z Axis** |
        || **Value**       |                       || **Value(cont)** |                |
        +------------------+-----------------------+------------------+----------------+
        | 0x0000           | +Ux +Uy +Uz (default) | 0x00C4           | +Uz +Uy -Ux    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0009           | -Ux -Uy +Uz           | 0x00CD           | -Uz -Uy -Ux    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0023           | -Uy +Ux +Uz           | 0x00D3           | -Uy +Uz -Ux    |
        +------------------+-----------------------+------------------+----------------+
        | 0x002A           | +Uy -Ux +Uz           | 0x00DA           | +Uy -Uz -Ux    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0041           | -Ux +Uy -Uz           | 0x0111           | -Ux +Uz +Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0048           | +Ux -Uy -Uz           | 0x0118           | +Ux -Uz +Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0062           | +Uy +Ux -Uz           | 0x0124           | +Uz +Ux +Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x006B           | -Uy -Ux -Uz           | 0x012D           | -Uz -Ux +Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0085           | -Uz +Uy +Ux           | 0x0150           | +Ux +Uz -Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x008C           | +Uz -Uy +Ux           | 0x0159           | -Ux -Uz -Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x0092           | +Uy +Uz +Ux           | 0x0165           | -Uz +Ux -Uy    |
        +------------------+-----------------------+------------------+----------------+
        | 0x009B           | -Uy -Uz +Ux           | 0x016C           | +Uz -Ux -Uy    |
        +------------------+-----------------------+------------------+----------------+


**Set Lever Arm (TBD)**

    The following table shows the payload layout

.. table:: *Set Lever Arm payload*

    +-----------+----------------------------------+
    | **Byte**  | **Description**                  |
    +-----------+----------------------------------+
    | 0         | Destination Address              |
    +-----------+----------------------------------+
    | 1         | reserved                         |
    +-----------+----------------------------------+
    | 2         | Wheel Distance Value (LSB), mm   |
    +-----------+----------------------------------+
    | 3         | Wheel Distance Value (MSB), mm   |
    +-----------+----------------------------------+
    | 4         | Lever Arm Bx Value (LSB), mm     |
    +-----------+----------------------------------+
    | 5         | Lever Arm Bx Value (MSB), mm     |
    +-----------+----------------------------------+
    | 6         | Lever Arm By Value (LSB), mm     |
    +-----------+----------------------------------+
    | 7         | Lever Arm By Value (MSB), mm     |
    +-----------+----------------------------------+

		
**Set Bank of PS Numbers**

    The following tables provide descriptions of the payload for Bank0 and Bank1 set commands

    .. table:: *Set Bank of PS Numbers for Bank0 Payload*
        :align: left

        +----------+-------------------------------+
        | **Byte** | **Parameters**                |
        +----------+-------------------------------+
        | 0        | Destination Address           |
        +----------+-------------------------------+
        | 1        | Reset Algorithm PS number     |
        +----------+-------------------------------+
        | 2        | Save Configuration PS number  |
        +----------+-------------------------------+
        | 3        | Status Request  PS number     |
        +----------+-------------------------------+
        | 4        | Mag Align Command  PS number  |
        +----------+-------------------------------+
        | 5-7      | Reserved                      |
        +----------+-------------------------------+

    .. table::  *Set Bank of PS Numbers for Bank1 Payload*
        :align: left

        +----------+------------------------------------------------+
        | **Byte** | **Parameters**                                 |
        +----------+------------------------------------------------+
        | 0        | Destination Address                            |
        +----------+------------------------------------------------+
        | 1        | Set Packet Rate PS number                      |
        +----------+------------------------------------------------+
        | 2        | Set Packet Type(s) PS number                   |
        +----------+------------------------------------------------+
        | 3        | Set Digital Filer Cutoff Frequencies PS number |
        +----------+------------------------------------------------+
        | 4        | Set Orientation PS Number                      |
        +----------+------------------------------------------------+
        | 5        | Set User Behavior PS Number                    |
        +----------+------------------------------------------------+
        | 6        | Set Lever Arm PS Number                        |
        +----------+------------------------------------------------+
        | 7        | Reserved                                       |
        +----------+------------------------------------------------+
