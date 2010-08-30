--- 
wordpress_id: 132
layout: post
title: Experiences with flash and ejabberd (II)
wordpress_url: http://www.keoko.net/?p=132
---
When I was writing <a href="http://www.keoko.net/2009/02/experiences-with-flash-and-ejabberd/">Flash and ejabberd experiences</a> article, I thought to include all the detailed steps but I was afraid of repeating once more all the steps. However, after having some doubts whether it was really necessary to apply a patch to ejabberd I think it's worth to review some of them.

Below are the steps I followed to test a chat example.
<ol>
	<li>install latest SVN XIFF version (Checked out revision 10959)
<pre>export TEST_DIR=/tmp/xiffstep1
mkdir -p $TEST_DIR/xiff
cd  $TEST_DIR/xiff
svn co http://svn.igniterealtime.org/svn/repos/xiff/trunk/src/org org</pre>
</li>
	<li>install latest SVN ejabberd version (2.0.2)
<pre>cd  $TEST_DIR
svn co http://svn.process-one.net/ejabberd/tags/ejabberd-2.0.2/ ejabberd_src
cd $TEST_DIR/ejabberd_src/src
./configure --prefix=$TEST_DIR/ejabberd
make
sudo su
make install</pre>
</li>
	<li>setup ejabberd and create two users (sender, receiver)
<pre>cd $TEST_DIR/ejabberd/sbin
./ejabberdctl start
./ejabberdctl register user1 localhost user1
./ejabberdctl register user2 localhost user2</pre>
</li>
	<li>get chat example files: <a href="http://www.keoko.net/wp-content/uploads/2009/02/xiffstep1.mxml">xiffstep1.mxml</a> and <a href="http://www.keoko.net/wp-content/uploads/2009/02/testchat.as">TestChat.as</a>. Note this example is based on this <a href="http://paazio.nanbudo.fi/tutorials/flash/xiff-chat-part-1">article</a>.
<pre>cd $TEST_DIR/xiff
wget http://www.keoko.net/wp-content/uploads/2009/02/xiffstep1.mxml
wget http://www.keoko.net/wp-content/uploads/2009/02/testchat.as</pre>
</li>
	<li> create AS3 project "Test1" with eclipse + Flex Builder, import library XIFF and example file into the project and run the application.
<pre>
# run eclipse
File &gt; New &gt; Flex Project -&gt; Project Name: XIFFstep1
Delete &gt; XIFFstep1.mxml
File &gt; Import &gt; General &gt; File System &gt; Next &gt; From directory: $TEST_DIR/xiff
Run &gt; Run</pre>
</li>
	<li>send message through the flash application in the browser window that has been opened.</li>
	<li>check XML/jabber communication.
<ul>
	<li>Request:
<pre>&lt;policy-file-request/&gt;</pre>
</li>
	<li>Response:
<pre>&lt;?xml version='1.0'?&gt;
&lt;stream:stream xmlns='jabber:client' xmlns:stream='http://etherx.jabber.org/streams' id='3095424478' from='localhost' xml:lang='en'&gt;
&lt;stream:error&gt;
&lt;invalid-namespace xmlns='urn:ietf:params:xml:ns:xmpp-streams'/&gt;
&lt;/stream:error&gt;
&lt;/stream:stream&gt;</pre>
</li>
</ul>
</li>
</ol>
As pointed out by Mickaël Rémond <ins datetime="2009-02-08T23:52:39+00:00">in this <a href="http://www.keoko.net/2009/02/experiences-with-flash-and-ejabberd/#comment-30">comment</a></ins>, XIFF is not XMPP compliant. It's clear on the XML request. However, I still don't see how to connect XIFF with ejabberd without applying the patch mentioned on the first part. BOSH seems the only alternative.

Any suggestion?
