---
layout: page-fullwidth
title: "Multiroom Audio with Raspberry Pis and Speakers over WiFi"
subheadline: "No wiring required!"
meta_teaser: "Want to make your own Multi-Room Audio System that works entirely on your WiFi using nothing but Raspberry Pi’s and portable speakers?"
teaser: "Want to make your own Multi-Room Audio System that works entirely on your WiFi using nothing but Raspberry Pi’s and portable speakers?"
comments: true
header:
    image_fullwidth: header_unsplash_33.jpg
image:
    thumb: multi-room-audio.jpg
    homepage: header_unsplash_33.jpg
categories:
    - multiroom audio
    - raspberrypi
permalink: "/blog/multiroom-audio/"
---

<center><img src="{{site.url}}/images/multi-room-audio.jpg"></center>
<p>Don’t be fooled by the above picture, you will not be making that one! 😊</p>

<h3>Concept</h3>

<p>The concept is simple. There is a server (Runs on a Raspberry Pi) that runs a software called <strong><em>snapcast server</em></strong> that streams audio content, and then the clients (Raspberry Pis) connect to that server, and receive audio at real-time using a software called <strong><em>snapcast client</em></strong>.</p>

<p>When the server streams the content, it encodes each packet with it's local time. When the client receives the packet, it checks the server's local time with client's local time, and client decides if it should skip, fast forward or delay playing the track/stream. By doing so, all the clients start to play the content <strong><em>exactly at the same</em></strong>, thus you get the synchronized effect. When you walk from room to room, the music goes with you!</p>

<p>Best part of this setup is, EVERYTHING runs off of WiFi, and <u>absolutely NO wiring required</u>. The setup is capable to play Text To Speech (TTS) messages, or even music (local mp3 files or spotify...etc).</p>

<h3>Let's get to the details</h3>
The details are maintained on my github repository, and kept up-to-date. [Please follow this link for details](https://github.com/skalavala/Multi-Room-Audio-Centralized-Audio-for-Home)