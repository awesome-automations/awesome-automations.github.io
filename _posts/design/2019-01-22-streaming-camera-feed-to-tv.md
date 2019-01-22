---
layout: page-fullwidth
title: "Stream Camera Live Feed to TV, Raspberry Pi, or PC"
subheadline: "Stream Camera Live Feed to TV, Raspberry Pi or PC"
meta_teaser: "Do you have IP Cameras and want to stream camera live feed to your TV?"
teaser: "If you are looking to stream your live camera feed to your TV using simple steps, you've come to the right place!"
comments: true
header: yes
image:
    thumb: homeassistant.png
    homepage: header_unsplash_26.jpg
categories:
    - camera
    - Chromecast
mediaplayer: false
permalink: "/blog/camera2tv.md/"
---

<center><img src="{{site.url}}/images/homeassistant.png"></center>

### Prerequisites
* A Television (Or any TV/Monitor with a HDMI port should suffice)
* [IP Camera](http://amzn.to/2suiPhT)(s) with RTSP enabled
* A Chromecast device
* [Home Assistant](https://www.home-assistant.io) (A fantastic home automation software to make your dreams come true)

### Step 1: Setting up Chromecast

If you are new to Google's Chromecast device, it is time to get yourself familiarize with it. It is basically a very inexpensive and a small device that connects to the HDMI port of your TV and makes your TV a "smart" TV. 
The Chromecast device also comes with plenty of apps, where you can use effectively turn your dumb TV into a really smart one in no time. Setting up the device is easy, just install the Google Home App on your phone, and follow the instructions. 

Once you have Google's Chromecast device configured, restart your Home Assistant application. If you have the `discover:` option enabled, it will tell you that it found a new device and will let you automatically add that Chromecast device as a "media_player" in your Home Assistant. Remember the name (entity_id) of the Chromecast media player, you are going to use it later! Now a bit more information about Chromecast device:

The Chromecast device has the built-in capability to play live web stream - usually in the form of HTTP Live Stream (also known as HLS). Most of the IP based cameras that we have today support RTSP (Realtime streaming protocol).

For your Chromecast to play your camera feed, somehow the RTSP stream needs to be converted to HLS. For that, we will use program called [streamer](https://github.com/gihad/streamer) that does real-time stream conversion. It not only converts the stream, but also runs a web server on a specific port, where you can access the streams/camera feeds. For that, it uses NGINX (a reverse proxy), that dos the port translation. For example, you can chose to run the streamer on a port `9999`, the nginx receives the HTTP requests on the port `9999`, and forwards that request to port `80` running inside the docker container.

### Step 2: Setting up Streamer

The author of the streamer component did a fantastic job in documenting how to set it up. You can read the instructions [here](https://github.com/gihad/streamer), or run the following command:

**Before running the command, make sure you change the username, password and the IP addresses/path of the camera URL. Without this, the docker container will not start!**

```
docker run -e PARAMETERS="rtsp://username:password@192.168.xxx.xxx:xxx/cam/realmonitor?channel=1&subtype=1 frontyard" -v /tmp/stream:/tmp/stream -p 8080:80 gihad/streamer
```

If you have more than one camera, you simply add the camera URL and the name of the camera in the PARAMETERS. You can have as many cameras as you want. The following command streams two cameras.

```
docker run -e PARAMETERS="rtsp://username:password@192.168.xxx.xxx:xxx/cam/realmonitor?channel=1&subtype=1 frontyard rtsp://username:password@192.168.xxx.xxx:xxx/cam/realmonitor?channel=2&subtype=1 driveway" -v /tmp/stream:/tmp/stream -p 8080:80 gihad/streamer
```

If you want to run the docker container in the background, just add `-d` option to the command line. You may also want to add restart option to restart always - in case if it crashes for any reason.

TIP: I use [HikVision](http://amzn.to/2suiPhT) cameras in my setup, and the URLs for the [HikVision](http://amzn.to/2suiPhT) camera is usually in the format `http://username:password@192.168.xxx.xxx/ISAPI/Streaming/channels/101/picture`. The URL is typically different from vendor to vendor, you can't copy and paste the URL and expect it to work.

After running the docker, you should be able to access the camera stream using HTTP Live Streaming. To test it, Install VLC media player on your computer, and go to File -> Open network stream -> and paste the following URL:

```
http://192.168.xxx.xxx:8080/driveway.m3u8
```

Make sure you change the IP address and the camera name in the URL. If the VLC player plays the camera stream, that means everything is setup correctly. You can move on to the next step - which is streaming that to the TV.

### Step 3: Home Assistant Configuration

Now that you have connected Chromecast device to your TV, configured Chromecast as a `media_player`in Home Assistant, installed and running `Streamer` successfully, it is time to write a few lines of script to bring all of them together. The following code is from my [GitHub repository](https://github.com/skalavala/smarthome/blob/master/packages/cameras.yaml). What this does is anytime there is a "motion" detected in the frontyard, it turns ON the media player and plays the HLS frontyard stream to the chromecast media player that is connected to TV. The TV automatically shows the camera feed coming from the Chromecast.

```
  - alias: Frontyard Camera Feed To TV
    initial_state: true
    hide_entity: false
    trigger:
     - platform: state
       entity_id:
         - binary_sensor.motion_sensor_158d00024ee084
       to: 'on'
    action:
      - service: media_player.turn_on
        entity_id: media_player.attic_tv
      - service: media_player.play_media
        data:
          entity_id: media_player.attic_tv
          media_content_id: !secret frontdoor_camera_stream_url
          media_content_type: video
```

### Known Issues:
WHile streaming the live camera feed through Chromecast to TV is fantastic, if there is no activity, the chromecast may detect that and may go into screensaver mode. When it goes into screensaver mode, it randomly displays beautiful static images. There are techniques you can use to work around that, by changing the streams every few minutes, adding more dynamic content...etc.

Special thanks to [@quadflight](https://github.com/quadflight) for the idea behind using Chromecast and streamer combination. That man is full of ideas, and also the reason for my many sleepless nights! :)
