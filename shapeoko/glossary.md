# Glossary

This page contains a short definition of many usual terms used in the CNC world \(and throughout this book\). It is derived from [the version on the Shapeoko wiki](https://wiki.shapeoko.com/index.php/glossary), and included here for convenience.

## \#

**2D** : \(in terms of CAM\) Cuts are all made to the same depth along a single plane \(typically XY\) and may be described by a single monochrome \(b/w\) 2D drawing. Plasma cutters and lasercutters which are not doing engraving are limited to 2D, as are engraving machines which cut to only a single depth. The simplest form of CAM, any program should be able to create such a file. See [Toolpaths](toolpath-basics.md) section.

**2.5D** : \(in terms of CAM\) Cuts are all made in planes parallel to one another \(typically XY\), but may be made to different depths, any given area will have a flat bottom and the appearance will be like to a ziggurat or stepped pyramid, and may be described as a series of drawings each depicting a different depth or layer \(a series of onionskin drawings each stacked on top of the other\). Usually no more than two axes will move at once. Most programs are able to create files for cutting such shapes, usually by allowing one to define a given region, shape or path, and assigning to it a cut to a particular depth. See [Toolpaths](toolpath-basics.md) section.

**3D** : \(in terms of CAM\) Cuts can be made so as to describe an arbitrary 3D shape \(typically w/o overhangs\), and the cutter will need to move in all possible planes so as to make a smooth finish. On a 3-axis machines overhangs or carving fully in the round will require either special tooling or flip jigs \(or both\). When described as a drawing, a full 3D design would require a continuous variation in darkness, like to a photograph.Typically the domain of specialty CAD and CAM programs, they require the depiction of a form in full 3D, usually using a 3D-specific file format such as .stl. See [Toolpaths](toolpath-basics.md) section.

**6s and 9s** :\(in terms of machining\) Description of the preferred shape of chips cut by the machine.

## A

**A-Axis** :The A Axis usually refers to the first rotational \(the fourth axis when added to a machine with X-, Y- and Z-axes\) of a CNC machine. The fourth axis sits underneath a typical X,Y,Z setup. Picture a stepper or servo controlled lathe on the table of a typical 3 axis machine. That is your 4th axis. It is important to understand the difference between A axis and B axis. The A axis is the rotational axis around the X-axis. While the B axis is the rotational axis around the Y-axis. The picture below demonstrates an A-axis setup. 

**Absolute Coordinates** : Absolute coordinates are expressed relative to a fixed position \(in most cases the homing switches of your machine\). Relative coordinates on the other hand are relative to the current position of the cutting tool \(think zeroing your tool at some arbitrary point on the table\). Helpful Gcodes in this regard are the following:

* G90 command places the part program into absolute coordinate mode.
* G91 cancels absolute coordinate mode and places the controller software into relative coordinate mode.

**Accuracy** : Accuracy in a CNC context, refers to how close an actual measurement is to its true value. A CNC machine will have a mechanical resolution based on its movement systems, and a tolerance for accuracy which will be a function of that, and the material being cut, work-holding, and tool-path strategy. See [Dimensional accuracy](x-y-z-calibration.md) section.

**Accuracy vs Precision** : Accuracy refers to how close an actual measurement is to its true value. For example, a hardened rod is very accurate if it is supposed to be .0001 and it is in fact .0001. That is high accuracy. Precision on the other hand refers to the repeatability of a measurement no matter how accurate it actually is. Using the same example, there is high precision in a line of 50 hardened rods that all measure .0015 even though they are supposed to be .0001. In this case there is high precision but low accuracy. A machine cannot normally produce parts with a tighter tolerance than its accuracy.

 **Allowance** : Difference in dimension of two parts which ideally would meet/match. c.f., ''Clearance'', ''Interference'', ''Tolerance''

**APT Programming** : Automatically Programmed Tools. An early NC programming system, developed for aerospace capable of multi-axis contouring programming.

**Arbor** :An arbor \(American English\) is synonymous with the term mandrel \(also spelled mandril\). An arbor can be either a workholding device or a toolholder. For example you may read in a catalog, drill chuck \(includes R8 arbor\). The arbor here refers to the portion that adapts the spindle of your mill to the actual drill chuck.

**Axis** \(cnc\) :In Geometry there are three axes of movement \(X, Y, Z\). It is therefore confusing at first to hear of a 5 or 6 axis CNC machine. How can there be more than 3 axes? In CNC terminology an axis is either a linear or rotary freedom of movement.

## B

**B-Axis** : Usually the axis of circular motion of the spindle about the Y-Axis.

**Backlash** : Any kind of unexpected play in an axis due to clearance or looseness of mechanical parts. Check for it by moving an axis in one direction, stopping the unit, then noting how much movement is necessary in the opposite direction to begin moving the machine.

**Ball Endmill** : Ball end mills are end mills that have a half spherical bottom \(examples pictured below\). They’re typically employed in 3D profiling \(as opposed to profiling or pocketing. For example, carving out a 3D chess piece\) because they don’t have a flat bottom and hence tend to smooth out the steps. They have some unique cutting problems at the bottom because the cutting speed varies all along the ball. The cutter is moving faster along the areas of the ball with larger diameter. See [Cutters & collets](cutters.md#ballnose-endmills) section.

**Bézier curve** : Bézier curves are a mathematical description of a curve often used in vector editing programs. They allow for freeform shapes which would be quite difficult to draw with rules and a compass.

**Bisect Lines**: Corner over-cuts at 45 degrees which allow square parts to fit into pockets or mortises or slots cut with a round bit.

**Bitmap image** : An image that is made of a set of individual pixels \(as opposed to a vector image\)

**Boolean** : An operation which can be done in most Bézier drawing programs is to combine two \(or more\) shapes, either adding, or subtracting them. Carbide Create affords three such: Union \(combines to create the largest possible area\), Intersection \(leaves only the smallest possible area encompassed\), and Subtraction \(one object is removed from the other\).

**Bounding Box** : A bounding box is used in CAD drawing to get a rough estimate of how much volume a 3D object contains. A bounding box is defined as the smallest box that can be drawn and still contain the object\(s\) you are “bounding”. This is useful in CNC when trying to figure out how large of stock to need to start with in order to mill out the model. 

## C

**C-Axis** : The C Axis is the rotational axis around the Z axis. In and of itself a machine with just C Axis capability would be pretty useless. But if you couple it with B Axis movement you have a 5 axis machine with incredible versatility.

**Center Cutting Endmill** : A center cutting end mill is distinguished from a non center cutting end mill in that it has the ability to plunge the end mill in much the same way one would plunge a drill bit. End mills with coolant passages down the center of the bit are not centercutting and consequently cannot plunge straight down. Non-center cutting end mills can not make plunge cuts but clear chips better & it can be sharpened easier. Center-cutting \(left\) & non-center cutting \(right\) type end mills. Center-cutting end mills can make plunge cuts.

**Centerline Programming** : Centerline programming refers to CNC Gcode written in such a way that the toolpath is programmed along the centerline of the mill cutter \(or the virtual tip in turning\). Since it is rare that we actually want to cut this way, the actual path is generated using the centerline as a reference and calculating the offset via the tool offset and toolnose radius information.

**Chamfer** : An obtuse-angled relief or cut at an edge added for a finished appearance and to break sharp edges.

**Chatter** :Vibration or sound that comes from the machine tool under certain conditions. It interferes with proper cutting and produces cutting errors and bad surface finish. Generally, it is a harmonic vibration or natural resonance. It can be triggered through improper setup or operation of the machine. Frequently, changing the spindle speed, depth of cut, or feed rate can eliminate the chatter. It is generally not advisable to continue cutting with chatter present.

**Chipload or Chip Load** : Chip load is the thickness of a chip \(usually measured in thousandths \(i.e.: 0.010\)\) per tooth that is removed by one cutting edge of the tool \(Chip load is also sometimes measured as feed per tooth\). Chip load is an important factor in tool life because it determines how much heat will be carried away from the cutting edge. Better heat dissipation directly relates to increased tool life which is one of the primary reasons for the use of coolant. See [Feeds & speeds](feeds-and-speeds-basics.md) section.

**Circular Interpolation** : Circular Interpolation is combining the movement of two linear axes into a smooth arced or circular motion. Since an axis can only move linearly, intelligent coordination of axes are required to create an arc. G02 causes the motion to be in a clockwise direction, while G03 is counter-clockwise. Circular interpolation requires an endpoint, a feed rate, a center, a radius, and a direction of movement.

**Clearance** : Space added between two parts intentionally. c.f., ''Allowance'', ''Interference'', ''Tolerance''

**Climb Milling** : A milling cutter can cut in two directions: Conventional and Climb. Climb Milling moves the cutting tool the same direction that it wants to travel. See [Feeds & speeds](feeds-and-speeds-basics.md#climb-milling) section.

**CNC** : Computer Numerical Control --- the use of a computer to calculate and send numbers for positioning \(coordinates, acceleration, speed, &c.\) to a machine. Usually an existing machine is adapted to CNC, so one has CNC mills, CNC routers, &c. a 3D printer is a specialized \(additive\) CNC machine.

**Collet** : A collet is a mechanical device used to hold a tool or workpiece. Some of the more popular collet standards include:

* 5_C Collets \(Invented by Hardinge, commonly used with lathes\)_ 
* ER Collets \(DIN 6499\) developed and patented by Rego-Fix in 1973, is the most widely used clamping system in the world. ER collets are now available from dozens of companies worldwide. The standard sizes are ER-8, ER-11, ER-16, ER-20, ER-25, ER-32, ER-40, ER-50. The "ER" comes from an existent "E" collet which Rego-Fix modified and appended "R" for "Rego-Fix". The number is the cavity opening diameter in millimetres, the outside collet diameter. ER collets contract over a range of 1mm and are available in 1mm or 0.5mm steps, so a range of ER collets can hold any cylindrical shank, metric or imperial. ER collets may also be used on a lathe to hold work pieces. Suffix indicates wrench type
  * A Type is a Hex Nut, you can use a normal spanner wrench
  * UM Type is a Castle Nut, requires a special castle spanner
  * M Type is a Mini Castle Nut, requires a special castle spanner 
  * MS Type is a Slotted Nut, requires a special tapered spanner 
*  _R-8 \(The Bridgeport standard\)_ 
* 16C
* _3C_
* MT \(Morse taper 0-7; DIN 228-1\) 
* _3J_ 
* WW

The collet works by squeezing the tool or workpiece more and more tightly as the clamp is drawn up against a taper to squeeze it shut. This compressing by drawing into a taper is usually accomplished via a drawbar \(really just a long bolt\) pulling the collet into the taper \(5C and R-8 both use drawbars\), or a threaded cap that pushes the shoulder of the collet more deeply into the taper \(ER collets use this method\).

Most Dremels and routers either use a proprietary design, specific to that machine, or the ER style which has become an industry standard \(DIN 6499 as noted above\). 

**Contouring** : Contouring is very simply the process of cutting a smooth continuous curve or surface in CNC. Contouring by definition is impossible to do with manual mills and lathes since it requires the simultaneous movement of two or more axes to perform the operation.

**Control Software** : The typical setup uses a micro-controller to interpret G-Code, while receiving files and commands from a computer system running Communication / Control software.

**Conventional Milling** : A milling cutter can cut in two directions: Conventional and Climb. Conventional milling moves the cutting tool opposite the direction it wants to travel. See [Feeds & speeds](feeds-and-speeds-basics.md#conventional-milling) section 

**Conversational CNC** :Conversational CNC is a type of G-Code programming. There are actually three types of programming methods, manual programming , conversational programming \(which is also called shop floor programming\), and computer aided manufacturing \(CAM\) system programming. Each has it’s place and application. Conversational CNC can most easily be thought of as a G-code “wizard.” For example, one wizard might be for cutting a circular pocket. The wizard asks for critical input data like feedrate, diameter of pocket, step size, diameter of cutting tool, etc and then outputs G-code straight to the control software. Conversational CNC tools are usually best used for one-time machining operations.

**Coordinate Word** : In standard G-code a coordinate word is a section of code that begins with an axis letter followed by a position. For example, Y2.0175 is a coordinate word that may be part of a larger block of code.

**Corner Overcut** : Round endmills are unable to reach fully into corners when doing profile cuts, one solution for this is to overcut the corner with a circle, this may be done by positioning the edge of the circle at the corner, or at a 90 degree angle, termed a dogbone.

**Corner Radius** : The radius by which an otherwise square endmill is rounded.

**Counterbore** : Cylindrical hole to accept hardware.

**Countersink** : Conical hole to accept hardware.

**Counter drill** : Both cylindrical and conical hole

**Crash** : A crash is the unfortunate condition of “crashing” your tooling into a limit switch, workholding or some other unintended obstacle. It can result in broken tooling or broken machines, but on a belt-driven ShapeOko, in the X and Y-axes, normally just results in skipping steps on the belt. 

**Cutting Depth** \(also Cut Depth\) : This is the distance which a given CAM operation will cut into the stock. If greater than Depth of Cut \(DOC\), will be reached after multiple passes are made. See [Toolpaths](toolpath-basics.md) section.

**Cutting Force** : Cutting force refers to the force exerted on the workpiece by the tool. High cutting forces can potentially cause deflection, inaccuracy, chatter, and broken tooling. See [Feeds & speeds](feeds-and-speeds-basics.md#mrr-power-torque-force) section

**Cutting Length** : See '''Flute Length''' below.

**Cutter Radius Compensation** : Cutter radius compensation allows a program to be written without considering the size of the cutter being used. Three G codes are used to control compensation G40, G41 and G42. They are group modal.

* G40 cutter compensation off, centre line programming.
* G41 cutter compensation to the left of the programmed path.
* G42 cutter compensation to the right of the programmed path.

**Cutter Offset** : Cutter offset is the distance from the surface of part to bottom of tool along the z axis. In practice cutter offset is a predetermined distance from the surface of the workpiece that allows for the safe and rapid movement of the cutting tool between cutting operations.

## _D_

**Dado** \(US and Canada\) : A slot \(housing in the UK, trench in Europe\) cut into the surface of a material, normally only used in woodworking where it signifies a groove made across the grain of the wood. Through dadoes pass from one side to the other \(and have only two sides and a bottom\), stopped are closed at one end \(or both\) ends.

**Depth of Cut** \(DOC\) : Depth of Cut \(Depth per Pass in \[\[Carbide Create\]\]\) is how deep the tool is cutting under the surface of the stock material being cut. The depth of cut will determine the height of the chip produced. Typically, the depth of cut will be less than or equal to the diameter of the cutting tool. It takes more power to run a higher depth of cut and so slower feed rates and/or spindle speeds are usually necessary. Axial DOC is the depth specifically, radial DOC is the width of the tool which can be engaged in each pass. See [Feeds & speeds](feeds-and-speeds-basics.md#choosing-doc-and-woc) section.

**Depth Ring** : Plastic collar used to set bit cutting depth.

**Dog Bone** : Corner over-cut at 90 degrees which allows square parts to fit into pockets or mortises or slots cut with a round bit.

**Draft** : Tapered extrude, where the "draft" is the angle of taper.

**Dwell Time** : Timed delay of a set duration added to a control program for specific machining operations.

## E

**E-Stop / Emergency Stop** : An e-stop or emergency stop is the control that stops all machine operation in the event of a crash, runaway machine, or some dangerous or potentially dangerous situation. True emergency stop devices cut power to spindles, drives and any other powered element of a machine so that all sources of potential danger can be eliminated with a button.

**Endmill** : An endmill is a type of cutting tool used in industrial milling applications. It is distinguished from the drill bit, in its application, geometry, and manufacture. While a drill bit can only cut in the axial direction, a milling bit can generally cut in all directions, though some cannot cut axially. Endmills are used in milling applications such as profile milling, tracer milling, face milling, and plunging. See [Cutters & collets](cutters.md) section

**Extrude** : Uses a curve and direction to create a 3D version of the original curve.

## F

**Face Mill or Face Milling** : A face mill is a type of mill cutter that contains multiple cutting teeth and is often used to remove large amounts of material. In face milling, the cutter is mounted on a spindle having an axis of rotation perpendicular to the workpiece surface. The milled surface results from the action of cutting edges located on the periphery and face of the cutter.

**Feed or Feedrate** : The desired rate of movement of the cutting tool into the workpiece for a specific machining operation. See [Feeds & speeds](feeds-and-speeds-basics.md) section

**Finger Joint** : A joint which allows joining two pieces of wood so as to make a larger one. 

**Finishing Pass** : A finishing pass is usually the last pass over a part characterized by higher spindle speeds and a shallower depth of cut in order to improve the finish of a part and increase tolerances. See [Toolpaths](toolpath-basics.md#roughing-vs-finishing-toolpaths) section.

**Firmware** : Typically firmware is loaded onto a micro-controller and interprets G-code into machine motion. Grbl is a common choice.

**Fixture** : A piece of equipment which holds the stock in place. c.f., ''Jig''

**Flutes** : The term flute refers to the groove on the periphery of a cutter that allows for chip flow away from the cut. See [Cutters & collets](cutters.md) section.

**Flute Length** : Length of flutes or grooves. This length will determine how deeply an endmill will actually be able to cut. Note that it is possible to continue to cut deeper, but the shaft of the endmill will rub, generating friction heat which is not recommended \(and a tapered endmill with a shaft thicker than the cutting diameter will require relief cuts to clear for the taper and shaft\).

## G

**G-Code** : A defined set of command instructions and parameters which determine how a machine will move and function. Named for the ''G'' movement commands which are most frequently used and which were the only ones defined when the language was named. Usually indicated by the file extensions .gc, .gcode, .tap, .nc, .ngc, &c. See [CAD, CAM, and G-code](cad-cam-tools.md) section.

**Gantry** : A bridge-like structure that goes across the machine and above the work \(not necessarily moveable -- the bed could move in the Y direction instead as some 3D printers do\).

**Groove** : A slot or trench cut along the grain. Through grooves will pass from one end to the other, stopped will be housed within the material on one \(or both\) ends.

## H

**Homing** : Typically, the use of switches \(homing switches, a subset of limit switches\) to determine the machine origin, usually under the micro-controller’s control. One can also approximate this manually.

## I

**Indexing** : To rotate or move from one position to another in order to perform a set of operations. c.f., ''Tiling''

**Interference** : Intentional overlap of two parts. c.f., ''Allowance'', ''Clearance'', ''Tolerance''

## J

**Jig** : Tool used to control the location of a cutting tool. c.f., ''Fixture''

**Jogging** : The use of special-purpose buttons or commands to move the spindle/carriage/gantry under power into a particular position.

## K

**Kerf** :The empty space left behind in stock after making a cut.

## L

**Lead-In and Lead-Out** : The terms “lead-in” and “lead-out” refer to how a CNC part program approaches and leaves the part when cutting. Most CAM programs have parameters for describing the type of approach and exit strategy that will be employed in cutting a part. These cutting strategies can range from directly plunging and retracting the tool \(no lead-in or lead-out\) to arching toolpaths which “kiss” the origin after a soft lead-in. These cutting strategy decisions vary based on workholding, fixtures, material choices, etc. See [Toolpaths](toolpath-basics.md#lead-in-lead-out) section.

**Limit Switch** : The term limit switch is a generic term for a sensor or switch at the end of an axis which is placed there to trip an emergency stop situation if the axis for some reason travels that far. In a perfect world, limit switches would never be triggered, but in the real world they are critical to avoid machine self destruction. Ideally one would not run a CNC machine without limit switches on both ends of all axes fully operational, but the belt driven design of the Shapeoko allows one some leeway in this, and a crash may result in nothing worse than the belt skipping. Moreover, the current Shapeoko 3 configuration uses homing switches \(see above\) to enable soft limits. Limit switches can be made from microswitches, hall effect sensors or optical switches. Because inputs on CNC machines are often times limited, it is a common practice to tie the switches together so that all limit switches trigger use the same input.

**Length of Cut** : How much of the end mill has a cutting area on it

## M

**Machining Margin** \(MeshCAM\) : The distance outside the geometry outline which MeshCAM will allow the endmill to go.

**Max Depth** : The maximum depth to which the machine will be allowed to cut when setting up cutting in a CAM program.

**MDI \(Manual Digital Interface\)** : An interface area where commands may be directly sent to the machine to control it, or query it for its status.

## N

**NEMA** : industry group which defines the standard for sizing of electrical motors which are used in most CNC machines. The motors are sized in decimal inches by the mount hole spacing and the decimal point omitted. Hence the mounting holes for a NEMA 23 motor are spaced 2.3 inches on center.

**Nesting** : The arrangement of parts to be cut so as to make the most efficient use of material.

## O

**OAL \(Over all length\)** :How long the bit is in total from one end to the other.

**Offset** : Difference between one distance or dimension and another.

## P

**Peck Drilling** : Peck drill is a “canned cycle” drilling operation in which the bit advances into the hole retracts to clear chips and/or flood the hole with coolant and then advances further, retracts, etc. Peck drilling is often used for holes that are three or four times deeper than the drill diameter.

**PDF** : Adobe's Portable Document Format, PDFs are often used for drawing exchange. 

**Pixel** : Short for "picture element", the smallest division of an image into a grid, resulting in the depiction of an image as a series of dots arranged on a grid. Contrast with vector.

**Plunge** : See '''Step Down''' below.

**Plungerate** : How quickly the end mill will be driven into the material --- the feed rate used for the negative Z-axis move. See [Feeds & speeds](feeds-and-speeds-basics.md#plunge-rate) section.

**Pocket Milling** : Pocket milling is an interior recess that is cut into the surface of a workpiece. Pockets may be round, rectangular or an arbitrary shape. 

**Post-Processing/Post-Processor** : An automated process that takes G-Code commands produced by another process as an input and produces a modified output. Post-processors can be found integrated into CAM software, as well as in the form of stand-alone applications or scripts. Post-processing is often used to translate generic G-Code into variants of G-Code that only support a subset of the full command set. e.g. Some controllers do not support the drilling commands, and a post-processor could convert these commands into an equivalent set of G01 Z-axis movements that are supported by the target controller. Another example of post-processing is the modification of feed speeds in the original G-Code commands to scale or limit speeds in the resulting G-Code. 

**Potentiometer** : An adjustable resistor.

**Pre-Processing/Pre-Processor** : An automated process that modifies G-Code commands prior to their consumption by another process \(most often the sending of the commands to a controller\). An example of a common pre-processing operation is the speed limiting supported by many G-Code sending applications.

**Precision** : A measurement of how well things match the desired dimension, c.f., ''Accuracy''.

**Profile** : A 2d cut into the material, following a closed path --- the result will be a cut-out piece and a cut in the stock material and may be either inside \(the path will define the shape left in the stock material\) or outside \(the profile will define the shape cut out of the stock material\). The depth parameter specifies the depth of cut into the material and should be negative. See [Toolpaths](toolpath-basics.md#contour-profile-toolpaths) section.

## Q

**Quill** : The sleeve of a drill press or lathe headstock in which the spindle is mounted.

## R

**Rabbet** : A recess cut into the edge of a material creating a step. The English term is rebate.

**Rack and Pinion** : Drive system used in some CNC systems

**Ramping in** : Rather than plunge cutting to begin a cut in CAM, the machine approaches along a gradual incline. Some programs will continue that angling cut as the machine moves deeper to keep tool engagement consistent. 

**Raster image** : Synonym for pixel image, see above.

**Resolution** : The smallest distance which a machine may move or address, defined by the capabilities of the movement system. e.g., the Nomad has a mechanical resolution per its specifications of 0.0005". The actual movement of the endmill in relation to the stock will vary based on material, work holding and tool-path strategy, yielding the expected accuracy.

**Rest Machining** : Feature in some CAM programs where a successive operation machines the rest of the material which a previous one couldn't get to.

**Retract Height** : How high the Z-axis lifts up to ensure that the bit doesn't damage your part. Also ''Safety Height''.

**Roughing Clearance** : How much material will be left when roughing out material for the current operation as a roughing pass, and instead, will only be removed w/ a second operation, the finishing pass which may have different settings for stepover, feed rate, &c. so as to leave a nicer finish.

**RPM** : The speed at which an endmill is rotating. See [Feeds & speeds](feeds-and-speeds-basics.md#choosing-rpm-and-feedrate) section.

**Runout** : The inaccuracy of a rotating mechanical system, specifically the degree to which a tool does not rotate exactly in line with its axis.

## S

**Safety Height** : How high the Z-axis lifts up to ensure that the bit doesn't damage your part. c.f., Retract Height above.

**Speed** \(Spindle Speed\) : The rotational speed of the endmill mounted in the spindle in revolutions per minute \(RPM\).

**Spindle** : The main rotating axis of a machine, for most hobby CNC machines, this is a trim router or rotary tool \(Dremel is the most familiar brand\). 

**Step Down** : How deeply the pocket will be cut w/ each pass. Also termed "cut depth/depth of cut, q.v." or "depth per pass" or "plunge".

**Stepover** : The offset from one toolpath to the next adjacent toolpath. See [Toolpaths](toolpath-basics.md) section.

**Steps** : The movement of a stepper motor is composed of discrete rotational movements. Each such movement is a step \(or micro-step if that is enabled --- micro-steps sub-divide each step into smaller motions, each level half as small as the one before\). See [Anatomy of a Shapeoko](anatomy-of-a-shapeoko.md#stepper-motors) section.

**STL** : Standard Tessellation Language or Stereo Lithography, a representation of a 3D model using triangle meshes.

**Stock Size** : The size of the material which you are planning on cutting.

**Stock Surface** : The height of the material at which cutting operations will begin. After cutting one layer off, if you have \(a\) successive feature\(s\) w/in a previously cut pocket, you can define a stock surface which matches the bottom of the preceding pocket for efficiency's sake. Please note that not all programs, check on this and if you enter an incorrect value, or get the cutting order wrong when generating G-code, you could crash your machine or break a bit. There are simulation programs which will optimize and check for such.

**Surface Feet per Minute \(SFM\)** : The speed at which the flutes of the endmill move over the material as the machine cuts, based on the endmill diameter and spindle RPM. See [Feeds & speeds](feeds-and-speeds-basics.md) section.

## T

**Tiling** : Cutting out a design or part one section at a time.

**Tolerance** : The degree to which a part may unintentionally vary from the ideal dimension. c.f., ''Allowance'', ''Clearance'', ''Interference''

**Toolpath** : The course which an endmill will follow when performing a desired CAM operation. Carbide Create affords two options: Contour operations \(follow the path in question \(No Offset\), offsetting the spindle as needed \(Outside / Right and Inside / Left\), or so as to cut out the entire area \(Pocket\)\) and V Carve \(these move a V-bit so that the perimeter will be machined up to, increasing or decreasing depth as necessary\). See [Toolpaths](toolpath-basics.md) section.

**Toolpath Zero** : The intended origin point for zero when machining. Options include Lower-left, Center-left, Top-left and Center.

**Tram** : The squareness of your mill head to the table. See [Squaring, surfacing, tramming](squaring.md#tramming-the-router-spindle) section.

**Typical \(or TYP\)** : \(in CAD\) TYP means typical to all identical features, e.g., if there are multiple bolt patterns and only one is dimensioned with "TYP" on all DIMs, then all the bolt patterns are exactly the same.

## U

**USB** : Universal Serial Bus, a standard computer connection used for a wide variety of computer peripherals which is used to connect to the Arduino which up is used to run \[\[Grbl\]\] which functions as a control board for the system.

## V

**Vector** : Colloquially in graphics, the depiction of an image as a series of dots, lines, arcs, circles, Bézier curves, and stroked and filled regions. Contrast with pixel.

**Vice** \(Vise in American English\) : A tool which is attached to a table or bench, using two flat surfaces to hold stock for machining \(or woodworking\) operations, usually tightened with a screw mechanism.

## W

WCS : work coordinate system

## X

**X-Axis** : Gantry machines \(including Shapeoko\) are usually labelled X for the axis along the gantry \(left and right, with right being positive\). X moves just the carriage on the gantry. One may switch the X- and Y- axes if that better suits one working setup, but to avoid confusion, when referring to the physical aspects of the machine, one should use non-switched descriptors. On a lathe, it is the X-axis which determines the diameter of the cutting.

## Y

**Y-Axis** : Gantry machines \(including Shapeoko\) are usually labelled Y for across the gantry \(toward and away from you, with away being positive\). Y moves the whole gantry. One may switch the X- and Y- axes if that better suits one working setup, but to avoid confusion, when referring to the physical aspects of the machine, one should use non-switched descriptors.

## Z

**Zero** : Colloquially, the origin point of the current work coordinate system, or current mounted stock. Often will need to be set via relative off-sets, one axis at a time.

**Z-Axis** : In a typical Cartesian machine, Z is up and down \(up positive\), or, it is the axis around which the spindle revolves, e.g., on a lathe.

