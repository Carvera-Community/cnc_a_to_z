# Feeds & speeds

The whole "feeds & speeds" topic is arguably the most daunting part of learning CNC.

"**Feeds**" is feedrate, on some CNCs with a fixed tool and moving plate this is the speed at which the material is fed into the cutter, on a Shapeoko this is the speed of the gantry pushing the cutter into the material.

"**Speeds**" is the rotation speed of the endmill, i.e. RPM value.

"Feeds" and "Speeds" go hand in hand, what really matters is the _combination_ of feedrate and RPM values for a given situation. Actually, they are also somewhat coupled with a number of other parameters \(_e.g._ depth and width of cut\), so "feeds and speeds" is often short for "all the cutting parameters". 

When first starting CNC, selecting adequate cutting parameters feels a little bit like this:

![](.gitbook/assets/page_63_800.png)

Using proper feeds and speeds and depth/width of cut values is important to :

* get a good quality of the cut \(_e.g._ surface finish, dimensional accuracy\)
* increase tool life \(i.e. keep tool wear to a minimum\), or at least avoid tool breakage.
* avoid/minimize chatter \(the horrendous sound heard when the endmill/machine vibrates while cutting through the material\)
* optimize material removal rate \(_e.g._ how long it takes to complete the cut\)

While there is definitely a good amount of experience \(and experimentation\) involved in finding the perfect feeds and speeds for any given situation, there are a few underlying principles that are worth understanding for two reasons:

* to figure out reasonable values to **start** from, when a new situation shows up for which you cannot find any predefined recommended values.
* to be in a position to understand how to **tune** the cutting parameters to achieve the desired result.

{% hint style="info" %}
This section includes a little math \(nothing too fancy\), but not to worry: while it is important to understand the _dependencies_ between the cutting parameters, calculators will take care of all those computations for you.
{% endhint %}

Before diving into the relation between feedrate, RPM, and the other parameters, let's check how the tool cuts into the material.

## Conventional milling

In so-called "conventional" milling, the direction of the endmill movement is such that the cutting edges bite from the inside to the outside of the material. In the sketch below, imagine the blue triangle represents one cutting edge of the endmill. If the blue cross is the position of the center of the endmill when this cutting edge starts biting into the material, and if the endmill is moving into the material at feedrate F, then a little bit later the endmill center is at the position of the purple cross, and the cutting tooth has rotated and gone out of the material. The resulting chip of material that was cut during that time is the green part.

It starts out very thin, and gradually increases in thickness. The maximum thickness \(noted "C" below\) happens when the cutting edge exits the material. It is typically called the "feed per tooth" or "chipload per tooth", or usually just "**chipload**", and this is the cornerstone of feeds and speeds.

![](.gitbook/assets/page_65_800.png)

## Climb milling

"Climb" milling is when the direction of the endmill movement is such that the cutting edges bite from the outside to the inside of the material. In that situation, a cutting edge first bites a large chunk of material \(blue position\), and as the endmill rotates and moves to the right at feedrate F, the cut gets thinner, until the tooth has nothing left to cut \(purple position\). The resulting chip \(in green\) has a similar shape to that in conventional milling, and again the max thickness of the chip is the chipload.

![](.gitbook/assets/page_66_800.png)

## Chiploads

So why does chipload matter so much ? 

A large chipload requires a lot of router torque and machine rigidity, and each endmill has recommended chipload limits from the manufacturer anyway \(i.e. there's always a limit to the size of the bite you can take, whether you're a squirrel or a white shark\).

A too small chipload is actually worse: since the cutting edges are not infinitely sharp, at some point instead of slicing into the material, the cutting edges will mostly rub against the surface, and then "heat happens" and this is very bad for the quality of the cut and for tool life. In CNC, rubbing is the enemy.

So this is a Goldilocks situation: the chipload must be high enough to avoid rubbing and overheating of the endmill, and small enough to be within the torque/rigidity limits of the machine and the endmill's rated maximums.

Each flute contributes in turn to removing material during one revolution of an endmill. If we sketched N successive bites that the endmill takes into the material, it would look something like this:

![](.gitbook/assets/page_67_800.png)

If the endmill has _N_ flutes, one revolution will cut _N_ chips, i.e. a length of _N_ × _chipload_ of material. Since the endmill revolves at _RPM_ turns per minute, in one minute a length of _N_ × _chipload_ × _RPM_ will have been cut. And the distance being cut per minute is exactly the definition of feedrate, therefore _Feedrate_ = _N_ × _RPM_ × _Chipload_, which also means:

$$
Chipload = \frac{Feedrate}{Nb\_Flutes × RPM}
$$

This relation is quite intuitive:

* for a given endmill and RPM, the faster the feedrate the larger the chipload.
* for a given feedrate and RPM, an endmill with more flutes will cut thinner chips.
* for a given feedrate and endmill, the faster the endmill rotates the thinner each chip will be. 

The important takeway here, is that there are many possible combinations of feedrate, endmill type, and RPM to reach a given chipload. Say you are using a feedrate of 1000mm/min \(39ipm\), and a 3-flute endmill at 10,000RPM. Given the formula, you may just as well use a 2-flute endmill at 15,000RPM, or keep the 3-flute endmill but spin it at 20,000RPM while increasing feedrate to 2000mm/min \(79ipm\), the chipload thickness would be the same:

$$
\frac{1000}{3 × 10,000} =  \frac{1000}{2 × 15,000} =  \frac{2000}{3 × 20,000} =0.033mm
$$

{% hint style="info" %}
This also means that if your CAM tool comes up with feedrates or RPMs that are not in the range of your machine's abilities \(_e.g._, recommended RPM lower than the minimum RPM of your router\), you can just scale both RPM **and** feedrate values by any factor, and it will still provide the same chipload. 
{% endhint %}

For a given chipload, some combinations are still better than other mathematically-equivalent ones though \(more on this below\). Since the feedrate/RPM combination is derived from the desired chipload value, let's first have a look at what the range of acceptable chipload values is for the Shapeoko.

## Shapeoko chiploads guideline

For the **minimum** chipload value to avoid rubbing, there is a large consensus in the CNC community that a value of **0.001''** \(0.0254mm\) is a good absolute lower limit guideline, at least for 1/4'' endmills and larger. It may need to be lowered to 0.0005'' for 1/8'' and smaller endmills.

{% hint style="info" %}
This is assuming you are using a **sharp** cutter. You should never use a dull cutter anyway, if you do you may end up rubbing even at this 0.001'' chipload.
{% endhint %}

The **maximum** reachable chipload depends on a lot of things, but mostly:

* the **hardness of the material** being cut
* the **type and diameter of the endmill** \(smaller teeth need to take smaller bites: the maximum chipload for a given endmill scales linearly with its diameter\)
* the **toolpath** used \(how wide/deep the cutter is engaged\) and the **rigidity of the machine**: it is quite easy to forget that the Shapeoko is not as rigid as industrial CNCs, so endmill manufacturers recommendations may not be directly suitable for the Shapeoko. Any mechanical mod of the machine also impacts the max chipload capability.

The Shapeoko's limits must also be accounted for: the absolute maximum theoretical chipload on a stock Shapeoko would be reached when using a single-flute endmill at the lowest RPM \(10,000RPM on the Makita router\) and at the fastest feedrate of 200 inch per minute, and that would be 200/\(1×10,000\) = 0.02'' = 0.5mm

So all chiploads should be somewhere between 0.001'' and 0.02''.

{% hint style="info" %}
Technically, the feedrate can go beyond 200ipm, if the associated GRBL limits parameters are set to a higher limit. In practice, feedrates above 200ipm are used for rapids only, there are very little usecases where actually cutting at higher than 200ipm would be useful. 
{% endhint %}

The following is an \(arguable\) table I am using as a personal reference, which I derived from analysis of a large number of feeds and speeds settings shared in the Shapeoko community, as well as my own experimentations. Do not take it for granted, start above 0.001'' and increase it incrementally \(by keeping RPM constant and increasing feedrate\) to find the limits for your machine and for a given material. Higher chiploads are definitely possible \(but may not be desirable\)

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">Soft plastics</th>
      <th style="text-align:left">Soft wood &amp; hard plastics</th>
      <th style="text-align:left">Hard wood &amp; metals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">1/16&apos;&apos;</td>
      <td style="text-align:left">
        <p>0.002&apos;&apos;&#x2013;0.003&apos;&apos;/</p>
        <p>0.05mm&#x2013;0.075mm</p>
      </td>
      <td style="text-align:left">
        <p>0.001&apos;&apos;&#x2013;0.0015&apos;&apos; /</p>
        <p>0.025mm&#x2013;0.04mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0005&apos;&apos;&#x2013;0.001&apos;&apos;/</p>
        <p>0.013mm&#x2013;0.025mm</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">1/8&apos;&apos;</td>
      <td style="text-align:left">
        <p>0.002&apos;&apos;&#x2013;0.005&apos;&apos; /</p>
        <p>0.05mm&#x2013;0.13mm</p>
      </td>
      <td style="text-align:left">
        <p>0.001&apos;&apos;&#x2013;0.0025&apos;&apos; /</p>
        <p>0.025mm&#x2013;0.063mm</p>
      </td>
      <td style="text-align:left">
        <p>0.0005&apos;&apos;&#x2013;0.001&apos;&apos;/</p>
        <p>0.013mm&#x2013;0.025mm</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">1/4&apos;&apos;</td>
      <td style="text-align:left">
        <p>0.004&apos;&apos;&#x2013;0.01&apos;&apos; /</p>
        <p>0.1mm&#x2013;0.254mm</p>
      </td>
      <td style="text-align:left">
        <p>0.001&apos;&apos;&#x2013;0.005&apos;&apos; /</p>
        <p>0.025mm&#x2013;0.127 mm</p>
      </td>
      <td style="text-align:left">
        <p>0.001&apos;&apos;&#x2013;0.002&apos;&apos;/</p>
        <p>0.025mm&#x2013;0.05mm</p>
      </td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
To keep this guideline table simple, I chose to only divide woods into "soft" and "hard" categories, and this labeling is not the correct definition either \(which relates to whether the tree _seeds_ have a hard or soft shell\). I mean it in the "wood hardness" way, and there is a useful **Janka** scale that measures that. @Hooby on the forum consolidated a nice list of Janka hardness values for many types of wood, which I include here for reference. The Janka threshold for "hard" vs. "soft" is highly debatable, but a value of 1000 seems reasonable to steer the chipload selection.
{% endhint %}

{% file src=".gitbook/assets/wood\_hardness-for-cnc.xlsx" caption="@Hooby\'s Janka wood hardness list" %}

Now we have to take a little detour and talk about stepover, because it impacts the _effective_ chipload.

## Stepover/WOC/RDOC

"**Stepover**" refers to the offset distance of the endmill axis between one cutting pass and the next one, which also translates into how much new material is being removed by the endmill, or how much radial engagement is put on the endmill. It's also called Width of Cut \(**WOC**\) or Radial Depth of Cut \(**RDOC**\)

In the example below, the stepover S is 50% of the endmill diameter:

![](.gitbook/assets/page_71_800.png)

The larger the stepover, the larger the force on the endmill. Cutting passes with a small stepover are better for surface finish quality, while passes with large stepover obviously reduce overall cutting time since fewer passes are required to cut a given amount of material. And then **depth** of cut will also come in the picture \(more on this later\).

As a side note, for ball endmills, stepover value influences surface finish quite a lot. Consider the following sketch of a side view showing multiple passes:

![](.gitbook/assets/page_72_800.png)

Due to the geometry of the endmill tip, scallops of residual material will be left at regular intervals on the bottom surface. These will be more or less visible depending on how well the material can hold small details \(a 20% to 33% stepover should be small enough for wood, while it could need to be lowered down to 10% stepover for metal\)

## Chip thinning

The chipload values discussed earlier assumed that the stepover is at least 50% of the endmill diameter:

![](.gitbook/assets/page_73_800.png)

Now consider what happens if the stepover is lower than 50% of the diameter, say 20% only:

![](.gitbook/assets/page_74_800.png)

For the same RPM and feedrate, the _actual_ chip is smaller, its maximum thickness is smaller than targeted, so there is again a risk of rubbing, or at least of sub-optimal heat removal.

The solution is to artificially target a higher chipload value \(all other parameters staying the same\), such that the actual size of the chip is increased to approximately what it _would_ have been if the cutter were engaged at 50%:

![](.gitbook/assets/page_75_800.png)



The formula to determine this higher, adjusted target chipload is:

$$
Chipload(adjusted) = \frac{Diameter}{2 × \sqrt{(Diameter × Stepover) - Stepover^2}}× Chipload
$$

{% hint style="info" %}
For basic toolpaths, the stepover is often in the 40% to 50% range, and then you can just ignore chip thinning altogether. Where chip thinning really matters is for adaptive clearing toolpaths, that typically use small stepovers \(more on this in the [Toolpaths](toolpath-basics.md#adaptive-clearing-toolpaths) section\)
{% endhint %}

{% hint style="info" %}
If we wanted to be pedantic, the term _chipload_ should be used for the case where there is no chip thinning, while the term _chip thickness_ should be used to name the adjusted/effective chipload after chip thinning is taken into account.
{% endhint %}

## Choosing RPM and Feedrate

At this stage, the material is known, the endmill geometry is known, chip thinning is accounted for, which gave us an adjusted target minimal chipload. The remaining part is to chose a specific combination of RPM and feedrate values that together will produce this chipload, following the formula described earlier.

In theory, there are two options: selecting a feedrate value and solving for the associated required RPM value, or selecting an RPM value and solving for the associated feedrate.

In practice, the latter is done. The main reason is that the traditional way to determine feeds and speeds \(especially when cutting metal\) is to start from the required **SFM** \(Surface Feet Per Minute\): this is the linear speed of the edge of the cutter, and it should be within a certain range depending on the material and the endmill. And to achieve a given SFM for a given endmill diameter, only the RPM needs to be determined:

$$
RPM = \frac{SFM(in \ feet\ per\ min)}{0.262 × endmill\_diameter(in\ inches)}
$$

In practice, for most of the materials cut on a Shapeoko, there is a wide range of acceptable SFMs, so RPM could initially be chosen pretty much anywhere within the router's RPM limits \(10k to 30k for the Makita/Carbide router, 16k to 27k for the Dewalt router, and typically a few hundred to several tens of kRPM for spindles\)

* Low RPMs are quieter \(significantly so with a router\), but induce higher forces on the cutter \(more on this later\)
* High RPMs induce lower cutting forces and generally provide better finish quality, but will also require higher associated feedrates to maintain a correct chipload: feeding faster can be a little scary at first, and leaves less time to react should anything go wrong.

A rule of thumb is therefore to set RPM to "**the maximum value you can tolerate and feel comfortable using**", and then determine the associated feedrate to get the right chipload.

Example:

* Material is hard wood and endmill is a 3-flute 1/4'' =&gt; the chipload table recommends up to 0.002''
* Say we use a 12.5% stepover, i.e. 12.5% of 1/4'' = 0.03125'', so adjusted chipload is: 

$$
\frac{0.25}{2 × \sqrt{(0.25 × 0.03125) - 0.03125^2}}× 0.002 \approx 1.5×0.002 = 0.003''
$$

* The ideal setting would be to max out the RPM, say 24,000 \(to take an example that is reachable on the Makita, DeWalt, and common spindles\). The required feedrate would then be :

$$
Feedrate = Chipload×Nb\_Flutes × RPM= 0.003 × 3 × 24000 = 216\ ipm
$$

* That is above the default capability of the Shapeoko \(200ipm\), it would be scarily fast for cutting hard wood, and 24,000 RPM may sound too loud to your taste anyway. Let's say we decided to go for 16,000 RPM instead,  the required feedrate would become:

$$
0.003 × 3 × 16000 = 144\ ipm
$$

* If going 144ipm still _feels_ a little fast, it is possible to obtain the same chipload at lower RPM and lower feedrate, _e.g._ 12,000RPM and 108ipm, at the expense of higher cutting forces \(which or may not be a problem, see power analysis section later below\)
* Alternately it is also possible to lower the feedrate by targetting a smaller chipload while ensuring it is still at least at the minimum recommended value of 0.001'', and assuming you are using a sharp enough cutter: 
  * To get a 0.001'' effective target chipload, the adjusted target chipload would become 0.0015''
  * the feedrate would then be 0.0015 × 3 × 16,000 = 72ipm 

{% hint style="info" %}
Some usecases call for the use of an O-flute endmills: this will probably mean reducing the feedrate and/or increasing the RPM to maintain a proper chipload.
{% endhint %}

## Choosing DOC & WOC

**Depth of Cut** \(DOC\) a.k.a. Axial Depth of Cut \(ADOC\) a.k.a. Depth Per Pass, is how deep into the material the endmill will cut, along the Z axis. For a given feedrate and RPM, the deeper it is the larger the forces on the endmill.

Multiple cutting passes at depth of cut _d_ will be required to cut down to a total pocket depth of _D_:

![](.gitbook/assets/page_79_800_redo.png)

DOC is just as important as feeds & speeds to achieve a good cut, yet surprisingly there is much less information about how to determine its value, compared to the abundance of feeds and speeds charts.

The reason is probably that while there are mathematical recipes to choose feedrate and RPM for a given endmill geometry, the achievable DOC is much more tightly linked to the specific machine you are using, and specifically its rigidity and power.

The Shapeoko is not as rigid nor as powerful as pro CNC machines, so DOC recommendations for these machines need to be dialed back, even when using perfect feeds and speeds.

There is a strong dependency between DOC and WOC: since cutting forces increase with both DOC and WOC, you cannot cut very deep while using a very large stepover, that would put too much effort on the endmill. So the two choices are:

* large WOC but small DOC
* small WOC but high DOC

These two situations are illustrated below:

![](.gitbook/assets/page_80_800_redo.png)

The _**small WOC, high DOC**_ approach is much preferable, as it spreads the heat and tool wear much more evenly along the length of the endmill. However, it requires specific toolpath strategies \(_e.g._ to initially clear material down to the required depth, to allow small WOC to be used for the rest of the cut\), this is covered in the [Toolpaths](toolpath-basics.md) section. This is a very popular approach when cutting metals on the Shapeoko, but its benefits apply to other materials too.

The _**large WOC, small DOC**_ approach only ever uses the tip of the endmill, so that part will wear out quickly while the rest of the endmill length of cut remains unused. But it is still a very common approach for pocketing and profile cuts on the Shapeoko, and it has simplicity going for it.

Unlike chiploads that NEED to be in a specific range to get good cuts, the situation is easier for DOC and WOC: you can just start with small, conservative values and then increase them to find the limit for your machine/endmill/material combination.

For the "wide and shallow" cut scenario \(large WOC, small DOC\), I like to start in this ballpark:

For DOC:

* **5% to 10%** of the endmill diameter for metals _e.g._ **aluminium**
* **10% to 50%** of the endmill diameter for **softer materials**

For WOC:

* **40% to 100%** of the diameter of the endmill for roughing
* **5% to 10%** for finishing passes

{% hint style="info" %}
Don't go below 5% DOC, or you may get rubbing just like when chipload is too low
{% endhint %}

For the "narrow and deep" cut scenario \(small WOC, large DOC\), I like to use these guidelines:

For DOC:

* **100% to 300%** of the endmill diameter

For WOC:

* **10% to 25%** stepover for roughing 
* **5% to 10%** for finishing passes
* possibly even less for the hardest materials \(_e.g._, 4% for steel\)

{% hint style="info" %}
If you go for narrow and deep \(and you should!\), given the small WOC values you will definitely need to take chip thinning into account.  Also, check out **adaptive clearing** in the [Toolpaths](toolpath-basics.md) section, that goes hand in hand with high DOC and small WOC.
{% endhint %}

The figures above provide a ballpark for DOC and WOC, taking into account two specific cases: slotting, and corners. 

## Slotting

Depending on the stepover, the portion ****of the endmill that will be engaged in the material, a.k.a. the Tool Engagement Angle \(**TEA**\), will be different:

For a 50% stepover, the TEA will be 90°:

![](.gitbook/assets/page_83_800.png)

For a smaller stepover, say 25%, the TEA will be reduced \(in this case to 60°\):

![](.gitbook/assets/page_84_800.png)

**Slotting** is a different story: half of the endmill is engaged at all times, so the TEA is 180°:

![](.gitbook/assets/page_85_800.png)

The force on the endmill will be much higher than when cutting at 90° TEA, so the max achievable chipload/DOC combination for a given machine/endmill/material is lower. The recommended chipload/DOC values mentioned above include some margin to take this effect into account to some extent.

The other side effect of slotting is that chip evacuation is not as good: the flutes are in the air only 50% of the time, so the chips that form inside them have less time/fewer opportunities to fly away.  Deep slotting is notorious for causing issues when chips cannot be evacuated quickly enough.

Bottomline: slotting is hard on the machine, so you may have to:

* limit DOC to the low end of the range of values
* limit feedrate \(chipload...\)
* optimize chip evacuation by using an endmill with a lower number of flutes, and/or a good dust shoe or blast of air 

{% hint style="info" %}
Or, you can take a different approach and _avoid_ slotting altogether, by using smarter toolpaths. See adaptive clearing and pocketing in the [Toolpaths](toolpath-basics.md#adaptive-clearing-toolpaths) section!
{% endhint %}

Not that you will ever need to use it, but for the math-inclined among you, here's the equation to compute TEA from stepover value:

$$
TEA = \cos ^{ - 1}(1 - \frac{stepover}{0.5×endmill\_diameter})
$$

## Corners

While we are talking about TEA, let's take a look at what happens when cutting a square pocket at 50% tool engagement \(90° TEA\) and reaching a corner:

Just before moving into the corner, the tool engagement angle is 90°:

![](.gitbook/assets/page_87_800.png)

But _while_ cutting the corner, the TEA momentarily goes up to 180°:

![](.gitbook/assets/page_88_800.png)

before going down to 90° again. So the machine sees a "spike" in the cutting resistance at each corner. Just like for slotting, this means that the feedrate and DOC cannot be as high as one would like, since they need to be dialed back a bit to manage corners.   

{% hint style="info" %}
This boils down to optimizing the cut parameters used throughout the job specifically for these very short times when the corners are being cut, which is not very efficient. The alternatives include avoiding straight corners in the design if possible \(_e.g._ round the corners...\) or use an **adaptive clearing toolpath** that will take a lot of very shallow bites at the corners instead of a deep one.
{% endhint %}

## Plunge rate

All of the info above only focused on the feeds and speeds for the radial part of the cut, but when the endmill is plunging \(straight down/vertically\), things are quite different:

* \(obviously\) the cutting edges on the circumference of the endmill are not cutting anything anymore, the cutting happens at the tip of the endmill only, like a drillbit.
* BUT endmills are really not optimized for drilling, so their ability to plunge efficiently through material is quite limited.

One could compute specific plunge rate and RPM based on the specific geometry of the tip of the endmill, but in practice it's easier to just:

* use the same RPM as for radial cuts. This is a given when using a router where there is no dynamic control on the RPM anyway, so the same value is used throughout the cut. 
* use a plungerate that is experimentally chosen, following the rule of thumb 
  * **10% to 30%** of the feedrate for metals
  * **30% to 40%** of the feedrate for woods 
  * **40% to 50%** of the feedrate for plastics \(plunging fast is required to avoid melting\)

{% hint style="info" %}
These numbers are for plunging straight down. If the toolpath uses some ramping at an angle into the material, they can be increased quite a bit.
{% endhint %}

## Tool deflection

Endmills are not infinitely rigid, they tend to bend \(deflect\) when submitted to the cutting forces, and that deflection needs to be taken into account in the feeds and speeds. Here is a grossly exaggerated sketch of an endmill being subject to the cutting force:

![](.gitbook/assets/page_90_800_redo.png)

The amount of deflection depends on the endmill material \(carbide is more rigid than HSS\), diameter \(larger is stiffer\), stickout length, and of course the cutting forces that the endmill is subjected to, that depend on the chipload, DOC, WOC, and material.

When using conventional milling, the force tends to be parallel to the stock:

![](.gitbook/assets/page_91_800_redo.png)

When using climb milling, the force tends to be perpendicular, i.e. push the endmill away from the material:

![](.gitbook/assets/page_92_800_redo.png)

Either way, too much deflection is bad:

* moderate deflection will affect accuracy \(pieces will cut slightly larger or smaller than expected\)
* excessive deflection will cause tool wear or even tool breakage

So this is yet another parameter to watch out for when selecting feeds and speeds and DOC/WOC values, especially when using small diameter endmills.

## Climb or conventional ?

The direction of the cut \(climb versus conventional milling\) pertains to the toolpath's generation options and not directly to the feeds and speeds, but while we are on this topic: since tool deflection is mainly perpendicular to the cut when using climb milling, it would seem like it is better to use conventional milling, to keep deflection parallel to the cut and therefore minimize dimensional errors on the final piece. However there are other factors at play:

* in conventional milling, the chip is cut from thin-to-thick, so by definition when the flute first comes in contact with the material, it is rubbing the surface a little before it starts actually cutting into the material. This temporary rubbing amounts to heat, so in the long run a conventional cut produces more heat, leading to faster tool wear. Climb milling, since it cuts chips from thick-to-thin, does not have this problem.
* for the same "thick-to-thin" reason, climb milling is a little more tolerant of less-than-perfectly-sharp endmills.
* in conventional milling, the cutter flutes move against the direction of the feedrate, so chips are more likely to be pushed to the front of the cut, leading to chip recutting which is bad for finish quality. In climb milling, the chips tend to be pushed to the back of the endmill / behind the cut, so they are much less prone to recutting.
* in climb milling, the router torque pushes in the same direction as the feedrate, while in conventional it fights against the feedrate, so the forces on the stepper motors are higher.
* climb milling used to have a bad reputation for being dangerous to use on machines with a lot of backlash. While this was perfectly true on older manual mills, the point is moot on CNCs in general and the Shapeoko in particular.

So when all is said and done, climb milling wins on almost every aspect except deflection. The [Toolpaths](toolpath-basics.md#roughing-vs-finishing-toolpaths) section will cover the notion of "roughing" versus "finishing" toolpaths, and that will then open the way for the best approach: using **climb for roughing, then conventional for finishing**. 

This way, climb and its many advantages is used for most of the cut, and the possible deflections happening during this roughing pass will be taken care of by the light conventional finishing pass \(where the drawbacks of conventional will be irrelevant, since this finishing pass puts such low efforts on the machine anyway, and chip evacuation is not a problem either\) 

{% hint style="info" %}
This being said, your CAM tool may or may not give you the option to select the milling direction \(climb or conventional\). At the time of writing, Carbide Create did not have this feature, so it generated all toolpaths using conventional milling.
{% endhint %}

## MRR, Power, Torque, Force

If you need to optimize **cutting time** for a given piece,  you will also need to take a look at the **material removal rate** \(MRR\):

$$
MRR = WOC × DOC × Feedrate
$$

This yields a value in cubic inches \(or cubic millimeters\) of material removed per minute, and therefore relates to how fast you can complete a job. There is always a compromise to be found between going faster but with a lower tool engagement \(low DOC and/or low WOC\), or going slower but with a higher tool engagement \(higher DOC or high WOC\), while staying within the bounds of what the machine can do. The interesting thing about the MRR figure is that it allows one to **compare** different combinations, and figure out which one is the most efficient \(time-wise\).

Now if you want to figure out how close you are to the absolute/physical **limits** of the Shapeoko, \(yet\) another formula comes in the picture, to characterize the required power at the endmill level to achieve this MRR:

$$
Cutting \ power\ (in\ HP\ unit)= \frac{MRR}{K\_factor} = MRR × Unit\ Power
$$

the "K" factor \(or its inverse value the Unit Power\) is a constant that depends on the material's hardness, and corresponds to how many cubic inches per minute \(or cubic millimeters per minute\) of material can be removed using 1 horsepower.

For example, 

* 6061 T6 aluminium has a K of 3.34 cubic inches per minute
* it's about 10 in³/min for hard woods and hard plastics
* and up to 30 in³/min for soft woods, MDF, ...

Once you get this power value, you can compare it to your router's maximum output power. The Makita and DeWalt routers are rated at a max of 1.25HP \(932Watts\), but that is _input_ power, and the power efficiency of a router is not very good \(~50%\), so the max actual power at the cutter is more likely around 450W.

And finally, even if the cutting power is within the range of your router, there is still the matter of the **cutting force** that the Shapeoko has to put on the endmill to move it through the material: 

![](.gitbook/assets/page_95_800.jpg)

In metric units, the torque is the force \(in Newton\) times the distance in meters \(in this case the radius of the endmill\), and power in Watts is torque times the angular velocity w, in radians per second. Since the cutter does RPM revolutions per minute and each of them is 2×Pi radians:

$$
Cutting\ torque\_N⋅m= \frac{Cutter Power (Watts) }{2\pi×RPM/\ 60}
$$

or in the Imperial units converting N⋅m to lbf⋅in \(×8.85 factor\) , since \(60 × 8.85\) / \(2 × 3.14159\) = 84.5:

$$
Cutting\ torque\_lbf⋅in= \frac{Cutter Power (Watts) × 84.5}{RPM}
$$

and finally cutting force is:

$$
Cutting\ force= \frac{Cutting\ torque}{endmill\ radius}
$$

So all of this can be derived from the feedrate, WOC, DOC, endmill size, and material. And this cutting force can then be compared with the Shapeoko's limit estimated \(experimentally\) to be around **20 lbf** \(9Kg\)

## Wrapping up: suggested process

A proposed workflow to determine a _reasonable starting point_ for feeds and speeds and DOCs on the Shapeoko for a given project that uses a specific **material** and **endmill,** is:

* select a **target chipload** in the recommended chipload range ****for this material+endmill combination
  * refer to my proposed guideline table, or roll your own. Aim for the low end of the range, to reduce cutting force.
* determine **depth of cut** and **width of cut** \(stepover\) based on the machining style you want \(large WOC and small DOC, or large DOC and small WOC\).
* if stepover is less than 50%, **adjust target chipload for chip thinning**.
* select **target RPM** as the maximum value you can tolerate and feel comfortable using.
* determine the **required feedrate** for this RPM to achieve the adjusted target chipload
  * if computed feedrate exceeds the Shapeoko limit, choose a lower RPM value and recompute feedrate.
* determine **plungerate** depending on the material.
* check **deflection** value to make sure there is no risk of breaking the tool, and to optimize dimensional accuracy and finish quality.
* check that cutting **power** is within the router's limits.
* check that cutting **force** is within the machine's limits.
* check **MRR** to compare the efficiency of various cutting parameters.

Then...experiment**.** The realtime **feedrate override** available in most G-code senders is a great way to tune the chipload value and find the sweet spot for a particular job.

## Calculators

This section should have highlighted that MANY factors influence the selection of adequate feeds & speeds & DOC & WOC settings. 

While predefined recommendations for common endmills and materials are very useful, at some point it becomes impossible to produce feeds & speeds charts for every possible combination of factors, and also very tedious to compute everything manually. A number of calculators have been implemented to address this, ranging from free Excel spreadsheets that basically implement the equations mentioned above, to full-fledged commercial software that embed material/tool databases, the most famous one probably being **G-Wizard.**

Whether or not you _need_ a feeds & speeds calculator is debatable: most people use a limited number of combinations of material/endmill sizes anyway, in which case relying on a few good recipes for your machine is enough. The real value of calculators is in **optimizing** the feeds & speeds for a particular situation, and to see the effects of any parameter change on the rest of them.

A pretty neat feeds and speeds worksheet has been put together by @gmack on the Shapeoko forum \(which he derived from an original worksheet from the NYCCNC website\). I have attached a version here for convenience, but you may want to check if a more recent version is available on the forum.

![](.gitbook/assets/gmack_worksheet.png)

{% file src=".gitbook/assets/2019-08-11-speeds-and-feeds-workbook.xlsx" caption="gmack\'s advanced feeds and speeds worksheet" %}

1. fill-in the **specs of your router** or spindle \(once\).
2. fill-in the **specs of the selected endmill**, and the **target chipload value** you chose \(chip thinning will be taken into account automatically depending on WOC value\)
3. if you care about power/force analysis, look-up the **K-factor** for the material being cut \(there's a list in a separate tab of the worksheet\) and update it here.
4. select **target RPM** value \(or alternatively SFM, then RPM will be derived from it\). 
5. select **WOC and DOC** \(depending on your machining style\) 
6. The **required feedrate** to reach the target chipload will be computed. You can alternatively choose to override it with a given feedrate value \(and see what this does to chipload displayed below\), _or_ to forget about chipload and use a given cutting force as the ultimate target. Either way, the feedrate to be used will be displayed at the right end of this line.
7. You can then check the analysis of **deflection, cutting force, and power** in the lower part of the worksheet.

Then play with the input values to compare various cutting scenarios while staying within the machine's hard limits \(max RPM, max feedrate, max power, and max cutting force\)

{% hint style="info" %}
The latest version of **@gmack**'s worksheet is available in the forum here: [https://community.carbide3d.com/t/speeds-feeds-power-and-force-sfpf-calculator/16237](https://community.carbide3d.com/t/speeds-feeds-power-and-force-sfpf-calculator/16237)
{% endhint %}

{% hint style="info" %}
Once you determine good feeds and speeds and confirm that it is cutting correctly, it is useful to capture a snapshot of the worksheet for that particular usecase for future reference \(just duplicate the tab in the worksheet\)
{% endhint %}

If you still feel overwhelmed or don't care about optimizing power, force and deflection, I derived a more basic version:  

![](.gitbook/assets/basic_worksheet_screenshot.png)

{% file src=".gitbook/assets/fs\_worksheet\_basics.xls" %}

1. fill-in the **number of flutes** and **diameter** of your endmill
2. pick a **target chipload** value from the guideline table on the right
3. select an **RPM** value
4. select **WOC** and **DOC** based on the recommanded values on the right \(derived from the selected endmill diameter\)

This basic worksheet will just compute the required feedrate to get the desired chipload \(taking chip thinning into account\). If the computed feedrate turns red, it is beyond the limit of the Shapeoko, and you should select a lower RPM and/or use an endmill with a lower flute count.

## Telltale signs of wrong F&S

The most common signs of inadequate feeds and speeds are:

* sound, and specifically **chatter**: when feeds and speeds are not right for a given material/endmill/DOC/WOC, the tool tends to vibrate, and this vibration can get worse if there is resonance with another source of periodic variation elsewhere in the system \(most often: the router and its RPM\). This results in an ugly sound, a poor finish with marks/dents/ripples on the surface, and a reduced tool life. 
* **finish quality**: even without chatter, a poor surface finish can indicate that the final cutting pass was too agressive \(too much chipload or too much deflection\). Increasing RPMs may help, but the best approach is to use a finish pass with very low WOC.
* **melted material**: especially in plastics and soft metals like aluminium, if the feedrate is too low for the selected RPM, the friction will cause the material to melt rather than shear, the tool flutes will start filling with melting material, and this usually ends up with tool breakage. You will need to feed faster, and/or use an endmill with a lower flute count.
* **endmill temperature**: the endmill should not be more than slightly warm at the end of a cut: if it gets hot to the touch \(careful!\), the feeds and speeds are likely incorrect \(too low or too high chipload\), or the tool is dull and is rubbing rather than cutting. In extreme cases, the endmill color itself may change to a dark shade.
* making **dust**, instead of clearly formed chips is an indication that chipload is probably too low \(MDF is an exception, you just cannot get chips anyway with this material\)

## What about V-bits?

Notice how I carefully avoided the case of V-bits throughout this section ? That's because V-bits are special, due to their geometry and the nature of their associated toolpaths:

* The cutting speed varies along the edge of a V-bit, from its largest section \("top" of the V-bit\) to its point \(the surface speed is zero at the tip\).
* The effective cutting diameter varies depending on how deep the V-bit goes.
* The V-carving toolpaths tend to generate sloped trajectories and a lot of plunges and retracts, so the cutter engagement is constantly changing.

So it does not quite make sense to be using a target chipload value for a V-bit. Experimentation is king in V-carving, but a common starting point for using V-bits in wood is as follows:

* RPM in the 16k–20k range
* Feedrate in the 30–60 ipm range \(lower for hard wood, faster for soft wood\)
* Depth per pass in the 0.1–0.2'' range
* Plunge rate in the 10–20 ipm range 

{% hint style="info" %}
If your CAM software supports it, you may want to use a roughing pass and a finishing pass \(with more aggressive settings for the roughing pass to spare time, and more conservative settings for the finishing pass\)
{% endhint %}

{% hint style="info" %}
Sometimes when using V-bits, running the G-code twice can lead to a cleaner finish.
{% endhint %}

## What about surfacing?

While the principles decribed above apply, when doing a surfacing operation using either a surfacing bit \(a.k.a. Fly cutter\) or any large square endmill, the intent is usually to shave off just a thin layer of material off the top surface, so one can feed quite fast. A typical setting would be:

* RPM in the 16k–20k range
* Feedrate in the 80–100 ipm range 
* Depth per pass in the 0.01–0.05'' range
* Plunge rate is mostly irrelevant for such shallow passes, any value will do.

