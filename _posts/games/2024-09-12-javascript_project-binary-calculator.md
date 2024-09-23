---
title: Binary Calculator
layout: post
description: A Binary Math illustrative application using HTML, Liquid, and JavaScript.
categories: [Javascript]
permalink: /javascript/project/binary-calculator
menu: nav/javascript_project.html
toc: true
comments: true
---

<!-- 

Learn how the page works, plus learn about binary
Hack 0: Do your own on/off thing with the Image and Buttons thing
Hack 1: change the display to indicate the value of bin (128, 64, 32, 16, 8, 4, 2, 1)
Hack 2: change one-zero input under the bulb to perform updates to the page

Learn about binary representations
Hack 3: add an ASCII character display to text when 8 bits, determine if printable or not printable
Hack 4: change to 24 bits add a color code and display color when 24 bits. Think about the display on this one, perhaps wrap bits 

Jekyll Table Reference: https://idratherbewriting.com/documentation-theme-jekyll/mydoc_tables.html

--->

{% assign BITS = 8 %}
<style>
    td {
        text-align: center;
        vertical-align: middle;
    }
    .color-box {
        width: 100px;
        height: 100px;
        margin: 10px auto;
        background-color: #000000;
    }
</style>

<table>
    <thead>
        <tr class="header" id="table">
            <th>Plus</th>
            <th>Binary</th>
            <th>Octal</th>
            <th>Hexadecimal</th>
            <th>Decimal</th>
            <th>ASCII</th>
            <th>Minus</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><div class="calc-button" id="add1" onclick="add(1)">+1</div></td>
            <td id="binary">00000000</td>
            <td id="octal">0</td>
            <td id="hexadecimal">0</td>
            <td id="decimal">0</td>
            <td id="ascii">N/A</td>
            <td><div class="calc-button" id="sub1" onclick="add(-1)">-1</div></td>
        </tr>
    </tbody>
</table>

<!-- Bits Table with values displayed -->
<table>
    <thead>
        <tr>
            {% assign bit_values = "128,64,32,16,8,4,2,1" | split: ',' %}
            {% for i in (0..bits) %}
            <th>
                <img id="bulb{{ i }}" src="{{site.baseurl}}/images/bulb_off.png" alt="" width="40" height="Auto">
                <div class="button" id="butt{{ i }}" onclick="javascript:toggleBit({{ i }})">Turn on</div>
                <div>{{ bit_values[i] }}</div> <!-- Display bit value (Hack 1) -->
            </th>
            {% endfor %}
        </tr>
    </thead>
    <tbody>
        <tr>
            {% for i in (0..bits) %}
            <td><input type='text' id="digit{{ i }}" Value="0" size="1" oninput="updateFromInput({{ i }})"></td> <!-- Hack 2 -->
            {% endfor %}
        </tr>
    </tbody>
</table>

<!-- Display color box when using 24 bits -->
<div class="color-box" id="color-box"></div>

<script>
    const BITS = {{ BITS }};
    const MAX = 2 ** BITS - 1;
    const MSG_ON = "Turn on";
    const IMAGE_ON = "{{site.baseurl}}/images/bulb_on.gif";
    const MSG_OFF = "Turn off";
    const IMAGE_OFF = "{{site.baseurl}}/images/bulb_off.png";

    // Return string with current value of each bit
    function getBits() {
        let bits = "";
        for (let i = 0; i < BITS; i++) {
            bits = bits + document.getElementById('digit' + i).value;
        }
        return bits;
    }

    // Set conversions and ASCII character (Hack 3)
    function setConversions(binary) {
        const decimalValue = parseInt(binary, 2);
        document.getElementById('binary').innerHTML = binary;
        document.getElementById('octal').innerHTML = decimalValue.toString(8);
        document.getElementById('hexadecimal').innerHTML = decimalValue.toString(16);
        document.getElementById('decimal').innerHTML = decimalValue;

        // ASCII display
        if (decimalValue >= 32 && decimalValue <= 126) {
            document.getElementById('ascii').innerHTML = String.fromCharCode(decimalValue);
        } else {
            document.getElementById('ascii').innerHTML = "N/A";
        }

        // Update color if we are using 24 bit
        if (BITS === 24) {
            updateColor(binary);
        }
    }

    // Convert decimal to base 2 with padding for binary
    function decimal_2_base(decimal, base) {
        let conversion = "";
        do {
            let digit = decimal % base;
            conversion = "" + digit + conversion;
            decimal = ~~(decimal / base);
        } while (decimal > 0);
        if (base === 2) {
            while (conversion.length < BITS) {
                conversion = "0" + conversion;
            }
        }
        return conversion;
    }

    // Toggle selected bit
    function toggleBit(i) {
        const dig = document.getElementById('digit' + i);
        const image = document.getElementById('bulb' + i);
        const butt = document.getElementById('butt' + i);
        if (image.src.match(IMAGE_ON)) {
            dig.value = 0;
            image.src = IMAGE_OFF;
            butt.innerHTML = MSG_ON;
        } else {
            dig.value = 1;
            image.src = IMAGE_ON;
            butt.innerHTML = MSG_OFF;
        }
        setConversions(getBits());
    }

    // Update bit from text input
    function updateFromInput(i) {
        const dig = document.getElementById('digit' + i).value;
        toggleBit(i); // Toggles based on user input
    }

    // Add and subtract values
    function add(n) {
        let binary = getBits();
        let decimal = parseInt(binary, 2);
        if (n > 0) {
            decimal = MAX === decimal ? 0 : decimal += n;
        } else {
            decimal = 0 === decimal ? MAX : decimal += n;
        }
        binary = decimal_2_base(decimal, 2);
        setConversions(binary);
        for (let i = 0; i < binary.length; i++) {
            let digit = binary.substr(i, 1);
            document.getElementById('digit' + i).value = digit;
            if (digit === "1") {
                document.getElementById('bulb' + i).src = IMAGE_ON;
                document.getElementById('butt' + i).innerHTML = MSG_OFF;
            } else {
                document.getElementById('bulb' + i).src = IMAGE_OFF;
                document.getElementById('butt' + i).innerHTML = MSG_ON;
            }
        }
    }

    // Updates color when 24 bits are toggled
    function updateColor(binary) {
        if (binary.length === 24) {
            const red = parseInt(binary.substr(0, 8), 2);
            const green = parseInt(binary.substr(8, 8), 2);
            const blue = parseInt(binary.substr(16, 8), 2);
            document.getElementById('color-box').style.backgroundColor = `rgb(${red}, ${green}, ${blue})`;
        }
    }
</script>
