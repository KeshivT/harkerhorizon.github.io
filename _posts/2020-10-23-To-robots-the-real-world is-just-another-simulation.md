---
layout: "post"
title: "To robots, the real world is just another simulation."
author: "Ashwin Reddy (’19)"
print_publication: true
tags: issue3
categories: Perspectives
related_image: /imgs/Robot.png, /imgs/simulated-camera-grid.png
---

<!--excerpt-->
When first learning to program, students might think of programs as a sequence of instructions to solve a problem. This mindset is useful when one is dealing with a computer and some self-contained data but is perhaps not as instructive when it comes to the field of robotics. Robot software must play the role of a brain, taking in sensory data, processing it to make decisions, and interacting with the real world using actuators (e.g. motors). The recent surge in machine learning has made the brain metaphor even more appealing as new algorithms can learn iteratively from a robot’s attempt to accomplish a task. However, some current approaches require teams of robots to all simultaneously collect trials. An algorithm published last year called domain randomization deals with this problem and shows how a robot might successfully execute a task in the real world having only been trained in a simulation.

First, why exactly is it so difficult to program robots? Essentially, human-engineered robot software must deal with the messy complexity of the real world that humans navigate deftly, sometimes without realizing it. For example, robot software has to make the best use of sensor inputs and motor outputs (I/O) that are likely to contain noise. In other words, what the sensors perceive may not align with the real world, and the robot may not move precisely as a given command expected. This difference between the ground truth and the robot’s view of the world means that programs have to assume faultiness in both inputs and outputs. Imagine trying to open a jar of peanut butter while squinting your eyes and having your arms move more or less than you previously intended. Additionally, the software needs to process those inputs with respect to some actual goal or task it is trying to accomplish. Finally, one also has to treat every trial on a physical robot as a cost; robots sometimes operate on slow time scales and are susceptible to mechanical wear-and-tear. These are all challenges that arise because the software needs to interface with the real world, ideally at the same pace that a human would.

To create robot software that can deal with these problems, a classical roboticist might consider a pipeline, a sequence of programs which feed information into each other. The first program takes in the raw sensory input and localizes an object of interest. The next program starts with the object and determines how a robotic arm might grasp the object, and so on and so forth until the last program produces outputs for the motors. Robots from Boston Dynamics can perform incredible feats as the image below shows, and while Dynamics does not publish technical details of their software, WIRED magazine posits that their robots are heavily hand-engineered with a pipeline-esque pattern [2].

| ![](/imgs/Robot.png) | 
|:--:| 
|Atlas, a Boston Dynamics robot, dealing with antagonistic human interference. [4]
(Image: Levine, Sergey, et al. “Learning Hand-Eye Coordination for Robotic Grasping with Deep Learning and Large-Scale Data Collection.” The International Journal of Robotics Research, vol. 37, no. 4–5, Apr. 2018, pp. 421–436, doi:10.1177/0278364917710318.)|

However, recent advancements in machine learning have begun to change the robotics software paradigm. The problems of noisy I/O and interpreting the inputs with respect to a goal can be solved with enough data and deep learning algorithms, referring to statistical models sophisticated enough to take in all of the robot’s sensory inputs and produce outputs for all of the robot’s joints. Some deep learning models are called end-to-end because they can replace the entire classical pipeline, from the raw visual
input to the motor outputs. A team at Google Brain used an end-to-end method by collecting data from a team of robots operating simultaneously, but this approach lacks scalability or reproducibility as they used 14 robots and over 900,000 grasp attempts [4]. 

In the case of robots, transfer learning is starting to use an interesting tool to great effect: simulators. Imagine the same team of robots that Google Brain had, just rendered on a computer screen rather than in the real world. All that is required is a model of the robot, which precisely specifies the geometry of the joints and physical parameters like friction and visual parameters like color and texture. In this way, robot time, which is expensive and slow, is traded for computer time, which is faster and safer. A technical difficulty remains in making sure that the end-to-end model learned in simulation works in the real world. In practice, the real world and simulated world are too different to execute transfer learning (e.g. simulators might not account for backlash, which is slippage between gears) successfully without additional steps. 

In 2017, Josh Tobin et. al from OpenAI, a research institute which has been looking into the use of machine learning in robotics, published a paper providing an elegant and somewhat whimsical solution known as domain randomization [3]. While the robot was learning a given task in the simulator, the researchers forced the simulator to randomly vary parameters such as friction and color. The intent is to force the model to see enough of a variety of virtual worlds that the real world will look like just another variation. Thus, the value is in the diversity of simulated worlds and not in the verisimilitude of the worlds: as the authors say, “physically plausible choice of textures and lighting are not needed.” This surprisingly simple technique was enough to train a real robot to grasp the kinds of food containers that might be in a household pantry. In 2018, the same technique was extended to train a dexterous robotic hand to rotate a cube in its palm [1]. Unlike the robots of Boston Dynamics, these robots were able to operate effectively in the real world without a single test trial in it.

| ![](/imgs/simulated-camera-grid.png) | 
|:--:| 
|Examples of simulated images in which a robot arm manipulates the cube. [1]
(Andrychowicz, M., Baker, B., Chociej, M., & Jozefowicz, R. (2018, July). _Learning dexterous in-hand manipulation._ Retrieved from OpenAI website: [Learning Dexterity](https://blog.openai.com/learning-dexterity/)|

To some, domain randomization might seem like a gimmick, but it enables some exciting new fields of inquiry. For one, transfer learning makes robotics reproducible: anybody with a computer can train a robot in simulation, opening up the possibility of using distributed computing. It also means scalability as computer time is cheaper, faster, and safer than robot time. Finally, the same algorithm can easily be applied and tested to different kinds of robots. Together, these factors might make it possible someday for the ordinary person to train a robot to perform a new complex task in their house without ever seeing the robot fail in real life.


References
[1] Andrychowicz, M., Baker, B., Chociej, M., & Jozefowicz, R. (2018, July). _Learning dexterous in-hand manipulation._ Retrieved from OpenAI website: https://blog.openai.com/learning-dexterity/ 

[2] Friday, R. (2018, February 18). What’s really going on in those Boston Dynamics robot videos? _Wired._ Retrieved from https://www.wired.co.uk/article/boston-dynamics-robotics-roboticist-how-to-watch

[3] Tobin, J., Fong, R., Ray, A., & Schneider, J. (2017, March). _Domain randomization for transferring deep neural networks from simulation to the real world._ Retrieved from OpenAI website: https://arxiv.org/abs/1703.06907

[4] Levine, Sergey, et al. “Learning Hand-Eye Coordination for Robotic Grasping with Deep Learning and Large-Scale Data Collection.” The International Journal of Robotics Research, vol. 37, no. 4–5, Apr. 2018, pp. 421–436, doi:10.1177/0278364917710318.
