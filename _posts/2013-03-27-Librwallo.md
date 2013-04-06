---
layout: post
title: Librwallo
---

![Librwallo](http://farm9.staticflickr.com/8118/8625135933_2cbdc878cb_z.jpg)

How to build a [Librato](https://librato.com) dashboard powered by a [Raspberri Pi](http://www.raspberrypi.org/). Or, how to auto-boot a [Raspberri Pi](http://www.raspberrypi.org/) to a fullscreen webpage.

Step 1: Buy some stuff
----------------------
You'll need a [Raspberry Pi](http://www.amazon.com/gp/product/B009SQQF9C?ie=UTF8&camp=213733&creative=393177&creativeASIN=B009SQQF9C&linkCode=shr&tag=johnwulffcom-20&psc=1), [a case](http://www.amazon.com/gp/product/B008TCUXLW?ie=UTF8&camp=213733&creative=393177&creativeASIN=B008TCUXLW&linkCode=shr&tag=johnwulffcom-20&psc=1) for it, a [power supply](http://www.amazon.com/gp/product/B005LFXBJG?ie=UTF8&camp=213733&creative=393185&creativeASIN=B005LFXBJG&linkCode=shr&tag=johnwulffcom-20&psc=1), and a [memory card](http://www.amazon.com/gp/product/B007JRB0SS?ie=UTF8&camp=213733&creative=393185&creativeASIN=B007JRB0SS&linkCode=shr&tag=johnwulffcom-20&qid=1365287985&sr=8-1&keywords=sd+card). The Raspberri Pi comes with onboard ethernet but if you want to use WiFi, you'll want a [USB Wifi Adapter](http://www.amazon.com/gp/product/B003MTTJOY?ie=UTF8&camp=213733&creative=393185&creativeASIN=B003MTTJOY&linkCode=shr&tag=johnwulffcom-20&psc=1).

Any display that the Raspberri Pi can drive will work but I recommend something with a lot of pixels (1080p or better). I went with this one, [Samsung 32-Inch LED HDTV](http://www.amazon.com/gp/product/B00BCGRXD8?ie=UTF8&camp=213733&creative=393185&creativeASIN=B00BCGRXD8&linkCode=shr&tag=johnwulffcom-20&psc=1). You're probably going to want to put it on the wall, so get a [wall mount](http://www.amazon.com/gp/product/B002TZ4CRG?ie=UTF8&camp=213733&creative=393185&creativeASIN=B002TZ4CRG&linkCode=shr&tag=johnwulffcom-20&psc=1) too. Don't forget an [HDMI cable](http://www.amazon.com/gp/product/B005T3LKKM?ie=UTF8&camp=213733&creative=393185&creativeASIN=B005T3LKKM&linkCode=shr&tag=johnwulffcom-20).

Step 2: Setup Raspberri Pi
--------------------------
Prepare the SD card for the Raspberri Pi and boot it up following the [quick start guide](http://www.raspberrypi.org/quick-start-guide).

Step 3: Networking
------------------
The Raspberri Pi DHCP

Step 4: Browser Auto-Launch
---------------------------

### Disable Sleep
To prevent the Raspberri Pi from going to sleep:

    sudo nano -w /etc/lightdm/lightdm.conf

Add this to `[SeatDefaults]` section

    xserver-command=X -s 0 dpms

### Hide Mouse Cursor
Install unclutter, it'll hide the mouse cursor after a while.

    sudo apt-get install unclutter

Configure unclutter to luanch at boot.

    sudo sh -c 'echo "@unclutter" >> /etc/xdg/lxsession/LXDE/autostart'


### Configure Browser
Install Chromium

    sudo apt-get install chromium-browser

Configure Chromium to launch at boot in kiosk mode. Replace `http://www.nytimes.com` with what you'd really like to see.

    sudo sh -c 'echo "@chromium --kiosk http://www.nytimes.com --incognito" \ 
    >> /etc/xdg/lxsession/LXDE/autostart'
