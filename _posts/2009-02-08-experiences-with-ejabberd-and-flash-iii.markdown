--- 
wordpress_id: 182
layout: post
title: Experiences with ejabberd and flash (III)
wordpress_url: http://www.keoko.net/?p=182
---
<p>Round III, now with BOSH.</p>
<p>After <a href="http://www.keoko.net/2009/02/experiences-with-flash-and-ejabberd/">attempt 1</a> and <a href="http://www.keoko.net/2009/02/experiences-with-flash-and-ejabberd-ii/">attempt 2</a> trying to connect XIFF to ejabberd without applying any patch to ejabberd I finally tried with BOSH connection and IT WORKED!!!</p>
<p>Below are the steps I followed to test a chat example.</p>
<ol>
	<li>install latest SVN XIFF version (Checked out revision 10959)
<pre>export TEST_DIR=/tmp/xiffstep1
mkdir -p $TEST_DIR/xiff
cd  $TEST_DIR/xiff
svn co http://svn.igniterealtime.org/svn/repos/xiff/trunk/src/org org</pre>
</li>
	<li>install latest SVN ejabberd version (revision 1867)
<pre>cd  $TEST_DIR
svn co http://svn.process-one.net/ejabberd/trunk/ ejabberd_src
cd $TEST_DIR/ejabberd_src/src
./configure --prefix=$TEST_DIR/ejabberd
make
sudo su
make install</pre>
</li>
<li>install mod_http_bind in ejabberd and load module in <a href='http://www.keoko.net/wp-content/uploads/2009/02/ejabberd.cfg'>ejabberd.cfg</a> (see this <a href="http://www.ejabberd.im/ejabberd-modules">article</a>). 
<pre>cd $TEST_DIR/
svn co http://svn.process-one.net/ejabberd-modules ejabberd_modules_src
cd $TEST_DIR/ejabberd_modules_src/http_bind/trunk
./build.sh
sudo su
cp ebin/*.beam $TEST_DIR/ejabberd/lib/ejabberd/ebin/
# load mod_http_bind in ejabberd.cfg: {mod_http_bind,[]},
# add request_handler in ejabberd.cfg: {request_handlers, [{["http-bind"], mod_http_bind}]}
cd $TEST_DIR/ejabberd/etc/ejabberd
wget http://www.keoko.net/wp-content/uploads/2009/02/ejabberd.cfg
</pre>
</li>
<li>setup ejabberd and create two users (sender, receiver).
<pre>cd $TEST_DIR/ejabberd/sbin
./ejabberdctl start
./ejabberdctl register user1 localhost user1
./ejabberdctl register user2 localhost user2</pre>
</li>
<li>install nginx and configure it as reverse proxy to ejabberd
<pre>
sudo apt-get install nginx
sudo vim /usr/local/nginx/conf/nginx.conf
# add the following lines after location / block
        location /xmpp-httpbind {
            proxy_pass   http://localhost:5280/http-bind;
            break;
        }
sudo killall nginx
sudo /usr/local/nginx/sbin/nginx
</pre>
</li>
<li>get chat example files: <a href='http://www.keoko.net/wp-content/uploads/2009/02/xiffstep1bosh.mxml'>XIFFstep1BOSH.mxml</a> and <a href='http://www.keoko.net/wp-content/uploads/2009/02/testchatbosh.as'>TestChatBosh.as</a> (this example is based on this <a href="http://paazio.nanbudo.fi/tutorials/flash/xiff-chat-part-1">article</a>).
<pre>cd $TEST_DIR/xiff
wget http://www.keoko.net/wp-content/uploads/2009/02/xiffstep1bosh.mxml
wget http://www.keoko.net/wp-content/uploads/2009/02/testchatbosh.as</pre>
</li>
	<li> create AS3 project "XIFFstep1BOSH" with eclipse + Flex Builder, import library XIFF and example file into the project and run the application.
<pre># run eclipse
File &gt; New &gt; Flex Project -&gt; Project Name: XIFFstep1Bosh
Delete &gt; XIFFstep1BOSH.mxml
File &gt; Import &gt; General &gt; File System &gt; Next &gt; From directory: $TEST_DIR/xiff
Run &gt; Run</pre>
</li>
<li>open another XMPP client (e.g. Psi) with user2.</li>
<li>send message through the flash application in the browser window that has been opened.</li>
<li>check the message has been received in the other XMPP client (user2).</li>
</ol>
<p>Mickaël Rémond was right: <a href="http://www.keoko.net/2009/02/experiences-with-flash-and-ejabberd/#comment-30">XMPP works with flash out of the box</a> ...  <b>with BOSH connections, not with XML socket connections</b>.</p>
