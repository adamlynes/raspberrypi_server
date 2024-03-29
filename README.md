<h1>My Pi Server</h1>
This is documentation of how I have setup and configured my Raspberry Pi and references other repos and websites that I have used to get to this stage.<br>
This repo includes the scripts I have deployed on my Raspberry Pi for various applications and containers.

<h2>Initial Setup</h2>

<h3>Flashing the Image</h3>
The OS Image I used is Raspbian lite (32 bit). This deploys the Raspbian OS without any desktop environment, perfect for me as I plan to use the Pi headless and access it only via SSH or the web GUi of any applications I install. I chose 32 bit over 64 bit as I have no need for 64 bit applications on this server.

To flash the image I use the official Rapsberry Pi Imager software and I use the settings options to add some additional security by restricting login to SSH only and adding the SSH key of my computer that I will be primarily accessing the Pi from (A quick Google will find you a guide on how to set up SSH keys, otherwise you can ignore this and just use your username and password.). In this window I also choose my desired hostname, locale settings and I also set my preferred username and password. Even though I won't need these for logging into the server there are occassions when they're needed, such as running some sudo commands. If you're planning on running your Pi via wifi rather than wired LAN then you'll need to enter the WIFI credentials here as well.

I then set the imager on its way and wait for it to finish flashing the image before inserting it into my Pi and powering it up.

<h3>Connecting to the Pi</h3>
If this is the first time I've used this Pi I need to find out it's IP address before I'm able to SSH into it. There a couple of ways to do this but a quick universal way that will work for most people is to download an IP scanning application (Advanced IP scanner on Windows or Angry IP scanner on MacOS) - run the scan and you'll see the hostname of your Pi pop up somewhere on the list. 

At this stage it's also a good idea to reserve that IP on your network for this device so that it remains constant. On most routers or network management applications you can do this by logging into the web page, finding your Pi in the list of clients and choosing the option to use the fixed IP address. You may also be able to change the IP address to one of your choosing as well.

You can now use your client of choice to SSH into the Pi - I usually just use Terminal on Windows or MacOs but there are loads of options out there - and run the command <code>ssh username@192.168.0.0</code> replacing the username and IP address with the values that you have set. Once logged in it's time to start with the install of the applications.

<h2>Overview of Applications</h2>

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

<ul>
<li>Update Script and Cron</li>
<li>SSH Keys of Other Devices</li>
<li>Other Tools</li>
</ul>

<h2>Installing Applications</h2>
<h3>Samba Share</h3>
This creates a network share that I can access from my computer. Useful for updating files and scripts via a GUI rather than navigating via CLI. I usually open files via VS Code and edit them there.
<br>
<ol>
<li>Update the package list and all packages first
<code>sudo apt-get update
sudo apt-get upgrade</code></li>

<li>Install Samba
<code>sudo apt-get install samba</code></li>

<li>I always the use existing "Opt" directory for application files but if you want to create a new directory for sharing then use this command
<code>mkdir /home/pi/shared</code></li>

<li>You now need to edit the "smb.conf" file which stores the settings for the shares.
<code>sudo nano /etc/samba/smb.conf</code>

Use your keyboard arrows to naivate to the bottom of the file and add

<code>[pi_opt]
path = /opt
writeable=Yes
create mask=0777
directory mask=0777
public=no</code>

"[pi_opt]" is the share and you can call this what you prefer, remeber that this is how you will access your share eg <code>//raspberrypi/pi_opt</code>

"path" - chose the directory you want to share

"writeable" - allows the folder to be writeable

"create mask" and "directory mask" - allows maximum permissions for the files and folders - read, write and execute for permissioned users.

"public" - set to "no" to ensure that only a valid user is granted access to the share.</li>

<li>Use CTRL+X, then Y and then Enter to save and exit the file and return to the command line.</li>

<li>Next we need to set up a user that can use the share. I usually use the same username and password I set for the pi, just to keep things simple.

<code>sudo smbpasswd -a pi</code></li> and it will ask you to enter the password twice to set it.

<li>Now to restart the Samba service before we connect

<code> sudo systemctl restart smbd</code></li>

<li>You can now connect to your network share from any device.</li>
</ol>
<h3>Docker & Docker Compose</h3>
I use Docker Compose to deploy application images to my Pi. Rather than having a Pi flashed with just one image, this allows me to have multiple images in separate containers on my Pi.

<ol>
<li>Update and Upgrade

First of all make sure that the system runs the latest version of the software.
Run the command:

<code>sudo apt-get update && sudo apt-get upgrade</code>
</li>
<li>Install Docker

Now is time to install Docker! Fortunately, Docker provides a handy install script for that, just run:

<code>curl -fsSL test.docker.com -o get-docker.sh && sh get-docker.sh</code>
</li>
<li>Add a Non-Root User to the Docker Group

By default, only users who have administrative privileges (root users) can run containers. If you are not logged in as the root, one option is to use the sudo prefix.
However, you could also add your non-root user to the Docker group which will allow it to execute docker commands.

The syntax for adding users to the Docker group is:

<code>sudo usermod -aG docker [user_name]</code>

To add the permissions to the current user run:

<code>sudo usermod -aG docker ${USER}</code>

Check it running:

<code>groups ${USER}</code>
</li>
<li>Install Docker-Compose

Docker-Compose usually gets installed using pip3. For that, we need to have python3 and pip3 installed. If you don't have it installed, you can run the following commands:

<code>sudo apt install docker-compose</code>
</li>
<li>Enable the Docker system service to start your containers on boot

This is a very nice and important addition. With the following command you can configure your Raspberry Pi to automatically run the yDocker system service, whenever it boots up.

<code>sudo systemctl enable docker</code>

With this in place, containers with a restart policy set to always or unless-stopped will be re-started automatically after a reboot.
</li>
<li>Run Hello World Container

The best way to test whether Docker has been set up correctly is to run the Hello World container.
To do so, type in the following command:

<code>docker run hello-world</code>

Once it goes through all the steps, the output should inform you that your installation appears to be working correctly.
</li>
</ol>

<h2>docker-compose.yaml</h2>

<p>Now that Docker is up and running we can start to populate our docker-compose.yaml file. This is the file that tells Docker what containers to install and what parameters we would like to set for them. My docker-compose.yaml is available for you to reference in the "opt" folder, so named as it's the folder that I use on my Rasperry Pi system for keeping all my containers in.</p>

<p>The following sections will run through what services I have defined in my docker-compose.yaml file and explain a bit about what they are, how I use them and my experience of installing them.</p>

<h3>Pi-Hole</h3>
<a href='https://hub.docker.com/r/pihole/pihole'>Office Docker Pi-Hole Page</a>

<p>I use Pi-Hole primarily to block ads across all devices on my network. If you're used to using an ad blocker extension on your browser then this does the same thing across all your devices and all applications. For example when I'm reading my favourite newspaper app on my iPhone I can now scroll through without being inundated with ads. It will block ad trackers and other services that mine your web usage for your data.</p>
<p>It's also useful as a web filter for your home network, you can use an app such as Pi-Hole remote that gives you toggles for blocking popular services like Facebook and Youtube or you can manually configure the domains you want to block on the User interface.</p>

<h2>Other Tools</h2>
Here is a list of of tools that I use to manage my server and it's applications. Some of them are just details on the web front ends of the installed apps, whilst some of them are stand alone apps that and connect to the server and surface the data in a much more digestible way.

- Pi-Hole Remote
- Server Cat
- Home Assistant App
- Portainer






