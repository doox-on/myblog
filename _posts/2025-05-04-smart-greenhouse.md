---
title: Self-Sustaining Solar-Powered Greenhouse
categories:
- General
excerpt: |
  A pot still is a type of still used in distilling spirits such as whisky or brandy. Heat is applied directly to the pot containing the wash (for whisky) or wine (for brandy).
feature_text: |
  ## The Pot Still
  The modern pot still is a descendant of the alembic, an earlier distillation device
feature_image: "https://picsum.photos/2560/600?image=872"

---

## Self-Sustaining Solar-Powered Miniature Greenhouse

An automated, self-sufficient greenhouse designed for urban agriculture, featuring a sun-tracking solar panel and integrated environmental controls.


{% include figure.html image="/assets/greenhouse1.jpeg" %}

{% include video.html id="1XSyMbiWBRU" %}

### Project Overview

The SMART Greenhouse is a compact, automated system designed to make gardening accessible to urban dwellers and plant enthusiasts with limited space. It addresses the challenges of manual greenhouse management by automating key processes like temperature and humidity regulation, all while operating entirely on self-generated solar power. The core novelty lies in its self-sufficiency, combining automated controls with an energy-efficient power system featuring a solar panel mounted on a rotating gimbal to maximize sun exposure throughout the day.

{% include figure.html image="/assets/greenhouse2.jpeg" %}

### Key Features

I was responsible for the design and implementation of the greenhouse’s control systems. This involved developing the closed-loop climate regulation mechanisms and integrating real-time environmental monitoring.

{% include video.html id="Jp6AsWEoLeo" %}

* **Automated Climate Control**
    * The internal environment is regulated using a closed-loop control system.
    * If the internal temperature exceeds the external temperature, a servo-controlled venting window opens to allow for passive air exchange.
    * If the temperature continues to rise, an exhaust fan activates to actively push hot air out.

* **Real-Time Environmental Monitoring**
    * Three sets of temperature and humidity sensors monitor the conditions outside the greenhouse, inside the greenhouse, and in the soil.
    * All sensor data is clearly displayed on a 2-inch LCD screen, providing the user with an immediate overview of the plant's environment.


### Challenges & Solutions

* **Challenge: I/O Port Scarcity and Hardware Integration**
    * A key technical challenge arose from I/O port limitations on our microcontroller. The design required multiple temperature/humidity sensors, but due to the address-based nature of the I2C protocol, only one could be used per bus. This, combined with other peripherals, created a critical shortage of available I/O ports.
    * **Solution:** We implemented a dual-microcontroller architecture to overcome this hurdle. A second MCU was added to manage a subset of the sensors, and the two controllers communicated seamlessly via UART. This allowed the primary MCU to aggregate all environmental data for display on the LCD. For future iterations, using an I2C multiplexer would be an alternative solution to explore.

* **Challenge: Aligning Team-Wide Design Vision**
    * Initial implementation was significantly hampered because team members had different undeclared concepts for the final design. This discrepancy between visions caused rework and implementation difficulties, ultimately contributing to the decision to de-scope the water irrigation system.
    * **Solution:** Once we identified this conceptual gap, we immediately held detailed discussions to align on every aspect of the project. We made the pragmatic decision to cut features that could not be completed in time and focused entirely on the core functionalities. This experience was a critical lesson in the importance of ensuring all team members are on the same page through detailed, shared documentation from the project's outset.

### Future Enhancements

* **Hardware Optimization:** Consolidate the two STM32 microcontrollers into a single, more powerful MCU to reduce complexity and cost.
* **Advanced Controls:** Implement a more precise control model with detailed states for different environmental conditions to improve responsiveness.
* **Active Climate Systems:** Investigate the feasibility of adding low-power heating and cooling elements to provide a greater range of temperature control.
* **Data Logging:** Program a long-term "test mode" to collect sensor and power usage data over several days to quantitatively verify system performance and reliability.
