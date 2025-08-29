---
title:
feature_text: |

feature_image: "/assets/GT.jpeg"
excerpt: "Hi, I am an embedded/firmware engineer."
---

<div style="text-align: center; font-size: 1.5em; font-weight: bold; margin: 1em 0;">
  Jungwoo Moon
</div>
<div style="text-align: center;">
Buford, Georgia 30519 | jmoon318@gatech.edu
</div>
{% include figure.html image="/assets/JW.jpeg" width="400" height="300" %}

I am currently a senior at Georgia Tech and have been a teaching assistant for an embedded systems course for one year. I am interested in embedded development and aspire to become an on-device AI/ML developer.


## Projects

<ul style="font-size: 1.6rem; line-height: 1.6; text-align: center; margin: 0;">
  {% for post in site.posts %}
    <li style="list-style: none; margin: 8px 0;">
      <a href="{{ post.url | relative_url }}" 
         style="font-size: 1.3rem; text-decoration: none;">
        {{ post.title }}
      </a>
    </li>
  {% endfor %}
</ul>

<ul style="font-size: 1.6rem; line-height: 1.6; text-align: center; margin: 0;">
  <li style="list-style: none; margin: 0px 0;">
    <a href="https://jmoon318.github.io/CS2340Final/" 
       style="font-size: 1.3rem; text-decoration: none;">
      Food Management Android App
    </a>
  </li>
</ul>

## Skills

- **Programming Languages:** C, C++, Python, Assembly
- **Microcontrollers & Platforms:** STM32, ESP32, Arduino
- **OS & Systems:** Real-Time Operating Systems (Free RTOS), Linux kernel
- **Developer Tools:** Git, Docker, GDB
- **Concepts:** Device Drivers, System Architecture, Low-level Optimization
- **Languages:** English, Korean

{% include button.html text="LinkedIn" icon="linkedin" link="https://www.linkedin.com/in/jungwoo-moon-135743263" color="#0A66C2" %}


## About Me

<style>
  /* 한 번만 넣으면 됨 */
  .row-figs{display:flex;gap:16px;flex-wrap:wrap;align-items:flex-start}
  .row-figs > *{flex:0 1 auto;margin:0;text-align:center}
</style>

<div class="row-figs">
  {% include figure.html image="/assets/1.JPG" width="300" height="300" caption="사진 1 설명" %}
  {% include figure.html image="/assets/2.JPG" width="300" height="300" caption="사진 2 설명" %}
</div>


I was born in Korea, always searching for what I truly wanted to do. After completing my mandatory military service, I learned how to live and work with others, resolve conflicts, and take responsibility as part of a team.




---
