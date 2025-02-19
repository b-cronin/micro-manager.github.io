---
autogenerated: true
title: WieneckeSinske
redirect_from: /wiki/WieneckeSinske
layout: page
---

## WSB PiezoDrive CAN adapter

<table>
<tr>
<td markdown="1">

**Summary:**

</td>
<td markdown="1">

Interfaces [Wienecke & Sinske GmbH](http://www.wienecke-sinske.de) piezo
stage controller

</td>
</tr>
<tr>
<td markdown="1">

**Author:**

</td>
<td markdown="1">

[S3L GmbH](http://www.s3l.de/home/index/en)

</td>
</tr>
<tr>
<td markdown="1">

**License:**

</td>
<td markdown="1">

BSD

</td>
</tr>
<tr>
<td markdown="1">

**Platforms:**

</td>
<td markdown="1">

All Platforms (serial port)

</td>
</tr>
<tr>
<td markdown="1">

**Devices:**

</td>
<td markdown="1">

XYStage

</td>
</tr>
</table>

This adapter interfaces piezo xy stages driven by [Wienecke &
Sinske](http://www.wienecke-sinske.de) WSB PiezoDrive CAN controller. It
communicates with the controller through the serial port so that no
further software is needed.  
If the stage is connected via CAN to a motorized Zeiss microscope, you
also may use the ZeissCAN29 motorized microscope device to control those
stages.

Special properties of the XYStage:  

<table valign='left'>
<tr>
<td markdown="1">

**Velocity (micron/s)**

</td>
<td markdown="1">

Sets the maximum speed of the XY stage

</td>
</tr>
<tr>
<td markdown="1">

**Acceleration (micron/s^2)**

</td>
<td markdown="1">

Sets the acceleration of the XY stage

</td>
</tr>
</table>

COM Settings:  

<table valign='left'>
<tr>
<td markdown="1">

**Answer Timeout**

</td>
<td markdown="1">

Long enough for stage to complete moves

</td>
</tr>
<tr>
<td markdown="1">

**Baud Rate**

</td>
<td markdown="1">

57600

</td>
</tr>
<tr>
<td markdown="1">

**Delay Between Chars Ms**

</td>
<td markdown="1">

0

</td>
</tr>
<tr>
<td markdown="1">

**Handshaking**

</td>
<td markdown="1">

Off

</td>
</tr>
<tr>
<td markdown="1">

**Parity**

</td>
<td markdown="1">

None

</td>
</tr>
<tr>
<td markdown="1">

**Stop Bits**

</td>
<td markdown="1">

1

</td>
</tr>
</table>

