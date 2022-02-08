---
layout: page
title: Tesla AutoPilot
subtitle: 
---

In 2021, Tesla held an AI Day event ([youtube](https://www.youtube.com/watch?v=j0z4FweCy4M)). The pre-event section demos a full self-driving in a Tesla car. A little more information can be found on the Tesla [AI webpage](https://www.tesla.com/AI)m while [Jason Zhang's (unofficial) blog](https://saneryee-studio.medium.com/deep-understanding-tesla-fsd-part-1-hydranet-1b46106d57) goes into more detail.

Note that there was a [demo](https://www.youtube.com/watch?v=ivTeW4xWQv0) of suburban, highway, and parking driving back in 2016.

### Tesla Autopilot 

#### Vision subsystem

There are 8 cameras around the car. Along with direct kinematics from the car, these are converted into a 3D vector space. The cameras are 1280x960 12-bit 36Hz. After some preprocessing, each is passed to REGNETS, which feed to BiFPN feature networks. A Transformer network is used to combine 8 outputs (one per camera). Video is handled by a feature queue. There's a push to the queue every 27ms and every 1 metre tranversed. Then a spatial RNN is used as video module, which is able to handle e.g. occlusion. This is then a common output to HydraNets - a bunch of use-specific networks for object (cars?) detection, traffic lights, lane prediction (other things?) 

The per-camera networks do [semantic segmentation](https://cnvrg.io/semantic-segmentation/), object detection, and [monocular depth estimation](https://www.sciencedirect.com/science/article/abs/pii/S0925231220320014). They have an astonishing training set, coming from 1M Tesla vehicles in real time. A full build of Autopilot neural networks involves 48 networks that take 70,000 GPU hours to train. They output 1000 distinct tensors at each timestep.

The [sensor suite](https://www.tesla.com/autopilot) is cool:

- 3 forward cameras (wide - 120 degree fish eye; main; narrow (long-range)
- 2 forward-looking side cameras
- 2 rear-looking side cameras
- 1 rear-view camera

plus a bunch of ultrasonic sensors

#### Planner subsystem

The vision component passes to two planner modules, to place routes from 3D vectors. There's an explicit planning module - a non-convex high-dimensional search. It's hybrid, a coarse discrete search, followed by continuous function optimization. Plans not just for self, but for other vehicles on road, this anticipation is crucial to good driving. But there's also a learning-based method as well, to solve more complex problems like parking with cone obstructions. Use AlphaZero Atari game type techniques - neural networks and monte-carlo tree search. It's about 2000x more efficient. The neural net planner takes not only vector space but also intermediate features from vision.

#### Training

But how do you train these massive networks (1 x 10^8 parameters)? 

Tesla have a massive labelling programme. This includes an large in-house manual (i.e. human) labelling unit. The process began with annotation on 2D images, but graduated to labelling in 3D vector space, which is then reprojected back into image space for checking. They have some cool software for making this smooth for the human labeller. Apparently, the 3D vector space system improved throughput 100X. 

They have also developed auto-labelling systems, some of the details of which were not clear to me. One of the cool techniques, though, is the use of photorealistically rendered simulations. These simulations are videogames with autopilot as a player. This gives perfect labels, of course. 

On that labelling team - It seems to be [1,000 people](https://saneryee-studio.medium.com/deep-understanding-tesla-fsd-part-4-auto-labeling-simulation-60c9bfd3bcb5) being paid [$22 an hour](https://electrek.co/2021/02/08/tesla-looks-hire-data-labelers-feed-autopilot-neural-nets-images-gigafactory-new-york/). At 30 hours/week for 46 weeks/year (possibly both underestimates), that's an **annual bill of around $30m.** 

#### Compute resource

Tesla are building Project Dojo, a 10-cabinet exascale supercomputer designed from custom silicon up to be 4X performance and 1/5th footprint of the best existing system for AI training. It starts with the D1 chip, which contains 354 compute nodes in a 2D mesh. Each node is 1 TFLOP (although 1.25MB RAM), and each D1 chip is 362TFLOP and 400W, with massive (4TB/s) I/O speed. 25 D1 dies are placed on a training tile (9 PFLOP per tile). The tile is the size of an LP, and about 2 inches thick. Six tiles in a tray, two trays in a cabinet. 10 cabinets. Total 1.1 EFLOP. 

For scale, the deep learning workstation due for delivery to my School in 2022 has GPU compute equivalent to about one D1 chip. So, Dojo is about half-a-million times more powerful.

It looks like there may be [plans](https://www.tesla.com/AI) to provide mass access to Dojo in the future.


#### TeslaBot

Tesla is the world's biggest robotics company - cars with AutoPilot are robots. TeslaBot with 8 cameras in the head, 40 actuators, the Tesla Full Self Driving computer. The economy at foundation is labour, so when labour is not scarce, we will need a basic income. Physical work will be a choice. Capital equipment is just distilled labour. Is there any limit to the economy? Perhaps not. 
TeslaBot will do boring, repetitive, dangerous jobs that people don't want to do. 


#### Approach to free software

Elon was asked about publishing or open sourcing Tesla AutoPilot? Basically no, because it's really expensive to develop, and Elon doesn't know how it would be paid for if open-sourced, unless people want to work for free. Oh well. 


Join our team tesla.com/AI
