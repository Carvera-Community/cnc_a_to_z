# Introduction

The intent of this guide is to help new users of the Shapeoko CNC learn enough about both the big picture and the underlying technical details, to feel at ease with the machine, the workflow, and CNC lingo in general.

As fun as it is to go ahead and cut a first project following a tutorial \(and you should\), having a good understanding of how the machine works and what underlying CNC concepts are involved goes a long way to avoid frustration later, when things do not work out the way you expected \(and sooner or later they will\).

There are many ways to get started. Most people want to be up and running and making things as quickly as possible, without the burden of learning all the nitty-gritties, and that's fine. At the other end of the spectrum, some users \(including yours truly\) enjoy CNC and using the Shapeoko as a hobby, and end up spending more time experimenting with the machine than actually cutting useful projects, and that's fine too. Hopefully there is a little something for everyone in this guide.

This guide builds ontop of Julien Heyman's excellently written [Shapeoko CNC A to Z](https://ShapeokoEnthusiasts.gitbook.io). We have used the GitBooks varient functionality to allow you to select which family of CNC machines you are using in order to tailor the content based on your machine type.

## Safety considerations

This is not \(only\) a legal disclaimer, this is plain old common sense but needs to be said: using a CNC as a hobbyist comes with a small number of specific risks for your safety, that are easily adressed.

### Mechanical hazard

* keep your hands away from the moving parts during operation, cleaning up the mess on the belts/pulleys can probably wait until the end of the job.
* get yourself a way to do an emergency stop, within arm's reach. A killswitch removing ALL power \(to the machine _and_ to the router\) is easy to install.
* turn off the machine before diving in to check/adjust/fix something. Sure, the machine is "not supposed" to move by itself, but user errors happen \(ever clicked anywhere by mistake?\) and software has bugs. Changing an endmill while the machine is powered is probably the only sensible exception \(and unplugging the router while doing so is a best practice\).
* wear protection goggles anytime the machine is operating without an enclosure, no exceptions. Endmills break from time to time, and shrapnel may fly out in random directions when they do. 

### Respiratory hazard

* cutting some materials \(_e.g._ MDF or exotic wood\) produces dust that can be very toxic, and wood dust is just bad for your health in general. 
* a good dust collection and filtering system will take care of this risk. You'll find out soon enough if you don't use one \(and/or an enclosure\) that a thin layer of dust ends up covering everything around the machine, so imagine what it does to your lungs. If you need a good scare, go read about wood dust related health issues, I bet you will invest in dust collection and filtering promptly afterwards.

### **Fire hazard**

* while this risk is very remote, it does exist.
* don't leave your machine \(completely\) unattended while it is cutting. The stock material may become loose, and rubbing a piece of metal spinning at thousands of RPM against wood is a potential way to start a fire. Google "CNC" + "fire" if you need proof.

## **Overview**

There are many ways to learn and use the Shapeoko, but the typical experience for a new user can go something like this:

* order the machine \(after reading a lot of good things about it\) 
* while waiting for delivery, get familiar with CNC concepts: 
  * this is the intent of the first sections \([CNC workflow](workflow.md), [Anatomy of a Shapeoko](anatomy-of-a-shapeoko.md), [CAD, CAM, and G-code](cad-cam-tools.md), [Cutters & collets](cutters.md), [Feeds & speeds](feeds-and-speeds-basics.md), [Toolpaths](toolpath-basics.md), [Workholding](workholding.md)\). They go in _way_ more detail than is strictly necessary to get started, so if it feels overwhelming just skip the details and get cutting.
  * the [Cutters & collets](cutters.md) section includes a few indications as to what starter set of endmills and collets could be suitable for you to purchase, depending on the projects you have in mind. 
* receive and assemble the machine, and run the Hello World example with a marker. Oh the joy of these first few hours! I chose _not_ to have a section about machine assembly, because Carbide 3D's docs are quite detailed already, and there are many things that are either specific to the machine type \(standard vs XL vs XXL\) or bound to change frequently and that would render this information obsolete very quickly. Carbide 3D's email support and the community forum are the best way to sort out any specific machine assembly issue.
* run your first cuts in wood or MDF \(this is covered in the [Running a job](first-cuts.md) section\). 
* realize that:
  * CNC is messy, and that now is a good time to look into dust collection and possibly an enclosure. This is covered in the [Shapeoko setup](dust-collection.md) section.
  * The geometry of cut pieces or finish quality is not quite right, and that it may be necessary to look into [Squaring, surfacing, tramming](squaring.md) the machine, and possibly [Dimensional accuracy](x-y-z-calibration.md).
* get confused about required feeds and speeds for that particular case you want to use but for which no recommendation exists: hopefully re-reading the [Feeds & speeds](feeds-and-speeds-basics.md) section should help.
* try cutting plastics or metal \(see [Usecases: cutting plastics](cutting-plastics.md) , [Usecases: cutting metal](cutting-metal.md)\)
* at some point, witness the machine misbehave, blame the hardware or the software, start investigating, and \(often\) end up concluding that you messed up somewhere in the workflow. The [Troubleshooting & maintenance](maintenance.md) section has a few pointers, after that the user community and Carbide 3D's support team are your best bet.
* somewhere along this path, outgrow Carbide Create and Carbide Motion and consider moving to more advanced [CAD, CAM, G-code](cad-cam-tools.md) tools.
* after a while, begin to feel an itch for [HW upgrades](upgrading-the-machine.md).

## **Feedback & credits**

Comments/corrections/contributions are most welcome, you can contact me on the Shapeoko community forum \([https://community.carbide3d.com/](https://community.carbide3d.com/)\) as **@Julien**.

Shoutout to the whole forum community, the collective knowledge and experience accumulated over there and on the Shapeoko wiki \([https://wiki.shapeoko.com/index.php/Shapeoko\_3](https://wiki.shapeoko.com/index.php/Shapeoko_3)\) is incredible. I tried to bring out _some_ of that know-how in this e-book, but it is also tainted by my own limited experience and habits, so any potential incorrect information/guidance is my bad.

Special thanks to :

* **@patonclover** for the digital versions of all illustrations, which are a very welcome replacement for my initial \(poor\) hand-drawn doodles
* **@WillAdams** and **@luc.onthego** for proofreading and suggesting lots of good improvement ideas
* **@snaterst** \(on the forum\) for providing the vise setup picture
* **@Dusty.Tools** \(find him on Instagram\) for the T-tracks setup picture
* **@Griff** for the spindle cooling system pictures
* **@stutaylo** and **@PaulAlfaro** for the bullnose endmill pics
* **@bikerdan** for the Z-plus pic
* **@Todd** for the Sweepy pic
* **@i3oilermaker** for the v-carved inlay pics
* **@gmack** for his excellent feeds and speeds worksheet
* **@Hooby** for the nice Janka wood hardness database included in the worksheets
* **@neilferreri** for his awesome CNCjs macros, Fusion360 post processor, and other goodness
* **@Microwave\_Monkey** for the belt change tip and associated pics
* **@LiamN** for some of the maintenance tips and pictures.

This e-book is released under the Creative Commons CC BY-NC-SA license \(in plain English: do whatever you want with it except using it for commercial purposes, and if you modify & redistribute, give credit and keep the same license\).

![](.gitbook/assets/cc-by-nc-sa.png)

## Legal disclaimer

By accessing this eBook, you accept this disclaimer in full.  
  
The information provided within this eBook is for general informational purposes only. Even though I have attempted to present accurate information, there are no representations or warranties, express or implied, about the completeness, accuracy, or reliability of the information, products, services, or related graphics contained in this eBook for any purpose. The information is provided “as is,”  to be used at your own risk.  
The methods described in this eBook represent the author’s personal experiences. They are not intended to be a definitive set of instructions. You may discover there are other methods and materials to accomplish the same end result. Your results may differ.

This eBook includes information regarding products and services by third parties. I do not assume responsibility for any third party materials or opinions. Use of recommended third party materials does not guarantee that your results will mirror third party results or my own. Publication of such third party material is just a recommendation and expression of the authors’ personal opinions. Any reliance on the third party material is at your own risk.

All trademarks appearing in this eBook are the property of their respective owners.  
  
No warranty may be created or extended by any promotional statements for this work. The author shall not be liable for any damages arising herefrom.
