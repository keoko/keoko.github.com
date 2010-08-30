--- 
wordpress_id: 6
layout: post
title: Apache 2.2 + Wordpress 2.3.3 installed
wordpress_url: http://localhost/wordpress/?p=1
---
I've just installed Apache 2.2 and Wordpress 2.3.3 after looking into some alternatives, mainly ruby-based solutions:

Alternative web servers:
<ul>
	<li>lighttpd + fastcgi (old recommended solution)</li>
	<li>apache + mod_ruby (<a href="http://www.rubyinside.com/no-true-mod_ruby-is-damaging-rubys-viability-on-the-web-693.html">"class sharing issue"</a>)</li>
	<li>nginx + mongrel (seems the de facto RoR solution)</li>
	<li>SCGI?</li>
</ul>
Ruby-based weblogging systems:
<ul>
	<li>typo (complex?)</li>
	<li>mephysto (seems the most appropiate)</li>
	<li>radiant (more a CMS than a blogging system)</li>
</ul>
The decision was made because I'm really new in Ruby and RoR and I would like to start taking notes about my progress in this area
