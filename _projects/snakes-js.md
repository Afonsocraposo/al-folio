---
layout: page
title: snakes.js
description: A fun Javascript game based on the classic Snake game
img: assets/img/projects/snakesjs.png
category: web
---

In this game, two players (1v1) can compete to see which can grow the largest snake, while also being able to eat the other player if he’s smaller.

I developed this game in one afternoon! I was on vacation with my girlfriend and she challenged me to develop a complete game while she took a nap after lunch. As soon as she fell asleep, I started creating this. When she woke up, we had a fresh game to play together!

Explore the source code [here](https://www.github.com/afonsocraposo/snakes.js/).

### Try it!

<body>
    <p>1v1 snake game</p>
    <canvas id="canvas" width="512" height="512" style="border:1px solid #000000;"></canvas>
    <h3>Settings</h3>
    <form method="get">
        <label for="FPS">FPS:</label>
        <select id="FPS" name="FPS">
          <option value="5">5</option>
          <option value="10">10</option>
          <option value="15" selected>15</option>
          <option value="20">20</option>
          <option value="25">25</option>
          <option value="30">30</option>
        </select>
        &nbsp;&nbsp;
        <label for="SIZE">Snake Size:</label>
        <select id="SIZE" name="SIZE">
          <option value="4">4</option>
          <option value="8">8</option>
          <option value="16" selected>16</option>
          <option value="32">32</option>
          <option value="64">64</option>
        </select>
        &nbsp;&nbsp;
        <label for="WORLD">World Size:</label>
        <select id="WORLD" name="WORLD">
          <option value="128">128</option>
          <option value="256" selected>256</option>
          <option value="512">512</option>
          <option value="1024">1024</option>
        </select>
        &nbsp;&nbsp;
        <input type="submit" value="Submit">
    </form>
    <h3>Instructions</h3>
    <p>
    <b>Controls:</b>
    <br>
    <b>Snake 1:</b> W A S D &nbsp;&nbsp; <b>Snake 2:</b> I J K L
    </p>
    <p>
    If you are the biggest snake and hit the head of the other snake, you win.
    </br>
    If you hit any other part of the snake, you lose.
    </br>
    If both snakes are the same size and hit their heads, it's a tie.
    </p>

</body>
<script src="/assets/js/snakes.js"></script>
