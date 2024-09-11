---
layout: base
title: Student Home 
description: Home Page
image: /images/mario_animation.png
hide: true
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
  body, html {
    margin: 0;
    padding: 0;
    overflow: hidden; /* Prevent scrollbars */
  }

  .sprite {
    height: {{pixels}}px;
    width: {{pixels}}px;
    background-image: url('{{sprite_file}}');
    background-repeat: no-repeat;
    position: absolute; /* So Mario can move around */
    bottom: 0; /* Start on the "ground" */
  }

  #mario {
    background-position: calc({{mario_metadata["Walk"].col}} * {{pixels}} * -1px) 
                         calc({{mario_metadata["Walk"].row}} * {{pixels}} * -1px);
  }
</style>

<script>
  var mario_metadata = {}; 
  {% for key in hash %}  
  var mario_key = "{{key | first}}";
  var values = {};
  values["row"] = {{key.row}};
  values["col"] = {{key.col}};
  values["frames"] = {{key.frames}};
  mario_metadata[mario_key] = values;
  {% endfor %}

  class Mario {
    constructor(meta_data) {
      this.tID = null;
      this.positionX = 0;
      this.positionY = 0; // Track vertical position for jumping
      this.isJumping = false; // Prevent multiple jumps
      this.isFacingRight = true; // Track Mario's direction
      this.currentSpeed = 0;
      this.marioElement = document.getElementById("mario");
      this.pixels = {{pixels}};
      this.interval = 100;
      this.obj = meta_data;
      this.gravity = 5; // Adjusted gravity for smoother jump
      this.jumpHeight = 100; // Adjusted height of the jump
      this.jumpSpeed = 5; // Adjusted speed of the jump
      this.marioElement.style.position = "absolute";
      this.marioElement.style.bottom = "0px"; // Start at the bottom (ground level)
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

        // Ensure Mario stays within the viewport
        const viewportWidth = window.innerWidth;
        if (this.positionX > viewportWidth - this.pixels) {
          this.positionX = viewportWidth - this.pixels; // Stop at right edge
        } else if (this.positionX < 0) {
          this.positionX = 0; // Stop at left edge
        }
      }, this.interval);
    }

    startWalking(direction) {
      this.stopAnimate();

      if (direction === 'right') {
        this.marioElement.style.transform = 'scaleX(1)'; // Face right
        this.animate(this.obj["Walk"], 3); // Walk right
        this.isFacingRight = true;
      } else if (direction === 'left') {
        this.marioElement.style.transform = 'scaleX(-1)'; // Face left
        this.animate(this.obj["Walk"], -3); // Walk left
        this.isFacingRight = false;
      }
    }

    startRunning(direction) {
      this.stopAnimate();

      if (direction === 'right') {
        this.marioElement.style.transform = 'scaleX(1)'; // Face right
        this.animate(this.obj["Run1"], 6); // Run right
        this.isFacingRight = true;
      } else if (direction === 'left') {
        this.marioElement.style.transform = 'scaleX(-1)'; // Face left
        this.animate(this.obj["Run1"], -6); // Run left
        this.isFacingRight = false;
      }
    }

    startJumping() {
      if (!this.isJumping) {
        this.isJumping = true;

        // Set the jump animation
        this.marioElement.style.backgroundPosition = `-${this.obj["Jump"].col * this.pixels}px 
                                                      -${this.obj["Jump"].row * this.pixels}px`;

        let jumpInterval = setInterval(() => {
          if (this.positionY < this.jumpHeight) {
            this.positionY += this.jumpSpeed;
            this.marioElement.style.bottom = `${this.positionY}px`;
          } else {
            clearInterval(jumpInterval);

            // Apply gravity to bring Mario back down
            let fallInterval = setInterval(() => {
              if (this.positionY > 0) {
                this.positionY -= this.gravity;
                this.marioElement.style.bottom = `${this.positionY}px`;
              } else {
                clearInterval(fallInterval);
                this.positionY = 0;
                this.marioElement.style.bottom = `0px`; // Reset to ground level
                this.isJumping = false;

                // Return to walking/resting animation
                if (this.currentSpeed !== 0) {
                  this.startWalking(this.isFacingRight ? 'right' : 'left');
                } else {
                  this.marioElement.style.backgroundPosition = `-${this.obj["Rest"].col * this.pixels}px 
                                                                -${this.obj["Rest"].row * this.pixels}px`;
                }
              }
            }, 20);
          }
        }, 20);
      }
    }

    stopAnimate() {
      clearInterval(this.tID);
    }
  }

  const mario = new Mario(mario_metadata);

  // Arrow key controls
  window.addEventListener("keydown", (event) => {
    switch (event.key) {
      case "ArrowRight":
        event.preventDefault();
        mario.startWalking('right');
        break;
      case "ArrowLeft":
        event.preventDefault();
        mario.startWalking('left');
        break;
      case "ArrowUp":
        event.preventDefault();
        mario.startJumping();
        break;
    }
  });

  // Stop movement on key release
  window.addEventListener("keyup", (event) => {
    if (event.key === "ArrowRight" || event.key === "ArrowLeft") {
      mario.stopAnimate();
    }
  });

  document.addEventListener("DOMContentLoaded", () => {
    // Adjust sprite size for high pixel density devices
    const scale = window.devicePixelRatio < 1.5 ? 1 : 0.5; // Scale down for large screens
    const sprite = document.querySelector(".sprite");
    sprite.style.transform = `scale(${scale})`;
    mario.startResting();
  });
</script>

My Name is Ian Manangan and I'm a Junior at Del Norte Highschool. My hobbies are volleyball, biking, and basketball. After school, I practice volleyball, work, and sleep.

This page is a forked page from student_2025, and portfolio_2025.

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
    <div class ="chicken_sandwich"><img src = "images/Chicken_Sandwich.png" alt= "Logo img" width="250" height="250"></div>
      <br><hr>
    <h3> EXAMPLE BUTTONS: W/ & W/O LINK </h3>
    <button id = "x">Without</button>
    <br><br>
    <a href="about:blank"><button id = "x">With</button></a>
  </body>
</html>
