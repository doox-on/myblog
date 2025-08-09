---
title: About Me
feature_text: |
  ## Jungwoo Moon
  Computer Engineer & Embedded Software Developer
feature_image: "/assets/GT.jpeg"
excerpt: "Hi, I am an embedded/firmware engineer."
---
{% include figure.html image="/assets/JW.jpeg" width="400" height="300" %}

As a Computer Engineer from the Georgia Institute of Technology, I have a deep passion for embedded systems that operate at the intersection of hardware and software. With a solid foundation in operating systems (OS), firmware, and real-time systems development, I aim to create efficient and reliable software.


## Projects
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">
        {{ post.title }}
      </a>
    </li>
  {% endfor %}
</ul>



## Skills

- **Programming Languages:** C, C++, Python, Assembly
- **Microcontrollers & Platforms:** STM32, ESP32, Arduino
- **OS & Systems:** Real-Time Operating Systems (Free RTOS), Linux
- **Developer Tools:** Git, Docker, GDB
- **Concepts:** Device Drivers, System Architecture, Low-level Optimization
- **Hardware Skills:** PCB Design (KiCad/Altium), Oscilloscope, Logic Analyzer

{% include button.html text="LinkedIn" icon="linkedin" link="www.linkedin.com/in/jungwoo-moon-135743263" color="#0A66C2" %}



---
