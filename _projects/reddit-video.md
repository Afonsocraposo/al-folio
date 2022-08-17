---
layout: page
title: Reddit Video
description: Implementation of a Python program that generates videos from Reddit portuguese threads, using Text-To-Speech and FFMPEG.
img: /assets/img/projects/reddit-video/reddit-video.jpg
importance: 2
category: fun
---

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/1h7wMlrj6jw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>
<div class="caption">
    Example output of the developed program. Based on the following thread:
    <br>
    <a href="https://www.reddit.com/r/portugal/comments/irapjw/o_que_fazer_aos_covidiotas/">https://www.reddit.com/r/portugal/comments/irapjw/o_que_fazer_aos_covidiotas</a>
</div>

This program uses the Reddit API to scrape through Reddit threads and compile their top comments into a video. To read these comments, the program uses the gTTS (Google Text-to-Speech) Python library. Once it generates all the audio files, it creates the subtitles frames using the Pillow library for image processing. After generating everything, it uses the ffmpeg package for compiling all the resources into a video publishable to Youtube.
