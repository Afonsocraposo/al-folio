---
layout: page
title: Genetic Neural Network Flappy Bird
description: Genetic algorithm coupled with neural networks to play Flappy Bird
img: assets/img/projects/genetic-nn-flappy/thumbnail.png
category: python
---

<center>
    <iframe width="560" height="315" src="https://www.youtube.com/embed/1PUq66TIl-U" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>

I developed a genetic algorithm that improves a neural network through out iterations. Like every evolution algorithm, it has three main events: reprodution, mutation, and death. So in each of this events, the weights of a neural network that controlls the behaviour of the Flappy Bird might change.

This makes it that the birds become smarter overall through out time. In the demo above you can see how they compete in each iteration and they progressively improve together.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/projects/genetic-nn-flappy/screenshot.png" title="Screenshot from video" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Screenshot from Youtube video. Here you can see multiple birds competing and learning.
</div>
