# Anatomy of a Carvera

This section provides details on _how_ the Carvera family of CNC machines is working. This article includes content for both the Original Carvera (sometimes called C1) and the Carvera Air (CA1). Do note that while the machines are similar they also have small differences.

{% hint style="info" %}
None of this info is required to be able to successfully use the machine, but you may find some of this information useful for troubleshooting, repairing, or later upgrading the machine.
{% endhint %}

## Physical Dimensions

{% tabs %}
{% tab title="Carvera" %}
| Basic Info             | S.A.E.                               | Metric                            |
| ---------------------- | ------------------------------------ | --------------------------------- |
| Work Area (3 axis)     | 14.2in (X) \* 9.4in (Y) \* 5.5in (Z) | 36cm (X) \* 24cm (Y) \* 14cm (Z)  |
| Work Area (4 axis)     | 3.6in (Diameter) \* 9.4in (Length)   | 9.2cm (Diameter) \* 24cm (Length) |
| Gantry Clearance       | 4.7in                                | 12cm                              |
| Max Height (Lid Open)  | 33in                                 | 84cm                              |
| Footprint (Lid Closed) | 22.8in \* 20.5in \* 21.3in           | 57.9cm \* 52cm \* 54.1cm          |
| Weight (Approx)        | 110lbs                               | 50kg                              |

#### Work Area

The Carvera has a cutting area of around 374mm (X) \* 256mm (Y). One thing to consider is that this cutting area is somewhat larger than the bed area which is 360mm (X) \* 240mm (Y)

Some implications of the design may not be immediately apparent:

* Due to the proximity of the tool holder to the bed, part of the bed is in difficult to use due to risks of crashing tools into the raised tool holder bar
* Due to the black plastic used by the tool proximity sensor approximately 15mm of the bed is lost in the X direction.
{% endtab %}

{% tab title="Carvera Air" %}
| Basic Info             | S.A.E.                                              | Metric                                        |
| ---------------------- | --------------------------------------------------- | --------------------------------------------- |
| Work Area (3 axis)     | 11.8in (X) \* 7.9n (Y) \* 5.1in (Z)                 | 30cm (X) \* 20cm (Y) \* 13cm (Z)              |
| Work Area (4 axis)     | 3.6in (Diameter) \* 7.9in (Length)                  | 9.2cm (Diameter) \* 20cm (Length)             |
| Gantry Clearance       | 4.7in                                               | 12cm                                          |
| Max Height (Lid Open)  | 33in                                                | 84cm                                          |
| Footprint (Lid Closed) | 19.7in (Width) \* 17.7in (Depth) \* 17.7in (Height) | 50cm (Width) \* 45cm (Depth) \* 45cm (Height) |
| Weight (Approx)        | 66lbs                                               | 30kg                                          |
{% endtab %}
{% endtabs %}

## Electronics

### Control Board

Both the original Carvera and Carver Air use the same control board. This uses a 32-bit ARM Cortex-M3 microcontroller called the NXP LPC1768 and runs a firmware based on [Smoothieware v1](https://github.com/Smoothieware/Smoothieware).

### Power Supply

{% tabs %}
{% tab title="Carvera" %}
| Power                 | Value                             |
| --------------------- | --------------------------------- |
| Supported Mains Input | 100-120VAC / 200-240VAC @ 50/60Hz |
| Spindle PSU           | 200w w/ 48v output                |
| System PSU            | 200w w/ 12v output                |
| Spindle PSU Model     | Mean Well EPP-200-48              |
| System PSU Model      | Mean Well EPP-200-12              |


{% endtab %}

{% tab title="Carvera Air" %}
| Power                 | Value                             |
| --------------------- | --------------------------------- |
| Supported Mains Input | 100-120VAC / 200-240VAC @ 50/60Hz |


{% endtab %}
{% endtabs %}

## Spindle

{% tabs %}
{% tab title="Carvera" %}
| Spindle            | Value                                        |
| ------------------ | -------------------------------------------- |
| Input Power        | 200w                                         |
| Speed              | 1000 - 15000 RPM                             |
| Included Collet    | 1/8in                                        |
| Available Collets  |  1/4in, 6mm, 4mm, 3mm                        |
| Collet System Type | 263504                                       |
| Tool Library Size  | 6                                            |
| Cooling            | Air-cooled controlled by temperature monitor |
| Spindle Runout     | Less than 0.01mm / 0.0003in                  |


{% endtab %}

{% tab title="Carvera Air" %}
| Spindle            | Value                                        |
| ------------------ | -------------------------------------------- |
| Input Power        | 200w                                         |
| Speed              | 3000 - 13000 RPM                             |
| Included Collet    | 1/8in                                        |
| Available Collets  |  1/4in, 6mm, 4mm, 3mm                        |
| Collet System Type | 263504                                       |
| Cooling            | Air-cooled controlled by temperature monitor |
| Spindle Runout     | Less than 0.01mm / 0.0003in                  |


{% endtab %}
{% endtabs %}

## Motion System

{% tabs %}
{% tab title="Carvera" %}
| Motion                   | Value                                                        |
| ------------------------ | ------------------------------------------------------------ |
| Drive System             | Ball screws with linear rails                                |
| Max Movement Speed (X+Y) | 600cm / 236in per minute                                     |
| Motors                   | Servo motors for X/Y/Z axis, Nema17 stepper motor for A axis |
| Motion Motor Resolution  | 0.005 mm/ 0.0002in                                           |
| Motion Driver IC         | BigTreeTech TMC2209                                          |

### Servo Motors

The X, Y, and Z Axis of the Carvera are driven by **Servo Motors**. These function similar to **Stepper Motors** but have the benefit of knowing the amount of movement they achieved. This makes them "closed-loop", they move and can verify if they moved the requested amount.&#x20;

If you got back to the [Coordinate System](cnc-workflow.md#coordinate-systems) theory, you will remember that most hobby CNC machines know their current position based on their memory of the movements since H**oming,** this is because they typically use **Stepper Motors.** The problem with this is that each requested movement is assumed to have been completed successfully. If their is insufficient torque in the motor for example and the system is unable to complete the requested movement operation, a normal stepper motor will "skip" a few motion steps, and the machine controller will be none the wiser. This would result in the physical location to be different to where the machine thinks it is! This is very bad because every movement operation until the machine is re-homed will be incorrect.

Servo Motors address this problem by being able to verify that the requested movement was actually achieved or not. In the Carvera if a Servo is unable to complete the requested movement a halt condition will be raised and G-code playback aborted.

Servo Motors verify that the requested movement was successful using a feedback device on the output side of the servo motor itself, such as an encoder or potentiometer.
{% endtab %}

{% tab title="Carvera Air" %}
| Motion                   | Value                                                                   |
| ------------------------ | ----------------------------------------------------------------------- |
| Drive system             | Ball screws with linear rods                                            |
| Max traverse speed (X+Y) | 600cm / 236in per minute                                                |
| Motors                   | <p>Stepper motors for X/Y/Z axis<br>Nema17 stepper motor for A axis</p> |
| Motion motors resolution | 0.005 mm/ 0.0002in                                                      |

### Stepper motors

As the name implies, stepper motors are designed to rotate by small angle increments, rather than rotating continuously as conventional motors do when they are powered. Stepper motors are ubiquitous in hobby CNCs, for a good reason: they provide the capability to move by a precise amount (i.e. a given number of steps), without the need for a position feedback mechanism.

The stepper motors on the Carvera are **NEMA17** (just a fancy way to say that their front/back face size is 1.7'' square), are of the "**bipolar**" type (which means that they are controlled by two pairs of wires, the "A" phase and the "B" phase, and that one step is made by sending current in A and turning off B, or the other way around), and are internally designed so that each step rotates the shaft by exactly 1.8°, which translates to 360°/1.8° = **200 steps to perform a full revolution**.

It's the job of the **stepper driver** to generate current alternatively in the A and B phases, when instructed to move by one or more steps.

{% hint style="info" %}
The stepper drivers use a trick to optimize the torque: instead of generating a constant voltage (and therefore current) in a phase, they generate a higher voltage and chop it at a high frequency so that it produces the desired value on average. It's just an implementation detail, but it explains the (normal) humming sound that the motors emit when the machine is idle.
{% endhint %}

But it gets better: instead of just turning the current on and off completely alternatively on phases A and B to move one step at a time, if the stepper driver sends a carefully chosen variable amount of current in A and B simultaneously, then it is possible to reach (and hold) intermediate positions between two steps: these are called "**microsteps**", and the drivers used in the Carvera Air are capable of controlling **8 microsteps for each step**.

So overall, the motors can be controlled with a precision of 200 × 8 = 1600 microsteps per revolution. Combined with the ball screws, this means 0.005mm accuracy is achieved.

{% hint style="info" %}
Interestingly, stepper motors use more power when doing nothing (_i.e._, holding a position) than when moving. This is why they tend to get warm when the machine is on but idle. This is not a problem in itself, the motors are designed to support high temperatures but you might as well turn the machine off if it is going to stay idle for a long time, if only to save power.
{% endhint %}

It is the job of the **controller** to send commands to the stepper drivers (which in turn will generate the right current waveforms on the motor phases), to achieve the desired movements of the machine.
{% endtab %}
{% endtabs %}

## Dust/Chip Control

{% tabs %}
{% tab title="Carvera" %}
| Dust Control                          | Value                         |
| ------------------------------------- | ----------------------------- |
| Dust Control System                   | Integrated or external vacuum |
| Dust Tank Volume                      | 0.8 liter                     |
| External Dust Port OD Size            | 26mm                          |
| External Compressed Air Input OD Size | 8mm                           |
| External Compressed Air Input Fitting | Pushlock                      |
| Recommended Compressed Air Pressure   | 3.5 bar (50 psi)              |
| Recommended Compressed Air Flow       | 40L/min                       |


{% endtab %}

{% tab title="Carvera Air" %}
| Dust Control                          | Value                         |
| ------------------------------------- | ----------------------------- |
| Dust Control System                   | Integrated or external vacuum |
| External Dust Port OD Size            | 26mm                          |
| External Compressed Air Input OD Size | 8mm                           |
| External Compressed Air Input Fitting | Pushlock                      |
| Recommended Compressed Air Pressure   | 3.5 bar (50 psi)              |
| Recommended Compressed Air Flow       | 40L/min                       |


{% endtab %}
{% endtabs %}

### Laser Engraver

{% tabs %}
{% tab title="Carvera" %}
| Laser Engraver | Value             |
| -------------- | ----------------- |
| Laser power    | 2.5w              |
| Laser Type     | 445nm diode laser |
| Cooling        | Air-cooled        |
{% endtab %}

{% tab title="Carvera Air" %}
| Laser Engraver | Value             |
| -------------- | ----------------- |
| Laser power    | 5w                |
| Laser Type     | 445nm diode laser |
| Cooling        | Air-cooled        |


{% endtab %}
{% endtabs %}

