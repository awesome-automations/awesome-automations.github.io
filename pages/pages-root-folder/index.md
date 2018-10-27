---
#
# Use the widgets beneath and the content will be
# inserted automagically in the webpage. To make
# this work, you have to use › layout: frontpage
#
layout: frontpage
header:
  image_fullwidth: header_unsplash_12.jpg
widget1:
  title: "Blog & Portfolio"
  url: 'https://blog.kalavala.net/'
  image: widget-1-302x182.jpg
  text: 'Check out my blog about home automation shannanigans. Itis more of a cheatsheet than a blog at this point.'
widget2:
  title: "Whats the purpose of this web site?"
  url: 'https://www.awesome-automations.com/info/'
  image: awesome-1-302x182.jpg
  text: "To bring all the awesome ideas and automations into a common place for you to get started. The Home Automation community is by far the most creative comunity out there, constantly thinking of automating various things in their life. This site attempts to capture some of that, and we ned your help to make it better!"
  # video: '<a href="#" data-reveal-id="videoModal"><img src="http://phlow.github.io/feeling-responsive/images/start-video-feeling-responsive-302x182.jpg" width="302" height="182" alt=""/></a>'
widget3:
  title: "Awesome-HA"
  url: 'http://www.awesome-ha.com'
  image: awesome-ha-303x182.jpg
  text: '<em>Awesome Home Assistant</em> is a web site that was started by Franck Nijhof to bring all Home Assistant enthusiats together and provide them with a bunch of awesome github respositories to help them get started and share a common purpose!'

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

<!-- <div id="videoModal" class="reveal-modal large" data-reveal="">
  <div class="flex-video widescreen vimeo" style="display: block;">
    <iframe width="1280" height="720" src="https://www.youtube.com/embed/3b5zCFSmVvU" frameborder="0" allowfullscreen></iframe>
  </div>
  <a class="close-reveal-modal">&#215;</a>
</div> -->
