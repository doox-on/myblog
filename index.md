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


## Mini Projects
<ul style="font-size: 1.6rem; line-height: 1.6; text-align: center; margin: 0;">
  {% assign sorted_experiments = site.experiments | sort: 'date' | reverse %}
  {% for post in sorted_experiments %}
    <li style="list-style: none; margin: 8px 0;">
      <a href="{{ post.url | relative_url }}" 
         style="font-size: 1.3rem; text-decoration: none;">
        {{ post.title }}
      </a>
    </li>
  {% endfor %}
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
  {% include figure.html image="/assets/1.JPG" width="350" height="300" caption="Complete military service" %}
  {% include figure.html image="/assets/2.JPG" width="350" height="300" caption="In Vietnam with my friends" %}
</div>


I was born in Korea, always searching for what I truly wanted to do. After completing my mandatory military service, I learned how to live and work with others, resolve conflicts, and take responsibility as part of a team.




<div class="row-figs">
  {% include figure.html image="/assets/3.JPG" width="350" height="300" caption="Farewell!" %}
  {% include figure.html image="/assets/4.JPG" width="350" height="300" caption="At the Incheon airport" %}
</div>

After discharge, I traveled, worked, and eventually moved to the U.S. because of my parents’ job. At first it was scary, but I hoped a new world would help me discover my path.




<div class="row-figs">
  {% include figure.html image="/assets/5.jpg" width="350" height="300" caption="GTLI friends" %}
  {% include figure.html image="/assets/6.jpg" width="350" height="300" caption="My first home party in the U.S." %}
</div>

My first stop was Georgia Tech Language Institute. I couldn’t speak any English at the time, but I avoided using Korean and made friends from other countries. That mindset helped me adapt quickly.




<div class="row-figs">
  {% include figure.html image="/assets/7.jpg" width="350" height="300" caption="Pizza place near campus" %}
  {% include figure.html image="/assets/16.jpeg" width="350" height="300" caption="Tech green" %}
</div>

<div class="row-figs">
  {% include figure.html image="/assets/9.jpeg" width="350" height="300" caption="Disney's Hollywood studios" %}
  {% include figure.html image="/assets/10.jpeg" width="350" height="300" caption="K-pop concert with my sister" %}
</div>

Later, I studied hard to get into Georgia Tech and was so happy when I was accepted. I met many friends, enjoyed both school and travel—Disney World, K-pop concerts, and more.




<div class="row-figs">
  {% include figure.html image="/assets/11.jpeg" width="350" height="300" caption="In Tokyo with my friends" %}
  {% include figure.html image="/assets/12.jpeg" width="350" height="300" caption="Akihabara Genshin shop" %}
</div>

I also revisited Asia to see old friends and explore new countries. Some even came to Japan just to see me, which meant a lot.




<div class="row-figs">
  {% include figure.html image="/assets/13.jpeg" width="350" height="300" caption="In Thailand" %}
  {% include figure.html image="/assets/14.jpeg" width="350" height="300" caption="Met new friends" %}
</div>

Learning English really opened my world. In Thailand, the Philippines, and Japan, I could easily connect with people and felt confident that I could do anything.

---
