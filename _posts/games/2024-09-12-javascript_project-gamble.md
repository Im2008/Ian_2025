---
title: Gamble
layout: post
description: GAMBILING
categories: [Javascript]
permalink: /javascript/project/gamble
menu: nav/javascript_project.html
toc: true
comments: true
---

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simple Gambling Game</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
      background-color: #2d3436;
      color: white;
    }
    h1 {
      margin-bottom: 20px;
    }
    .balance, .result {
      font-size: 1.5rem;
      margin: 10px 0;
    }
    .bet-input {
      font-size: 1.2rem;
      padding: 10px;
      margin-bottom: 20px;
      width: 100px;
      text-align: center;
    }
    .bet-btn {
      padding: 10px 20px;
      font-size: 1.2rem;
      background-color: #00b894;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .bet-btn:hover {
      background-color: #55efc4;
    }
    .message {
      margin-top: 20px;
      font-size: 1.2rem;
      color: #d63031;
    }
  </style>
</head>
<body>
  <h1>Gambling Game</h1>

  <!-- Display player's balance -->
  <div class="balance">Balance: $<span id="balance">100</span></div>

  <!-- Input for the bet amount -->
  <input id="bet-input" class="bet-input" type="number" min="1" placeholder="Bet amount">

  <!-- Button to place the bet -->
  <button id="bet-btn" class="bet-btn">Place Bet</button>

  <!-- Result of the bet (win/lose) -->
  <div id="result" class="result"></div>

  <!-- Message for errors or instructions -->
  <div id="message" class="message"></div>

  <script>
    let balance = 100; // Initial balance
    const balanceDisplay = document.getElementById('balance');
    const betInput = document.getElementById('bet-input');
    const resultDisplay = document.getElementById('result');
    const messageDisplay = document.getElementById('message');
    const betBtn = document.getElementById('bet-btn');

    // Update the balance display
    function updateBalance() {
      balanceDisplay.textContent = balance;
    }

    // Handle placing a bet
    betBtn.addEventListener('click', () => {
      const bet = parseInt(betInput.value);

      // Check if the bet is valid
      if (isNaN(bet) || bet <= 0) {
        messageDisplay.textContent = "Please enter a valid bet amount!";
        return;
      }

      if (bet > balance) {
        messageDisplay.textContent = "You don't have enough money to place that bet!";
        return;
      }

      // Reset the message and result
      messageDisplay.textContent = '';
      resultDisplay.textContent = '';

      // 50/50 chance of winning or losing
      const isWin = Math.random() < 0.5;

      if (isWin) {
        balance += bet; // Player wins, increase balance
        resultDisplay.textContent = `You won $${bet}!`;
        resultDisplay.style.color = '#00b894';
      } else {
        balance -= bet; // Player loses, decrease balance
        resultDisplay.textContent = `You lost $${bet}.`;
        resultDisplay.style.color = '#d63031';
      }

      updateBalance();

      // Check if the player is out of money
      if (balance <= 0) {
        messageDisplay.textContent = "You're out of money! Game over.";
        betBtn.disabled = true;
        betInput.disabled = true;
      }
    });

  </script>
</body>
</html>
