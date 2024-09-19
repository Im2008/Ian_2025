---
title: Cookie Clicker
layout: post
description: Cookie Clicker
categories: [Javascript]
permalink: /javascript/project/cookie
menu: nav/javascript_project.html
toc: true
comments: true
---

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cookie Clicker</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
      background-color: #f7e7ce;
    }
    h1 {
      font-size: 2.5rem;
      margin-bottom: 20px;
    }
    .cookie-container {
      margin: 20px;
    }
    .cookie-btn {
      background: url('https://upload.wikimedia.org/wikipedia/commons/3/34/Chocolate_Chip_Cookie.png');
      background-size: cover;
      width: 200px;
      height: 200px;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }
    .cookie-btn:active {
      transform: scale(0.95);
    }
    .score {
      font-size: 2rem;
      margin: 10px;
    }
    .upgrade-btn {
      background-color: #4CAF50;
      color: white;
      border: none;
      padding: 15px 32px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 16px;
      margin: 20px;
      cursor: pointer;
      border-radius: 8px;
    }
    .upgrade-btn:disabled {
      background-color: grey;
      cursor: not-allowed;
    }
  </style>
</head>
<body>
  <h1>Cookie Clicker Game</h1>

  <!-- Display the score -->
  <div class="score">Cookies: <span id="cookie-count">0</span></div>

  <!-- Cookie Button -->
  <div class="cookie-container">
    <button class="cookie-btn" id="cookie-btn"></button>
  </div>

  <!-- Upgrade button -->
  <button id="upgrade-btn" class="upgrade-btn" disabled>Upgrade (Cost: 50 cookies)</button>

  <script>
    let cookieCount = 0;
    let cookiesPerClick = 1;
    const upgradeCost = 50;

    const cookieBtn = document.getElementById('cookie-btn');
    const cookieCountDisplay = document.getElementById('cookie-count');
    const upgradeBtn = document.getElementById('upgrade-btn');

    // Update the cookie count display
    function updateCookieCount() {
      cookieCountDisplay.textContent = cookieCount;
    }

    // Handle cookie button click
    cookieBtn.addEventListener('click', () => {
      cookieCount += cookiesPerClick;
      updateCookieCount();
      
      // Enable upgrade button when enough cookies are collected
      if (cookieCount >= upgradeCost) {
        upgradeBtn.disabled = false;
      }
    });

    // Handle upgrade button click
    upgradeBtn.addEventListener('click', () => {
      if (cookieCount >= upgradeCost) {
        cookieCount -= upgradeCost;
        cookiesPerClick += 1;
        upgradeBtn.disabled = true;
        updateCookieCount();
      }
    });
  </script>
</body>
</html>
