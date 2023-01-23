<h1>Home Assistant Configuration</h1>

<h2>HACS</h2>

<h3>What is it?</h3>
Home Assistant Community Store. It provides a user interface and download and manage custom components; integrations, lovelace cards and themes. These custom components can also be manually installed by dropping them into the custom_components folder

I rely on this heavily for these custom components 

<h3>How to install</h3>

https://hacs.xyz/docs/setup/download

<h3>Nest Protect</h3>

The Nest integration that is baked in to HASS doesn't include functionality for Nest Protects so orginally I was relying on the HACS custom component provided by iMickNL https://github.com/iMicknl/ha-nest-protect
Unfortunately Google broke the authentication he was using so it wasn't working for a good while. I recently found a fork by nicoinch https://github.com/nicoinch/ha-nest-protect which provides an alternative way to login using auth headers and cookies.

