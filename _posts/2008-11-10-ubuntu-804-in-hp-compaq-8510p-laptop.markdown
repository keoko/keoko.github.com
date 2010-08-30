--- 
wordpress_id: 48
layout: post
title: Ubuntu 8.04 in HP Compaq 8510p laptop
wordpress_url: http://www.keoko.net/?p=5
---
I installed successfully Ubuntu 8.04 in a HP Compaq 8510p laptop. My applications at the moment are:
<ul>
	<li>text editor: vim 7.1</li>
	<li>web browser: Firefox 3.0.3 (webdeveloper, firebug, firecookie, YSlow, vimperator, del.icio.us)</li>
	<li>IM client: pidgin 2.4.1</li>
	<li>java editor: eclipse 3.3.2</li>
	<li>web server: apache2 2.2.8 (php5, "mod_ruby")</li>
	<li>gnome-do (quicksilver for gnome)</li>
	<li>glipper (similar to klipper)</li>
	<li>network protocol analyzer: wireshark</li>
	<li>virtual machine: VirtualBox 1.5.6_OSE with Windows XP.</li>
	<li>oracle client: SQLDeveloper</li>
</ul>
There is still a list of issues pending to fix:
<ul>
	<li>battery life. I followed the tips (powertop, cpufrequtils,  ...) of this <a href="http://www.phoronix.com/scan.php?page=article&amp;item=ubuntu_battery_life&amp;num=1">article</a> although not sure anot the improvement.</li>
	<li>screen resolution. 1200x800 maximum resolution. I installed fglrx driver through envy but does not improve the resolution (1680*1050 seems the maximum, see this <a href="https://wiki.ubuntu.com/LaptopTestingTeam/HPCompaq8510p">article</a>).</li>
	<li>CD/DVD writer. brasero nor cdrecord command tool worked.</li>
	<li>video codecs. AVI files are reproduced very slowly.</li>
	<li>email client with Exchange support. Evolution does not manage properly meeting requests.</li>
	<li>glipper. It crashes in every start up.</li>
	<li>rubygems. Rubygems from repository does not work properly. You have to install it manually.</li>
</ul>
