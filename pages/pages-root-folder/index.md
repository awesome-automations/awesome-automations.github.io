---
#
# Use the widgets beneath and the content will be
# inserted automagically in the webpage. To make
# this work, you have to use › layout: frontpage
#
layout: frontpage
header:
  image_fullwidth: header_unsplash_13.jpg
widget1:
  title: "Hello & Welcome to our web site!"
  url: '/getting-started/'
  image: 'welcome-1-302x182.jpg'
  text: "Welcome to the <em><strong>awesome-automations.com</strong></em> web site! I am excited to bring you all of the cool and wonderful automations into one place to help with your home automation and smart home setup. There are a few things that you need in your setup prior to leveraging the automations we have on this web site. Let's get started!"
widget2:
  title: "What's the purpose of this web site?"
  url: '/info/'
  image: awesome-1-302x182.jpg
  text: "To bring all the <em><strong>awesome</strong></em> ideas and automations into a central place for you to get started. The Home Automation community is by far the most creative community out there, constantly thinking of automating various things in their life. This site captures the essence of that, and we need your help to make it better!"
widget3:
  title: "Awesome-HA Just for you!"
  url: 'http://www.awesome-ha.com'
  image: awesome-ha-303x182.jpg
  text: "<em><strong>Awesome</strong> Home Assistant</em> is a web site that was started by Franck Nijhof to bring all Home Assistant enthusiasts together and provide them with a bunch of awesome github repositories to help them get started and share a common purpose! If you haven't had a chance to visit that web site, it is not too late. Click on the link below!"

#
# Use the call for action to show a button on the frontpage
#
# To make internal links, just use a permalink like this
# url: /getting-started/
#
# To style the button in different colors, use no value
# to use the main color or success, alert or secondary.
# To change colors see sass/_01_settings_colors.scss
#
callforaction:
  url: https://tinyletter.com/skalavala
  text: Inform me about new updates and features ›
  style: alert
permalink: /index.html
#
# This is a nasty hack to make the navigation highlight
# this page as active in the topbar navigation
#
homepage: true
---
