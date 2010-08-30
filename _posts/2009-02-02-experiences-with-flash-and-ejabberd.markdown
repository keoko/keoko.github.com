--- 
wordpress_id: 98
layout: post
title: Experiences with flash and ejabberd
wordpress_url: http://www.keoko.net/?p=98
---
After finding out  a <a href="http://ralphm.net/">site</a> with a fishtank where each fish notified the presence of some jabber user (now it's a christmas tree) I thought it would be interesting to do the same with flash and add some more dynamism (e.g. using events, mood, ...) to the fishes.

The rest of the story is here:
<ol>
	<li>So the first thing was to find a <strong>Flash library</strong> to do all the XMPP communication with ejabberd through BOSH. Rapidly I googled and all links  pointed to <a title="XIFF" href="http://www.igniterealtime.org/projects/xiff/">XIFF</a>. So far, so good.
</li>
	<li>Some <strong>IDE to work with Flash in linux</strong>. Eclipse with  <a href="http://www.adobe.com/products/flex/features/flex_builder/">Flex Builer</a> to the rescue following this <a href="http://www.insideria.com/2008/04/step-by-step-setting-up-flex-b.html">notes</a>. Some java issues, but not more than 1 hour to have Flex "Hello world" application up and running.</li>
	<li>Let's look at some <strong>XIFF examples</strong> ... and what? there is no example nor even documentation ... except for some 4 examples on this <a href="http://paazio.nanbudo.fi/tutorials/flash">site</a>: <a href="http://paazio.nanbudo.fi/tutorials/flash/xiff-chat-part-1">chat</a>, <a href="http://paazio.nanbudo.fi/tutorials/flash/xiff-chat-part-2-roster">roster</a>, <a href="http://paazio.nanbudo.fi/tutorials/flash/xiff-chat-part-3-chat-room">multiuser chat room</a> and <a href="http://paazio.nanbudo.fi/tutorials/flash/xiff-chat-part-4-invitations">invitations</a>.</li>

	<li><strong>Test the examples with ejabberd</strong> and ... failed!!! This was getting harder than expected. Let's check the Flex traces what was wrong. No way. I was about to cry when I got to a <a href="http://www.sephiroth.it/weblog/archives/2008/04/flash_switcher_for_windows_osx_and_li.php"> site</a> that recommended me to remove the distribution flashplugin package and install the Adobe's one and ... well it worked.
<pre name="code" class="c">
sudo apt-get remove flashplugin-nonfree
</pre>
</li>
	<li>With Flex trace now working I didn't get too many answers, so I dug into the jabber packets with <a href="http://www.wireshark.org/">Wireshark</a> and discovered the XMPP stream communication was closed unexpectedly.  Time to google and see I needed to apply a patch to ejabberd to work with flash.</li>
	<li>At this point I was actually crying.  What was supposed to be a couple of hours of fun, it was being a trip to hell!!! So, with tears in my eyes I <strong>installed Erlang</strong> R12B-5 following these <a href="http://http://ciarang.com/posts/compiling-erlang-on-ubuntu">steps</a>. Downloaded ejabberd 2.0.2, applied the patch, unpacked it, run configure script, compiled and installed it. WHAT???</li>
	<li>I said ejabberd 2.0.2 and the patch I found was for <a href="http://erlangdevelopers.splinder.com/post/16639798">2.0.0</a>. Full of tears I adapted the patch for ejabberd 2.0.2 and then applied the patch, unpacked it, run configure script, compiled and installed it.</li>
	<li>Voilá! <strong>Chat</strong> (first example) working with AS3.</li>
	<li><strong>Roster</strong> (second example) failed with below AS3 exception.
<pre name="code" class="java">
Error: No class registered for interface 'mx.resources::IResourceManager'
</pre>
</li>
	<li>At this point I was looking how to express my level of depression with my jabber mood. Googling and googling finally I got to this <a href="http://www.igniterealtime.org/community/thread/34519">post</a> that recommended me to <strong>load the affected class at the very beginning of the application</strong> and so I did it including the following lines in my class constructor. It worked.

<pre name="code" class="java">
// http://www.igniterealtime.org/community/thread/34519
// Do what FlexModuleFactory does, only by hand.
var resourceManagerImpl:Object = flash.system.ApplicationDomain.currentDomain.getDefinition("mx.resources::ResourceManagerImpl");
//mx.core.Singleton.registerClass("mx.resources::IResourceManager", Class(resourceManagerImpl));
Singleton.registerClass("mx.resources::IResourceManager", Class(resourceManagerImpl));
</pre>

</li>
	<li>Time to test <strong>multiuser chat room</strong> (third example). Press debug button and then what??? What the **ll is doing??? I knew I'd just entered to the world of pain. I was feeling the pain until I stumbled upon <a href="http://www.iminlikewithyou.com">iaminlikewithyou</a> and played balloono until 3 AM. This site was really cool and seemed that they were using XMPP for the game communication but not really sure.</li>
	<li><strong>Multiuser chat room (round II)</strong>. I read the example code and realized that I had to create a multiuser chat room and invite the user from an XMPP client (e.g. Pidgin) to work. That was not so difficult but the morale was already killed in the 5th step.</li>
	<li><strong>Invitations</strong> (fourth example) was really easy probably at this stage I was trained to stand any suffering.</li>
	<li>No mood to create any fishtank nor christmas tree!!!</li>
</ol>
<p>To sum up:</p>
<ul>
	<li><strong>XIFF</strong>
<ul>
	<li>no documentation</li>
	<li>no examples</li>
	<li>not work with AS3 out of the box</li>
	<li>support for chat, roster, MUC, invitations and BOSH although the latter I've not tested.</li>
</ul>
</li>
	<li><strong>ejabberd</strong>
<ul>
	<li><del datetime="2009-02-08T23:45:03+00:00">not work with Flash out of the box.</del> <ins datetime="2009-02-08T23:45:03+00:00">work with flash out of the box ... with BOSH connection, not with XML socket connection (see <a href="http://www.keoko.net/2009/02/experiences-with-ejabberd-and-flash-iii/">Experiences with ejabberd and flash (III)</a></ins>.</li>
</ul>
</li>
</ul>
