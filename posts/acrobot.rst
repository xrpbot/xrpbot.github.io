.. title: Acrobot
.. slug: acrobot
.. date: 2014/05/04 15:57:59
.. tags: acrobot, draft
.. link: 
.. description: 
.. type: text

[VIDEO GOES HERE]

In order to gain experience with a smaller, less ambitious project, we decided to build an Acrobot.

The Acrobot is a famous toy system in optimization based control. It consists of a double pendulum where only the middle joint is actuated. The task for the controller is to drive the Acrobot from its rest position (hanging down) to the goal position, pointing straight up.

We use the open-source `PSOPT <http://www.psopt.org/>`_ code to generate trajectories for the Acrobot. This is mostly a black-box method, i.e. it does not require any specific knowledge about the Acrobot. You just supply it with the equations of motion, the start and goal configurations and an objective that allows it to choose between the (infinitely many) possible trajectories. Like all non-linear optimization, it required a bit of fiddling to get it to work, but in the end, the process was relatively painless.

Our Acrobot will use an R/C BLDC motor, which drives the middle joint through a timing belt, providing a 1:5 reduction. The simulation suggests that this motor should have ample torque and velocity reserves for the task.

One problem with R/C BLDC controllers is that they usually employ *sensorless commutation*. This avoids needing an encoder on the motor shaft. However, sensorless commutation only works when the motor is already turning. Starting is a bit of a black art and never perfect, causing jaggy motion during startup. This is of no use for the Acrobot project, where precisely tracking trajectories is essential. We have therefore build a sensor-based BLDC controller, which will be the subject of future blog posts.

The video above shows a model of the Acrobot, generated from actual CAD drawings, executing the trajectory PSOPT calculated for it.

Of course, actually executing the trajectory requires some form of feedback to correct the tracking errors that inevitably occur. We plan to use the LTV-LQR method for this, which will be discussed in the next post in the series.

Actual construction will start once our mill is ready, probably in 1 to 2 weeks. You can find the current version of the construction drawings in the `acrobot-hardware repository <https://github.com/xrpbot/acrobot-hardware/>`_.
