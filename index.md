---
title: Jungwoo Moon
feature_text: |

feature_image: "/assets/GT.jpeg"
excerpt: "Hi, I am an embedded/firmware engineer."
---
{% include figure.html image="/assets/JW.jpeg" width="400" height="300" %}

As a Computer Engineer from the Georgia Institute of Technology, I have a deep passion for embedded systems that operate at the intersection of hardware and software. With a solid foundation in operating systems (OS), firmware, and real-time systems development, I aim to create efficient and reliable software.


## Projects

<ul style="font-size: 1.2em; line-height: 1.6; text-align: center;">
  {% for post in site.posts %}
    <li style="list-style: none; margin: 8px 0;">
      <a href="{{ post.url | relative_url }}" style="font-size: 1.3em; text-decoration: none;">
        {{ post.title }}
      </a>
    </li>
  {% endfor %}
</ul>

<ul style="font-size: 1.2em; line-height: 1.6; text-align: center; padding-left: 0;">
  {% for post in site.posts %}
    <li style="list-style: none; margin: 8px 0;">
      <a href="{{ post.url | relative_url }}" 
         style="font-size: 1.3em; text-decoration: none; color: #007acc;">
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

{% include button.html text="LinkedIn" icon="linkedin" link="https://www.linkedin.com/in/jungwoo-moon-135743263" color="#0A66C2" %}


---
