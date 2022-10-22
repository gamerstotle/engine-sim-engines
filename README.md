# engine-sim-engines

Hi!

This is simply a place for me to share the engines I create in Ange-Yaghi's project Engine Sim. 

Please visit his Github for more information and the latest release if you want to try out these engines, or try making one for yourself. 

Ange's Github: https://github.com/ange-yaghi/engine-sim

The engine's I'm making all follow the same basic pattern of description (definition? creation? whatever.)

They are described top to bottom in this order:

imports/labels/constants -> here is where you will adjust the vehicle mass that gets incorporated later

private node: turbulence -> I usually don't fiddle with this, simply because I'm still learning what (if anything) it does. It is, however, required.

private node: wires -> this is where you define how many ignition leads your ignition module and engine will use, the sim emulates distributor/points-based ignition.

private node: head -> this is where you will define the head for your motor. Ultimately this is a fluid dynamics simulation, not really an engine sim, so the procedural 
  generation of engine sounds comes in part from these flow_sample charts, which models the flow and attenuation of flow of two volumetric containers: intake and exhaust.
  Changes here, as far as I understand, can fundamentally alter the way your engine sounds. Since, an engine is merely an air pump that go vroom. You also define the cham-
  ber volume of your cylinders. This is important and it's a good idea to make existing engines first to get a feel for what it's *supposed* to be. Gotta learn the rules
  before you can break them.
  
private node: camshaft -> again, this can look daunting, but for me, best practice has been to comment my firing order and crank rotation angle above the cam definitions,
  it really keeps my head straight. I also have a comment that explains the Lobe Separation Angle formula, thus for any given intake and exhaust lobe center you can calculate
  the separation without needing to see it on the cam card. Especially for older motors, I've found the cam cards to be severely lacking.
  
public node: engine! -> here's where you define your engine! This is where it all comes together. Everything sort of flows top to bottom, you define the engine, give it a name
  starter_motor specs, redline, max_AFR efficiency (I think, I'm fairly certain this is what determines your calculated AFR in "game"), and you can define certain sim parameters
  so they're constant on subsequent startups. Then you instance your ignition wires, define stroke, bore, conrod_length, rod_mass, crank_mass, compression_height, etc. Instance
  your crankshaft and define its TDC location in degrees. In general, Inline motors will be 90* TDC, then in ascending/descending increments you will change the TDC as you add more
  cylinder banks. Like a 72* V10 for example. Next you instance however many journals for your crankshaft, defining their degrees of rotation off centerline. This can be found online
  for most engine types. If you're building something wacko, experiment :) Next define your piston and con_rod params, intake, exhaust, then instance your cylinder bank and cylinders.
  Next you define the intake and exhaust lobe(s), I eventually intend to abstract quite a bit of this out to other dedicated files for import. I want to create a "parts bin" of real world
  and custom designed "OE Spec", performance, and competition camshafts, exhausts, and other various parameters. Next you define/instance the ignition module, you delineate the spark-cut rev
  limit, the limiter duration (how fast you bang off that redline, set this to 0.0 for true Honda-Boi enjoyment |:3 ), then you connect all of the wires to each cylinder in the correct firing order. 
  Again, I leave a comment for myself of the firing order from above so I get it right. 
  
private node: vehicle -> Here's where you define the vehicle (if you want one.) I just followed my gut on the drag coeff, and cross_sect_area mostly. The cross_sectional area currently is roughly
  equivalent to a 2006 Mitsubishi Lancer EVO IX MR; No particular reason...I just think it's neat. :D Diff ratio: Set this to what you want your diff to be. Pretty simple. Tire radius:
  I'm prety much leaving it at around 16in tires, I'm not entirely sure how the sim incorporates this, but my logic was "smaller tire, probably smaller wheel, less unsprung weight === go fas"
 
private node: transmission -> define your transmission name, torque limit, and gear ratios. From what I can tell, the sky's the limit on gearing. Go nuts. 

public node: package name -> essentially this works as a convenient collection and entry point for the main.mr file. In main.mr,take the node_name and call it at the bottom of main.mr

for example: 

import "engine_sim.mr"
import "themes/default.mr"
import "engines/custom/RB26DE.mr"

use_default_theme()
nissan_rb26de()

Anyway, I hope this helps you get started designing your own engines! This is a fantastic sim, thank you so much to Ange-Yaghi for creating what I've always wanted to create myself, but didn't know
where to start. Here's his github again, and his youtube channel. 

https://github.com/ange-yaghi/engine-sim

https://www.youtube.com/channel/UCV0t1y4h_6-2SqEpXBXgwFQ

Thanks for stopping by!
