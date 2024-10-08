---
layout: base
title: Student Home 
description: Home Page
image: /images/mario_animation.png
hide: true
comments: true
---


<!-- Liquid:  statements -->

<!-- Include submenu from _includes to top of pages -->
{% include nav/home.html %}
<!--- Concatenation of site URL to frontmatter image  --->
{% assign sprite_file = site.baseurl | append: page.image %}
<!--- Has is a list variable containing mario metadata for sprite --->
{% assign hash = site.data.mario_metadata %}  
<!--- Size width/height of Sprit images --->
{% assign pixels = 256 %}

<!--- HTML for page contains <p> tag named "Mario" and class properties for a "sprite"  -->

<p id="mario" class="sprite"></p>
  
<!--- Embedded Cascading Style Sheet (CSS) rules, 
        define how HTML elements look 
--->
<style>

  /*CSS style rules for the id and class of the sprite...
  */
  .sprite {
    height: {{pixels}}px;
    width: {{pixels}}px;
    background-image: url('{{sprite_file}}');
    background-repeat: no-repeat;
  }

  /*background position of sprite element
  */
  #mario {
    background-position: calc({{animations[0].col}} * {{pixels}} * -1px) calc({{animations[0].row}} * {{pixels}}* -1px);
  }
</style>

<!--- Embedded executable code--->
<script>
  ////////// convert YML hash to javascript key:value objects /////////

  var mario_metadata = {}; //key, value object
  {% for key in hash %}  
  
  var key = "{{key | first}}"  //key
  var values = {} //values object
  values["row"] = {{key.row}}
  values["col"] = {{key.col}}
  values["frames"] = {{key.frames}}
  mario_metadata[key] = values; //key with values added

  {% endfor %}

  ////////// game object for player /////////

  class Mario {
    constructor(meta_data) {
      this.tID = null;  //capture setInterval() task ID
      this.positionX = 0;  // current position of sprite in X direction
      this.currentSpeed = 0;
      this.marioElement = document.getElementById("mario"); //HTML element of sprite
      this.pixels = {{pixels}}; //pixel offset of images in the sprite, set by liquid constant
      this.interval = 100; //animation time interval
      this.obj = meta_data;
      this.marioElement.style.position = "absolute";
    }

    animate(obj, speed) {
      let frame = 0;
      const row = obj.row * this.pixels;
      this.currentSpeed = speed;

      this.tID = setInterval(() => {
        const col = (frame + obj.col) * this.pixels;
        this.marioElement.style.backgroundPosition = `-${col}px -${row}px`;
        this.marioElement.style.left = `${this.positionX}px`;

        this.positionX += speed;
        frame = (frame + 1) % obj.frames;

        const viewportWidth = window.innerWidth;
        if (this.positionX > viewportWidth - this.pixels) {
          document.documentElement.scrollLeft = this.positionX - viewportWidth + this.pixels;
        }
      }, this.interval);
    }

    startWalking() {
      this.stopAnimate();
      this.animate(this.obj["Walk"], 3);
    }

    startRunning() {
      this.stopAnimate();
      this.animate(this.obj["Run1"], 6);
    }

    startPuffing() {
      this.stopAnimate();
      this.animate(this.obj["Puff"], 0);
    }

    startCheering() {
      this.stopAnimate();
      this.animate(this.obj["Cheer"], 0);
    }

    startFlipping() {
      this.stopAnimate();
      this.animate(this.obj["Flip"], 0);
    }

    startResting() {
      this.stopAnimate();
      this.animate(this.obj["Rest"], 0);
    }

    stopAnimate() {
      clearInterval(this.tID);
    }
  }

  const mario = new Mario(mario_metadata);

  ////////// event control /////////

  window.addEventListener("keydown", (event) => {
    if (event.key === "ArrowRight") {
      event.preventDefault();
      if (event.repeat) {
        mario.startCheering();
      } else {
        if (mario.currentSpeed === 0) {
          mario.startWalking();
        } else if (mario.currentSpeed === 3) {
          mario.startRunning();
        }
      }
    } else if (event.key === "ArrowLeft") {
      event.preventDefault();
      if (event.repeat) {
        mario.stopAnimate();
      } else {
        mario.startPuffing();
      }
    }
  });

  //touch events that enable animations
  window.addEventListener("touchstart", (event) => {
    event.preventDefault(); // prevent default browser action
    if (event.touches[0].clientX > window.innerWidth / 2) {
      // move right
      if (currentSpeed === 0) { // if at rest, go to walking
        mario.startWalking();
      } else if (currentSpeed === 3) { // if walking, go to running
        mario.startRunning();
      }
    } else {
      // move left
      mario.startPuffing();
    }
  });

  //stop animation on window blur
  window.addEventListener("blur", () => {
    mario.stopAnimate();
  });

  //start animation on window focus
  window.addEventListener("focus", () => {
     mario.startFlipping();
  });

  //start animation on page load or page refresh
  document.addEventListener("DOMContentLoaded", () => {
    // adjust sprite size for high pixel density devices
    const scale = window.devicePixelRatio;
    const sprite = document.querySelector(".sprite");
    sprite.style.transform = `scale(${0.2 * scale})`;
    mario.startResting();
  });

</script>

My Name is Ian Manangan and I'm a Junior at Del Norte Highschool. My hobbies are volleyball, biking, and basketball. After school, I practice volleyball, work, and sleep.

This page is a forked page from student_2025, and portfolio_2025.

Name: Ian Manangan
Age: 16 years old

Interests and Hobbies:
- Volleyball, I played for 2 years.

Other Hobbies:
- Video games
- Biking
- Volleyball
- Basketball
- Reffing

I mostly took this class because not only will it benefit me a lot for later college majors, but it could help with job applications to get some experience to show to other companies that i've had experience in this subject, or i've delt with a situation like this before. 

This is My Nighthawk Homepage. Here are three buttons:
<style>
  hr {
    border-width: thin;
    border-style: solid;
    border-color: white;
  }
  .Border_1 {
    border-style: solid;
    border-color: lightgray;
    box-shadow: 2px 2px 0px 1px white;
    border-width: thick;
  }
  .Border_2 {
    border-style: solid;
    border-color: darkgray;
    box-shadow: 2px 2px 0px 1px white;
    border-width: thick;
  }
  .Border_3 {
    border-style: solid;
    border-color: gray;
    box-shadow: 2px 2px 0px 1px white;
    border-width: thick;
  }
  .chicken_sandwich { 
    -webkit-animation: spin 1s linear infinite;
    animation: spin 1s linear infinite;
  }

  @-webkit-keyframes spin {
    0% { -webkit-transform: rotate(0deg); }
    100% { -webkit-transform: rotate(360deg); }
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
</style>
<html>
  <body>
    <h2> Placeholder Button: </h2>
    <hr>
    <button id="Chicken"><div class = "Border_1"> This button does nothing...</div></button>
      <br><br><br>
    <h2> CSP page Button: </h2>
    <hr>
    <a href = "navigation/section/csp"><button id="chicken1"><div class = "Border_2"> This button gets you to the CSP page</div></button></a>
      <br><br><br>
    <h2> DNHS page Button: </h2>
    <hr>
    <a href = "https://delnorte.powayusd.com/apps/bell_schedules/"><button id="chicken2"><div class = "Border_3"> This button gets you to DNHS bell schedule</div></button></a>
      <br><br><br><br>
    <div class ="chicken_sandwich"><img src = "images/Dog.png" alt= "Logo img" width="250" height="250"></div>
    <img src = "images/Chicken_Sandwich.png" alt= "Logo img" width="250" height="250">
      <br><hr>
    <h3> EXAMPLE BUTTONS: W/ & W/O LINK </h3>
    <button id = "x">Without</button>
    <br><br>
    <a href="about:blank"><button id = "x">With</button></a>
    <br><br>
    <h2>(Here is my games!):</h2>
  </body>
</html>
{% include nav/javascript_project.html %}