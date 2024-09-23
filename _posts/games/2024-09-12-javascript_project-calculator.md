---
layout: post
title: Javascript Calculator
description: A common way to become familiar with a language is to build a calculator.  This calculator shows off button with actions.
categories: [Javascript]
permalink: /javascript/project/calculator
menu: nav/javascript_project.html
toc: true
comments: true
---

<!-- 
Hack 0: Right justify the result
Hack 1: Test conditions on small, big, and decimal numbers, and report on findings. Fix issues.
Hack 2: Add the common math operation that is missing from the calculator
Hack 3: Implement 1 number operation (ie SQRT) 
-->

<!-- 
HTML implementation of the calculator. 
-->

<!-- 
    Style and Action are aligned with HRML class definitions
    style.css contains the majority of style definitions (number, operation, clear, and equals)
    - The div calculator container sets 4 elements to a row
    The background is credited to Vanta JS and is implemented at the bottom of this page
-->
<style>
  .calculator-output {
    /*
      calculator output
      the top bar shows the results of the calculator;
      result to take up the entirety of the first row;
      span defines 4 columns and 1 row
    */
    grid-column: span 4;
    grid-row: span 1;
  
    border-radius: 10px;
    padding: 0.25em;
    font-size: 20px;
    border: 5px solid black;
  
    display: flex;
    align-items: center;
  }
  canvas {
    filter: none;
  }
</style>

<!-- Add a container for the animation -->
<div id="animation">
  <div class="calculator-container">
      <!--result-->
      <div class="calculator-output" id="output">0</div>
      <!--row 1-->
      <div class="calculator-number">1</div>
      <div class="calculator-number">2</div>
      <div class="calculator-number">3</div>
      <div class="calculator-operation">+</div>
      <!--row 2-->
      <div class="calculator-number">4</div>
      <div class="calculator-number">5</div>
      <div class="calculator-number">6</div>
      <div class="calculator-operation">-</div>
      <!--row 3-->
      <div class="calculator-number">7</div>
      <div class="calculator-number">8</div>
      <div class="calculator-number">9</div>
      <div class="calculator-operation">*</div>
      <!--row 4-->
      <div class="calculator-clear">A/C</div>
      <div class="calculator-number">0</div>
      <div class="calculator-number">.</div>
      <div class="calculator-equals">=</div>
      <!--row 5-->
      <div class="calculator-operation">/</div>
      <div class="calculator-operation">sqrt</div>
  </div>
</div>

<!-- JavaScript (JS) implementation of the calculator. -->
<script>
// initialize important variables to manage calculations
// initialize important variables to manage calculations
var firstNumber = null;
var operator = null;
var nextReady = true;
// Build objects containing key elements
const output = document.getElementById("output");
const numbers = document.querySelectorAll(".calculator-number");
const operations = document.querySelectorAll(".calculator-operation");
const clear = document.querySelector(".calculator-clear");
const equals = document.querySelector(".calculator-equals");

// Number buttons listener
numbers.forEach(button => {
  button.addEventListener("click", function() {
    number(button.textContent);
  });
});

// Number action
function number(value) {
    if (value != ".") {
        if (nextReady == true) {
            output.innerHTML = value;
            if (value != "0") {
                nextReady = false;
            }
        } else {
            output.innerHTML += value;
        }
    } else {
        if (output.innerHTML.indexOf(".") == -1) {
            output.innerHTML += value;
            nextReady = false;
        }
    }
}

// Operation buttons listener
operations.forEach(button => {
  button.addEventListener("click", function() {
    operation(button.textContent);
  });
});

// Operator action
function operation(choice) {
    if (choice === "sqrt") { // Handle square root immediately
      if (firstNumber < 0) { // If square root is a negative
        alert("Cannot calculate square root of a negative number!");
        return;
      }
      firstNumber = parseFloat(output.innerHTML);
      firstNumber = Math.sqrt(firstNumber); // Use Math.sqrt for square root calculation
      output.innerHTML = firstNumber.toString();
      nextReady = true;
      return;
    }
    if (firstNumber == null) {
        firstNumber = parseFloat(output.innerHTML); //Parsefloat allows a decimal to be produced
        nextReady = true;
        operator = choice;
        return;
    }
    firstNumber = calculate(firstNumber, parseFloat(output.innerHTML)); 
    operator = choice;
    output.innerHTML = firstNumber.toString(); // allows decimals...
    nextReady = true;
}

// Calculator
function calculate(first, second) {
    let result = 0;
    switch (operator) {
        case "+":
            result = first + second;
            break;
        case "-":
            result = first - second;
            break;
        case "*":
            result = first * second;
            break;
        case "/":
            if (second === 0) {
                alert("Cannot divide by zero!");
                return first;
            }
            result = first / second;
            break;
        default: 
            break;
    }
    return result;
}

// Equals button listener
equals.addEventListener("click", function() {
    equal();
});

// Equal action
function equal() {
    firstNumber = calculate(firstNumber, parseFloat(output.innerHTML));
    output.innerHTML = firstNumber.toString();
    nextReady = true;
}

// Clear button listener
clear.addEventListener("click", function() {
    clearCalc();
});

// A/C action
function clearCalc() {
    firstNumber = null;
    output.innerHTML = "0";
    nextReady = true;
};
</script>


