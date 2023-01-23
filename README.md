<h1>My Pi Server</h1>
This is documentation of how I have setup and configured my Raspberry Pi and references other repos and websites that I have used to get to this stage.<br>
This repo includes the scripts I have deployed on my Raspberry Pi for various applications and containers.

<h2>Initial Setup</h2>

<strong>Flashing the Image</strong><br>
The OS Image I used is Raspbian lite (32 bit). This deploys the Raspbian OS without any desktop environment, perfect for me as I plan to use the Pi headless and access it only via SSH or the web GUi of any applications I install. I chose 32 bit over 64 bit as I have no need for 64 bit applications on this server.

To flash the image I use the official Rapsberry Pi Imager software and I use the settings options to add some additional security by restricting login to SSH only and adding the SSH key of my computer that I will be primarily accessing the Pi from (A quick Google will find you a guide on how to set up SSH keys, otherwise you can ignore this and just use your username and password.). In this window I also choose my desired hostname, locale settings and I also set my preferred username and password. Even though I won't need these for logging into the server there are occassions when they're needed, such as running some sudo commands. If you're planning on running your Pi via wifi rather than wired LAN then you'll need to enter the WIFI credentials here as well.

I then set the imager on its way and wait for it to finish flashing the image before inserting it into my Pi and powering it up.

<strong>Connecting to the Pi</strong>
If this is the first time I've used this Pi I need to find out it's IP address before I'm able to SSH into it. There a couple of ways to do this but a quick universal way that will work for most people is to download an IP scanning application (Advanced IP scanner on Windows or Angry IP scanner on MacOS) - run the scan and you'll see the hostname of your Pi pop up somewhere on the list. At this stage it's also a good idea to reserve that IP on your network for this device so that it remains constant. On most routers or network management applications you can do this by logging into the web page, finding your Pi in the list of clients and choosing the option to use the fixed IP address. You may also be able to change the IP address to one of your choosing as well.

You can now use your client of choice to SSH into the Pi - I usually just use Terminal on Windows or MacOs but there are loads of options out there - and run the command <code>ssh username@192.168.0.0</code> replacing the username and IP address with the values that you have set.

<h2>Overview of Applications</h2>

This sections provides an overview of what is installed on my server, a later section details installation instructions for each.

<h3>Applications installed to the base System</h3>

<ul>
<li>Docker</li>
<li>Docker Compose</li>
<li>Tailscale</li>
<li>Samba</li>
</ul>

<h3>Applications installed to Docker containers</h3>
<ul>
<li>readsb</li>
<li>FR24</li>
<li>Portainer</li>
<li>Pihole</li>
<li>Home Assistant</li>
</ul>

<h3>Other setup</h3>

<ul><li>Update Script and Cron</li></ul>

