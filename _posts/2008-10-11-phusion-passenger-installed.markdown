--- 
wordpress_id: 47
layout: post
title: Phusion Passenger installed
wordpress_url: http://www.keoko.net/?p=3
---
I eventually decided to install <a href="http://www.modrails.com/">phusion passenger</a> for his simplicity on rails deployment. My first intention was to install Apache + Mongrel, but after reading Sam Ruby's <a href="http://intertwingly.net/blog/2008/04/28/modrails-easy-if-you-are-root">article</a> and the following lines in this <a href="http://www.rubyinside.com/28_mod_rails_and_passenger_resources-899.html">article</a> I changed my mind:
<blockquote>"The de-facto Ruby (and Rails) deployment system seems to change rapidly (remember Apache+FastCGI, then lighttpd+FastCGI, then Apache+Mongrel, then Nginx+Mongrel...?) and while Passenger may or may not be a de-facto standard in a few years' time, it's certainly becoming the standard for now, so jump on board!"</blockquote>
Really quick the install and set up in Ubuntu.
