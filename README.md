<h1>My Pi Server</h1>
This is documentation of how I have my Raspberry Pi setup and references scripts or files I 
This repo includes the scripts I have deployed on my Raspberry Pi for various applications and containers.

<h2>Initial Setup</h2>

<strong>Flashing the Image</strong><br>
OS Image used is Raspbian lite as I have no requirement for a desktop.

I use the settings options to add the SSH key of my computer and restrict to SSH login only.

I also set my desired username and password instead of leaving the default 'pi'

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

<h3>Other setup<h3>

<ul><li>Update Script and Cron</li></ul>

