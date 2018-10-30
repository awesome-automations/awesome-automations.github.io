---
layout: page-fullwidth
title: "Restart & Update Home Assistant using GUI"
subheadline: "Restart or Upgrade your HA using GUI"
meta_teaser: "How do you restart or upgrade your Home Assistant system?"
teaser: "Here is how you can simply restart or upgrade your home assistant system using a few mouse clicks instead of using SSH or console. This comes in handy when you are too lazy to log into your Home Assistant server to do the restart or upgrade to a newer version."
comments: true
header:
  image_fullwidth: header_unsplash_20.jpg
image:
    thumb: homeassistant.png
    homepage: header_unsplash_20.jpg
categories:
    - homeassistant
permalink: "/blog/restart-ha/"    
---
<center><img src="{{site.url}}/images/restart-ha-gui.jpg"></center>
<p>&nbsp;</p>
<p>Follow the two simple steps to make your HASS Restart and even Upgrade to newer version (if available) without using command line interface (CLI). Please note that if there are any existing configuration errors, your upgrade might not be successful. Also, If the upgrade has any breaking changes, your Home Assistant might failed to load some or all. Please read the breaking changes section before upgrading.</p>

Also, if you have not used `hassctl`, it is HIGHLY recommended that you take a minute and set it up. You can find the `hassctl` <a target="_blank" href="https://github.com/dale3h/hassctl">here</a>.

### Step 1: Add the following to the sudoers by running the following command 

<em><strong>PLEASE NOTE THAT IT IS EXTREMELY DANGEROUS TO EDIT SUDOERS FILE USING YOUR FAVORITE EDITOR. YOU WILL NOT BE ABLE TO BOOT! USE THE RIGHT COMMAND, AND BE CAREFUL NOT TO MAKE ANY STUPID MISTAKES. DOUBLE CHECK EVERYTHING BEFORE SAVING.</strong></em>

```
sudo visudo
```

Enter the following contents to your SUDOERS file. You might already have most of the content, in that case, just add the last 3 lines.

```
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d

# Allow homeassistant to use hassctl
homeassistant ALL=(ALL) NOPASSWD: /bin/systemctl
homeassistant ALL=(ALL) NOPASSWD: /bin/journalctl
```

### Step 2: Add the following to a package/file and restart your HASS

```
homeassistant:
  customize:
  
    script.update_hass:
      friendly_name: Update Home Assistant
      icon: mdi:home-assistant

    script.restart_hass:
      friendly_name: Restart Home Assistant
      icon: mdi:home-assistant

script:
  update_hass:
    sequence:
      - service: shell_command.update_hass
  restart_hass:
    sequence:
      - service: shell_command.restart_hass

shell_command:
  restart_hass: >-
    hassctl restart

  update_hass: >-
    hassctl update-hass && hassctl config && hassctl restart
```

Make sure you restart your Home Assistant server to see the UI. Hopefully, this will be the last time you use your console/cli/ssh to restart your home assistant server.
