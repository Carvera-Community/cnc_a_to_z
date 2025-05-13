# Dimensional accuracy

For "decorative" projects, some inaccuracy in the dimensions of pieces cut on the Shapeoko does not matter, but many projects require having a finer control of the final dimensions:

* cutting geometrical patterns where a discrepancy between the X and Y dimensions could be visible to the naked eye, _e.g._ round shapes coming out like ellipses and squares like rectangles. Out of the box, on a properly assembled/squared/tensioned Shapeoko, this X/Y dimensional discrepancy should be small enough to not be noticeable.
* cutting pieces that are intended to fit into each other \(_e.g._ hole & peg, box & lid, ...\). This is where one typically starts noticing that dimensions are not _quite_ what they were supposed to be, and are inaccurate enough that pieces either do not fit, or require manual rework \(_e.g._ sanding\) to be assembled.
* cutting pieces that are supposed to fit, from a hard material \(hard plastics and metals\). This is where the accuracy problem becomes obvious, as there is no elasticity in the material to compensate for small inacurracies.
* and then obviously, projects where the whole point is to cut pieces of specific dimensions within a controlled tolerance \(_e.g._ mechanical parts\)

Many factors are involved in getting good dimensional accuracy:

* intrinsic mechanical accuracy of the machine \(that may drift over time, too\)
* _actual_ dimensions of the tool being used
* rigidity of the workholding solution
* dynamic effects during the cut, mainly tool deflection, depending on the tool, material,  toolpaths, feeds & speeds.

If the Shapeoko forum is any indication, many people \(including myself\) have gone through these phases:

1\) starting to care \(then obsess\) about tweaking/calibrating/tuning the machine to improve dimensional accuracy

2\) chasing their own tail trying to squeeze these last few % of additional precision

3\) realizing that there are so many factors involved, that a much more efficient approach is to just cut a piece, measure it, adjust the CAD and/or CAM to compensate, rinse and repeat until the result is perfect.

Yet, I still think it is important \(or at least interesting\) to know about sources of inaccuracy to decide what to do \(or not to do\) about each of them. This section will only scratch the surface on this topic, but hopefully provide useful insights on the matter.

{% hint style="info" %}
The accuracy that is possible to achieve on a Shapeoko with the proper mix of calibration and CAD & CAM iterations, seems to be typically around 0.002'' / 0.05mm, with some achieving down to 0.0002'' / 0.005mm
{% endhint %}

## X/Y/Z calibration

The Shapeoko belts and pulleys are such that it takes \(very close to\) 40 motor steps to move by 1 mm. But the tensioning of the belts induces some stretch, such that in reality when the stepper motor does 40 steps, the actual resulting movement on the associated axis will be slightly more or slightly less than 1mm. 

The GRBL software in the Shapeoko controller stores a "steps per mm" adjustable parameter for each axis, and uses it to convert a G-code command that requests a displacement expressed in distance units \(inches or mm\), to a number of steps to be generated on the stepper motor.

These parameters are stored as $100, $101, and $102 for X, Y and Z axis respectively.

By measuring by how much a given machine _actually_ moves for a given commanded distance,  one can adjust these parameters, i.e. calibrate/compensate for belt stretch. 

The basic principle is to: 

* make a note of the current value of $100, $101, and $102 parameters.
* design a piece of given dimensions in your CAD tool.
* cut the piece on the machine, and measure its actual X and Y and Z dimensions with a caliper.
* adjust the $100, $101, and $102 values accordingly:

$$
new\ parameter\ value =  \frac{expected\ dimension}{actual\ dimension}× old\ parameter\ value
$$

* cut the piece again: it should now be very close to the expected dimensions

In theory, cutting a cube would be ideal to support the XYZ measurements. In practice, the Z measurement is much easier to do inside a pocket, while for the X and Y dimensions there is a catch: they include at least two sources of errors that are actually not coming from the machine:

* **tool deflection** during the cut: this is influenced by the tool diameter and stickout, the material, the feeds & speeds, the DOC and WOC. If tool deflection is significant, the piece will be larger or smaller than expected due to this, not due to belt stretch.
* **tool diameter** uncertainty and **tool runout**: the endmill used to cut the test piece is never _exactly_ the diameter it is supposed to be, and there is probably a little runout too, but the CAM tool generated a toolpath based on the theoretical value of the tool diameter and zero runout, so the piece dimension will be slighty off due to this too. 

These errors should not be included/compensated for in the X/Y calibration, since they will change when a different set of conditions are used \(different endmill, toolpaths,...\)

A simple way to address these contraints is to:

* cut three deep pockets \(two aligned along X, two aligned along Y\)
* measure the border to border distances, and the pocket depth:

![](.gitbook/assets/xyzcalibration_pockets.png)

The border-to-border measurement for X and Y will cancel out the endmill diameter/runout/deflection errors from the measurement, and the pocket depth can also easily be measured with a caliper to adjust $102.

The downside of this method is that measuring border-to-border distance with a ruler will not be extremely accurate, but if the distance between pockets is large enough, it should still grant good results.

Another popular method is to rather measure the gantry movements themselves, instead of measuring the dimensions of the resulting cuts. One can use:

* a **dial indicator**
  * attached to the bed of the machine and oriented along X then Y axis, to measure how much the gantry moves versus what was commanded.
  * attached _e.g._ to the router and oriented along the Z axis, to measure how much the Z plate moves versus what was commanded.

![](.gitbook/assets/xyzcalibration_dialindicator.png)

* a \(long\) **caliper**, oriented along X then Y axis, with the fixed part attached to the bed, then pushing the moving part with the shank of the endmill
* a **DRO** \(digital readout scale\) can be used to do the same, over a longer distance to minimize the error.

{% hint style="info" %}
The axis of the measurement tool should be aligned carefully to match the axis of the machine, any angle between the two would reduce the accuracy of the measurement.
{% endhint %}

{% hint style="info" %}
If you tensioned the belts evenly using the frequency measurement method mentioned in [Squaring, surfacing, tramming](squaring.md#squaring-the-machine-belt-tension-part), chances are that the calibrated number of steps per mm on X and Y will be very close, and may even not require calibration at all, which is a desirable thing since using the round default value of 40 steps per mm minimizes rounding errors in GRBL's computations.
{% endhint %}

### Limitations of belt calibration

The pitfall of belt calibration is expecting to get excellent accuracy out of it alone. Unfortunately, belt stretch is not fully linear, and many users found out that the calibration values varies depending on the calibration distance, and where on the work area the measurement is made.

It could even be considered to be counter-productive, because of the finite precision of GRBL computations that will round numbers with lots of decimal places, and the rounding effect may be worse with calibrated values than with the default 40steps/mm value.

As mentioned earlier, another approach to dimensional accuracy is simply to do test cuts, measure the result, adjust the design and toolpaths, and iterate. In this case, leaving the steps per mm at their default value is arguably a good option.

I chose a middle ground: calibrate X/Y/Z to be in the right ballpark on the first try \(which is enough for many projects\), and then do a second round after compensating the error in the design/toolpaths when this is required for a given project. 

{% hint style="info" %}
The "stock to leave" option in Fusion360 is a great way to deal with this: you can initially use a positive stock to leave value to have margin, run the job and measure, then adjust stock to leave accordingly, relaunch the cut, and measure again. You can even specify negative stock to leave values, to cut beyond the profiles defined in the design \(_e.g._ to cut a hole slightly larger than it was designed to be\). All of this assuming of course that you will not cutout/remove the piece from the stock before you are done with these iterative cuts.
{% endhint %}

## Managing tool deflection

Beyond being bad for tool life \(and quality of the cut in general\), tool deflection is bad for dimensional accuracy. To minimize it, and depending on the situation, one can:

* use a larger \(=stiffer\) endmill.
* use an endmill with large shank diameter \(stiffer, again\).
* use an endmill with a long shank and short cutting part, if depth of cut allows.
* minimize tool stickout \(when possible\)
* use conventional milling cut direction
* minimize lateral forces on the tool, which usually means taking a lighter cut \(_e.g._ small DOC and or WOC\).

This last point is best adressed by the "**roughing + finishing**" toolpath strategy: the roughing toolpath can take aggressive cuts, leaving enough margin \(stock to leave\) that deflection effects will be included in this margin. And then the finishing toolpath will only have to shave off a thin layer of material, which takes very little force and therefore causes very little deflection.

For smaller endmills, deflection becomes a critical issue that can easily lead to breaking tools, a deflection-aware feeds & speeds calculator is a good way to check before cutting.

{% hint style="info" %}
HSS endmills are more flexible than carbide endmills, so they will withstand a greater amount of deflection before breaking.
{% endhint %}

## Managing runout

### Why bother?

Runout was defined in the [Cutters & collets](cutters.md#runout-tir) section. Since the toolpaths in the CAM tool are based on the endmill diameter, and since runout artificially increases the effective cutting diameter, the parts will have a dimensional error of \(at least\) the runout value.

Runout matters a lot for projects using small endmills, i.e. anything smaller than 1/8''. The reason is that the endmill will see +/- 1 runout of variation on the chipload. Since runout can be of the same order of magnitude as the target chipload, a small endmill seeing a chipload twice as high as the optimal target value can easily break.

Runout is bad anyway for tool life, chatter, and surface finish.

### **Measuring runout**

The simplest way to measure runout is to do a test cut of a slot, measure the actual width of the resulting slot, then subtract the actual endmill diameter: the difference is the runout value. Unfortunately, it is not straightforward to determine precisely the actual diameter of a given endmill: they are manufactured within given tolerances, and are never quite the diameter they are sold to be. A 1/4'' endmill could typically have an actual diameter anywhere between 0.245'' and 0.255''. Measuring precisely the actual diameter is not easy, as the measurement must be made on the flutes, and without proper tooling that can be difficult especially for endmill with an odd number of flutes. There is also a real risk of damaging \(chipping\) the flutes during this measurement.

Another way to measure runout is to use a **test indicator**, capturing the minimum and maximum values read over a 360° revolution of the endmill. Ideally, the runout should be measured at the tip of the endmill, where the cutting action is, but that means measuring on the flutes themselves. Alternatively, the dial indicator can be positioned on the shaft, as close as possible to the flutes, this will give a continuous readout of the runout over a 360° revolution, but could under-estimate the actual runout a bit.

**Runout** is the difference between the maximal and minimal values read, at a given measurement point.

**TIR** \(Total Indicated Runout\) is the range of runout values if we were to measure across the full length of the endmill, so it boils down to the max runout.

Runout measured on the endmill is the result of the combination of the router runout, collet runout, and endmill runout.

Router runout itself can be measured by positioning the test indicator inside the collet taper:

![](.gitbook/assets/runout_router.png)

In this case, the measured taper runout was approximately 0.02mm \(0.0008''\). 

But what really matters is total runout at the tip of the endmill. Below is an illustration of measuring runout at the tip of a 3-flute 1/4'' square endmill. First, the test indicator is positioned at a point where no flutes touch it, and zero is set there:

![](.gitbook/assets/runout_flute0.png)

then the endmill is rotated manually until the test indicator readout maxes out at the tip of the first flute. In this case it read 0.41mm:

![](.gitbook/assets/runout_flute1.png)

the endmill is then rotated further and the deviation at the tip of the next flute is measured, in this case 0.37mm:

![](.gitbook/assets/runout_flute2.png)

ditto for the last flute, which read 0.42mm in this setup:

![](.gitbook/assets/runout_flute3.png)

So the max difference is 0.42 - 0.37 = **0.05mm runout**

As a comparison, measuring runout on the shaft after setting the indicator zero to the point giving the minimal readout,

![](.gitbook/assets/runout_shaft_min.png)

gave a max value of about 0.045mm:

![](.gitbook/assets/runout_shaft_max.png)

Not too far from the 0.05mm measured on the flutes, and easier to measure there.

This example value of 0.05mm \(0.002''\) runout on a Makita router with the stock Makita collet and a 1/4'' endmill is not great, but not really a concern in day to day use. It will vary significantly depending on which collet and endmill are used and how they were fastened. Another combination of collet/endmill on this same router granted a runout of only 0.02mm \(~0.0008''\).

### Reducing runout

* there is nothing one can do about the intrinsic runout of a specific router \(short of returning the router to get a new one, that _could_ have a lower runout, but that is random\)
  * **Spindles** usually have lower runout than routers, so upgrading to a spindle can be another approach \(runout alone may not be a good enough reason to upgrade though\)
* using **precision collets** is a simple way to help minimizing runout, and is one of the cheapest "upgrades" to the stock Shapeoko setup.
* using quality endmills helps, but the endmill is probably not the main source of runout anyway.
* anything which comes between the bore and the collet will prevent correct alignment and cause/increase runout: always make sure the taper and collet are **clean** when inserting a new tool.
* minimize **tool stickout**: this will minimize the effect of the axial runout.
* considering that the final runout results from the relative positioning of the router, collet, and endmill, one can try to "**clock**" these three elements, so that the different runout contributions cancel each other to some extent. There is no easy way to determine runout of each element separately, so this can be a trial & error process, but worth trying:  slightly turn the collet inside the router taper, and/or turn the endmill inside the collet, and this _might_ yield a lower overall runout.
* finally, a method that looks scary but gives surprisingly good results is **tapping** \(gently!\) on the endmill shaft using a screwdriver \(or ideally a brass drift punch\) and a mallet: 
  * first find the orientation of the endmill that gives the maximum readout on a dial indicator placed on the front side.
  * if using a screwdriver put a small piece of electrical tape on its tip, to avoid metal to metal contact.
  * then..._lightly_ tap the back of the screwdriver with a mallet a couple of times.
  * check runout and repeat if necessary. It takes a few tries to find the right amount of force. In this example, the original runout was around 0.002'', and tapping the endmill reduced it to around 0.0006''. This adjustment should hold, at least until the end of the current job with this endmill.

![](.gitbook/assets/runout_tapping.png)

### Managing residual runout

Even if it can be minimized to some extent, some runout will always exist. A simple way to deal with it is to make the CAM tool believe that you are using a slightly larger endmill, corresponding to the actual endmill diameter + runout, measure this **effective cutting diameter** on a scrap piece of material, and then use that effective diameter in the CAM tool instead of the theoretical diameter.

This won't do anything about the negative aspects of runout such as possible vibrations/chatter/poor surface finish, but at least it should give more accurate dimensions.

## Multi-tool jobs

When running jobs involving tool changes, an additional constraint impacting dimensional accuracy appears: position errors from readjusting the zero reference each time a tool change is required \(usually, resetting Z0 to compensate for the tool length difference\).

There are at least two things you can do to minimize those errors: 

* do not turn-off/re-home the machine between tool changes if you can avoid it. While homing establishes a known absolute position, it comes with a finite repeatability performance; upon returning to zero after homing, you may notice that it is not PERFECTLY in the same location as before.

{% hint style="info" %}
You might argue that homing provides the benefit of clearing the effect of lost/skipped motor steps. But if you lose steps in the first place, there is no chance that dimensional accuracy will be good, so you should address that first \(using less aggressive settings\)
{% endhint %}

* use a [tool length probe](upgrading-the-machine.md#tool-length-offset-probe) to automate the adjustment of the Z0 between runs. Sure, a tool length probe comes with its own accuracy/repeatability limitations, but chances are that it will still provide a more repeatable result than manual re-zeroing. And once things are repeatable, it's easy to adjust the CAM design to compensate for residual dimensional errors.

