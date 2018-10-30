---
layout: page-fullwidth
title: "Facial and Object Recognition"
subheadline: "Using Machinebox.io"
meta_teaser: "Want to use machine learning to recognize people and objects using your camera feed? Let's get started!"
teaser: "<em>Facial Recognition</em> using machine learning. Use machine learning technology to see what's on your camera feed, and alert you when it finds something of interest to you!"
comments: true
header:
    image_fullwidth: header_unsplash_32.jpg
image:
    thumb: machine-learning.jpg
    homepage: machine-learning.jpg
categories:
    - machine learning
    - image processing
    - camera
permalink: "/blog/facial-recognition/"
---

## Facial Recognition using Machine Learning
There are many open source packages out there that will allow you to do facial/object recognition from a given image/picture. This particular implementation focuses on [machinebox.io's](https://www.machinebox.io) [facebox](https://machinebox.io/docs/facebox) and [tagbox](https://machinebox.io/docs/tagbox) to detect faces and objects. You can also use the same to train new faces and objects giving you more power, and more importantly EVERYTHING runs locally. [machinebox.io's](https://www.machinebox.io) uses machine learning to detect faces (using faceprint), and you can also train facebox to detect new faces.The ingenuity of machinebox is that you never know you are working with a typical machine learning program. All the machine learning complexity is completely hidden from you, and an extremely easy to use interface and API access is provided to you. That is what we will use to detect and train faces here.

## How do I install Facebox and Tagbox?
Before you install, you need to get a KEY from [machinebox.io](https://machinebox.io/). The key is required for you to run facebox and tagbox. So signup, get your key, and run the following commands to get started. 

### Facebox Install
```
$ MB_KEY="SET YOUR KEY HERE"
$ sudo docker run --name=facebox -p 8080:8080 --restart=always -e "MB_KEY=$MB_KEY" -d machinebox/facebox
```

### Tagbox Install
```
$ MB_KEY="SET YOUR KEY HERE"
$ sudo docker run --name=tagbox -p 8081:8080 --restart=always -e "MB_KEY=$MB_KEY" -d machinebox/tagbox
```

If you are not familiar with docker, the above commands basically downloads, installs and runs the machinebox software. 
The `--restart=always` option basically restarts the application if it crashes for any reason. 
The `-d` option indicates that the application should run as a daemon/background service. That way, after running the command, you can close the terminal, and the application is still running in the background. 
The `-e` option basically gives access to the environment to the docker, so that it can read the MB_KEY. You can also simply pass the key if you don't want to use `-e` option.
The `-p` option sets the port on which the app and container listens on.
The `--name=xxx` option allows you to give custom name. This is completely optional!
The last variable `machinebox/xxxbox` is the actual command you are running.

Please note that the port numbers that the apps are listening are different for each application, but the container is listening on same port #8080. After running the commands, go to the following URLs to access facebox and tagbox respectively.

```
Facebox: http://192.168.1.xxx:8080 
Tagbox:  http://192.168.1.xxx:8081
```

If you already have those ports in use, you have the flexibility to change them in the `docker run ...` command.

**The MB_KEY is same for all machinebox products**, and that key specifies what type of subscription you have - whether it is FREE or Small/Medium/Large Business license. 

[https://www.machinebox.io](https://machinebox.io/) has so many other products like textbox, videobox, and more. We will only explore (hardly) facebox and tagbox in here.

I use a free version, which means the state management is disabled. What that means is that all the training that I do with facebox and/or tagbox is discarded conveniently as soon as I restart the application. Yes, that's a bummer! 

When you use commercial license, there is a way you can export and import data to maintain the state. To get around this problem, I wrote a few scripts that I manually run to re-train the facebox and tagbox on-demand. I may waste a few CPU cycles, but I am okay as long as everything is running locally, and I have plenty of hardware resources that I can spare and scale. I run the script after restarting the docker container(s) - facebox and tagbox. Speaking of docker container, if you haven't used, I **highly** recommend using [Portainer](https://portainer.io/)! You will not regret, it is an awesome way to manage your docker containers.

## Script to train new faces using facebox

Here is the script that I run to re-train faces using facebox. I can use the same concept to train objects using tagbox as well. The script runs on Windows machine. It is where all my pictures are, and it is a pain in the butt to copy over all my pictures to a headless linux/ubuntu operating system just to train faces. Since the script calls curl command, the script is pretty simple.

Facebox allows you to test, train and inspect using the application itself. It also has an API and using `curl` commands you can train additional faces and objects. The following script uses `curl` command and processes all the images in a folder. 

### Folder Structure
```
C:\kalavala\facebox\
....................\suresh\
............................\suresh1.jpg
............................\suresh2.jpg
............................\suresh3.jpg
............................\suresh4.jpg
............................\suresh5.jpg
.....................\mallika\
.............................\mallika1.jpg
.............................\mallika2.jpg
.............................\mallika3.jpg
.............................\mallika4.jpg
.............................\mallika5.jpg
```

### Script to process pictures
There are a few things that you need to change in the script below.

1. `localdir` : Make sure you set the correct folder where the jpg files are located. Make sure you do not have any files other than the images, as the script is written to read all the files. If you have anything other than jpg/png files, you can modify the script to only process image files - using *.jpg or *.png.

2. `IP Address` : Change the IP address of the facebox/tagbox application to your instance's IP

3. `ID` : Make sure you change the ID in the URL. If you manage to screw it up, just restart the container, and you are good to go! Who knew having a free version can be useful at times?

```
@echo off 
setlocal enableDelayedExpansion

echo Processing Suresh's pictures...
set localdir=C:\kalavala\facebox\suresh
for /F %%x in ('dir /B/D %localdir%') do (
  set FILENAME=%localdir%\%%x
  echo Processing !FILENAME!
  curl -X POST -F file=@!FILENAME! "http://192.168.1.xxx:8080/facebox/teach?name=Suresh&id=suresh"
)

echo Processing Mallika's pictures...
set localdir=C:\kalavala\facebox\mallika
for /F %%x in ('dir /B/D %localdir%') do (
  set FILENAME=%localdir%\%%x
  echo Processing !FILENAME!
  curl -X POST -F file=@!FILENAME! "http://192.168.1.xxx:8080/facebox/teach?name=Mallika&id=mallika"
)

echo Done processing pictures.
```
## Note (Update as of October, 2018)

Machinebox recently announced that they support saving the state for free subscription as well. That means, once you train the faces, you do not need to re-train after every restart.