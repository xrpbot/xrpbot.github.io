.. title: Acrobot progress
.. slug: acrobot-progress
.. date: 2014/09/12 19:25:05
.. tags: mathjax
.. link: 
.. description: 
.. type: text

In the video below, you see the first demo of our Acrobot in motion. However, this is work in progress - we cannot balance it at the top yet. Read on for more details.

.. raw:: html

	<style>.embed-container { position: relative; padding-bottom: 56.25%; padding-top: 30px; height: 0; overflow: hidden; max-width: 100%; height: auto; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='http://www.youtube.com/embed/ET3YHnJrKZ0' frameborder='0' allowfullscreen></iframe></div>

After the mechanical construction of the Acrobot was finished, we had to develop a suitable motor controller to power the BLDC motor. As mentioned in the last blog post, most R/C BLDC controllers employ *sensorless commutation*, which leads to jerky motion at startup and for slow speeds and is therefore of little use for us. Thus, we added an encoder to the motor axis and started to develop a new controller. To reduce development effort, we re-used the power stage from a cheap sensorless BLDC controller, but completely replaced the logic by an STM32 microcontroller board. We also added current sensors to be able to precisely measure the current in the motor.

Initially, we used a "standard" BLDC control algorithm with the commutation timing determined by the motor encoder. This, however, still resulted in fairly jerky motion at low speeds. Therefore, we implemented `space vector modulation <http://en.wikipedia.org/wiki/Space_vector_modulation>`_, i.e. we feed the motor with actual 3-phase AC current, the phase of which is locked to the motor motion via the encoder. The new controller works fairly well, but still requires some cleanup. It will be documented in detail in a future blog post.

Now that the Acrobot hardware works, it is time to work on the control algorithms. For the demo in the video above, we used a very simple energy-pumping controller, as suggested in a `paper by M. Spong <http://www.clemson.edu/ces/crb/ece496/spring2002/group1a/acrobot_swingup.pdf>`_. The idea is to use a PD controller to control the angle on the active joint, :math:`q_2`, and dynamically set it to

.. math::
    q_2^{d} = \alpha \arctan(\dot{q}_1)

where :math:`\alpha` is a fixed parameter (gain) and :math:`\dot{q}_1` is the angular velocity of the passive joint. Obviously, this needs an initial "kick" to get started, so we manually play with the offset in the beginning. If the gain is adjusted correctly, we can pump enough energy into the system such that the Acrobot almost gets to the top, but does not swing over. As we cannot capture it yet, we simply switch off the controller after a while and let the system swing down again.

The next step will be to implement more sophisticated control schemes to actually capture the Acrobot at the top and balance it there. After that, we need to clean up, document, and release everything.

