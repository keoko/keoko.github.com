--- 
wordpress_id: 4
layout: post
title: gizmo installed
wordpress_url: http://keoko.wordpress.com/2007/05/26/test/
---
I have just installed <a href="http://www.gizmoproject.com" title="gizmo">gizmo</a>, another voIP service similar to Skype but using SIP instead,  on my Ubuntu Feisty with some minor issues:
<ol>
	<li>download gizmo deb package from <a href="http://www.gizmoproject.com/download-linux.html" title="linux gizmo file">here</a></li>
	<li>mv ~/.asoundrc.asoundconf ~/.asoundrc.asoundconf.bak</li>
	<li>sudo mv /etc/asound.conf /etc/asound.conf.bak</li>
	<li>mv .asoundrc .asoundrc.bak</li>
	<li>sudo /etc/init.d/alsa-utils restart</li>
	<li>sudo dpkg -i ~/Desktop/gizmo-project_3.0.1.68_i386.deb</li>
</ol>
Now it's up and running but no friend with gizmo to call!
