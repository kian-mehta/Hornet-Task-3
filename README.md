# HornetXII Challenge: Bottom Camera Search

## Task

The starting point was an AUV simulation with an 8-thruster vehicle, a cascaded position and velocity controller, and a tiled seafloor containing a red cube. The supplied model did not have a camera; the `Cameras` subsystem only contained Mechanics Explorer viewpoints. The task was to add sensing and a mission layer that would allow the vehicle to find the cube, hold depth while searching, and centre itself over the target.

## My Solution Overview
My final solution has three main parts: a synthetic downward-facing pinhole camera, a red cube detector that streams the live feed of the camera, and a mission planner that uses a lawnmower searching technique, and then switches to a centering branch when the red cube is detected. 

I implemented this using MATLAB, Simulink, Simscape and Simscape Multibody, without adding external libraries or toolboxes. 

It is important to note that while the camera system uses the cube’s position and pixel centroid **the red detector and mission planner do not access this data directly.** 

The PinHole Renderer basically simulates a camera; it uses the cube pose ONLY to check if the raycast intersects with the cube. it returns an image, which is what the real camera would return as well. the only limitation of this approach is that the red color in reality would be more desaturated and prone to interferences. I have implemented a solution by making the camera return a desaturated red tone, and then added a noise layer which applies to the entire image. I think that this system of processing the clear image to transform it into something simulating a real underwater image can be improved upon, and I can add better noise and distortion effect. 

Also, I changed the model properties to randomize the position of the cube in each run. The random function places the cube between a square centered at the origin and ±5 in the x and y directions. The AUV then conducts the lawnmower search in this area

**IMPORTANT NOTE:** While the total floor area is a 10x10 square centered at the origin, I have intentionally simplified my solution so that the simulation does not take too long. The idea can easily be extended to a 10x10 square by changing the `searchHalfSpan` in the `initFcn` in Model Properties and the `searchSize` constant in the project root (both will then be changed to 10).

## Setbacks and alternate routes considered

I considered using a real camera simulation using Unreal Engine and MATLAB 3D Animation (an idea suggested by generative AI) but decided against it as ???

Another approach was to use a multibeam sonar to detect the cube, instead of a camera. After some research, I found that this approach would be superfluous, as the real component price would be far too high compared to a camera and the gains in cube detection performance would not justify it. 

## Further work

With two more weeks I would want to experiment with computer vision using OpenCV and real underwater images to assess if such technology is required for this task. If the gains are considerable, I would then add it to the MATLAB project. 

I would also look into the controller to check for the potential bugs as highlighted in the task description. After that, I would want to work on attempting to increase the speed of the AUV, especially while centering itself on the object. 

Lastly, would add a better range or altitude estimate so that the vehicle could safely descend once it has aligned over the cube. 