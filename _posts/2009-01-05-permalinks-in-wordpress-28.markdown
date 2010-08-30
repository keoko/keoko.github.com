--- 
wordpress_id: 90
layout: post
title: Permalinks in wordpress 2.8
wordpress_url: http://www.keoko.net/?p=90
---
Looking at my awstats graphs I found out that I had some not exepcted 404 pages (e.g. "/2008/12/programming-erlang/"). I checked it and it was obvious it was caused to the activation of <a href="http://codex.wordpress.org/Permalinks_Options_SubPanel">Wordpress permalinks</a>. After some investigation I realized the issues where caused by:
<ul>
	<li>missing .htaccess. Probably due to file permissions.</li>
	<li>Apache's mod_rewrite module disabled.</li>
	<li>Apache's <a href="http://codex.wordpress.org/Using_Permalinks#Fixing_Other_Issues">Allow Override</a> not enabled.</li>
</ul>

I guess I made all possible mistakes :(
