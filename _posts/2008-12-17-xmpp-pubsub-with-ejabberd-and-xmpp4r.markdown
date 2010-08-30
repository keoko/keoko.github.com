--- 
wordpress_id: 49
layout: post
title: XMPP PubSub with ejabberd and XMPP4R
wordpress_url: http://www.keoko.net/?p=6
---
<p>After reading <a href="http://en.oreilly.com/oscon2008/public/schedule/detail/4359">&#8220;Beyond REST? Building Data Services with XMPP PubSub&#8221;</a> and other articles about PubSub with XMPP, I decided it would be worth to test it. However, I didn&#8217;t find any complete step by step guide in how to test it with ejabberd as XMPP server and XMPP4R as XMPP client.</p>
<p>Below are the steps I followed to test a subscriber (user &#8220;sub&#8221;) waiting for items published to the node (&#8221;home/localhost/pub/updates&#8221;) for the publisher (user &#8220;pub&#8221;). Note you can add as many subscribers/publishers as you want.</p>

<ol>
<li>install ejabberd (XMPP server). I followed instructions of this <a href="http://ecedeno.com.ve/blog/index.php/2008/08/07/servidor-jabber-con-ejabberd-en-ubuntu-hardy-804/">article</a> (spanish) or <a href="http://sysmonblog.co.uk/?p=11">article</a> (english) without any surprise.</li>
<li>create two ejabberd users: &#8220;pub&#8221; (the publisher) and &#8220;sub&#8221; (the subscriber).<br />

<pre name="code" class="c">sudo ejabberdctl register pub localhost pub
sudo ejabberdctl register sub localhost sub </pre></li>

<li>install XMPP4R Ruby gem.<br />
<code>sudo gem install xmpp4r</code></li>

<li>create file nodecreator.rb. See code below.
<pre name="code" class="ruby">
#! /usr/bin/ruby
require &quot;rubygems&quot;
require &quot;xmpp4r&quot;

require &quot;xmpp4r/pubsub&quot;
require &quot;xmpp4r/pubsub/helper/servicehelper.rb&quot;
require &quot;xmpp4r/pubsub/helper/nodebrowser.rb&quot;
require &quot;xmpp4r/pubsub/helper/nodehelper.rb&quot;

include Jabber
Jabber::debug = true

service = &#39;pubsub.localhost&#39;
jid = &#39;pub@localhost/laptop&#39;

password = &#39;pub&#39;
client = Client.new(JID.new(jid))
client.connect
client.auth(password)

client.send(Jabber::Presence.new.set_type(:available))
pubsub = PubSub::ServiceHelper.new(client, service)
pubsub.create_node(&#39;home/localhost/pub/&#39;)
pubsub.create_node(&#39;home/localhost/pub/updates&#39;)
</pre>
</li>

<li>create file publisher.rb. See code below.</li>
<pre name="code" class="ruby">
#! /usr/bin/ruby
require &quot;rubygems&quot;

require &quot;xmpp4r&quot;
require &quot;xmpp4r/pubsub&quot;
require &quot;xmpp4r/pubsub/helper/servicehelper.rb&quot;
require &quot;xmpp4r/pubsub/helper/nodebrowser.rb&quot;
require &quot;xmpp4r/pubsub/helper/nodehelper.rb&quot;
include Jabber
Jabber::debug = true
jid = &#39;pub@localhost/laptop&#39;

password = &#39;pub&#39;
service = &#39;pubsub.localhost&#39;
node = &#39;home/localhost/pub/updates&#39;
# connect XMPP client
client = Client.new(JID.new(jid))
# remove &quot;127.0.0.1&quot; if you are not using a local ejabberd
client.connect(&quot;127.0.0.1&quot;)
client.auth(password)
client.send(Jabber::Presence.new.set_type(:available))
# create item
pubsub = PubSub::ServiceHelper.new(client, service)
item = Jabber::PubSub::Item.new
xml = REXML::Element.new(&quot;greeting&quot;)
xml.text = &#39;hello world!&#39;

item.add(xml);
# publish item
pubsub.publish_item_to(node, item)
</pre>
<li>create file subscriber.rb. See code below.
<pre name="code" class="ruby">

#! /usr/bin/ruby
require &quot;rubygems&quot;
require &quot;xmpp4r&quot;
require &quot;xmpp4r/pubsub&quot;
require &quot;xmpp4r/pubsub/helper/servicehelper.rb&quot;

require &quot;xmpp4r/pubsub/helper/nodebrowser.rb&quot;
require &quot;xmpp4r/pubsub/helper/nodehelper.rb&quot;include Jabber
#Jabber::debug = true
jid = &#39;sub@localhost/laptop&#39;
password = &#39;sub&#39;
node = &#39;home/localhost/pub/updates&#39;
service = &#39;pubsub.localhost&#39;

# connect XMPP client
client = Client.new(JID.new(jid))
# remove &quot;127.0.0.1&quot; if you are not using a local ejabberd
client.connect(&quot;127.0.0.1&quot;)
client.auth(password)
client.send(Jabber::Presence.new.set_type(:available))
sleep(1)
# subscribe to the node
pubsub = PubSub::ServiceHelper.new(client, service)
pubsub.subscribe_to(node)
subscriptions = pubsub.get_subscriptions_from_all_nodes()
puts &quot;subscriptions: #{subscriptions}\n\n&quot;
puts &quot;events:\n&quot;

# set callback for new events


pubsub.add_event_callback do |event|
begin
event.payload.each do |e|
puts e,&quot;----\n&quot;
end
rescue
puts &quot;Error : #{$!} \n #{event}&quot;

end
# infinite loop
loop do
sleep 1
end
</pre>
</li>

<li>run nodecreator.rb. It creates the XMPP node &#8220;home/localhost/pub/updates&#8221;. It creates first the node &#8220;home/localhost/pub&#8221; and then the &#8220;home/localhost/pub/updates&#8221;. Seems quite obvious but I spent some hours after I got it.</li>

<li>check the nodes have been created. I used the discovery service functionality of Psi client.
<p><img src="http://keoko.files.wordpress.com/2008/12/psi_service_discovery.png?w=300" alt="psi_service_discovery" title="psi_service_discovery" width="300" height="288" class="alignnone size-medium wp-image-67" /></li>

<li>run subscriber.rb file. The &#8220;sub&#8221; user subscribes to the node &#8216;updates&#8217; and waits for items in the &#8220;updates&#8221; node. Be aware you should close any XMPP connection with users &#8220;pub&#8221; and &#8220;sub&#8221; in case you are using any XMPP client such as Pidgin, Psi,&#8230; otherwise it won&#8217;t work.

<li>run publisher.rb file. It will send a message &#8220;&lt;greetings&gt;hello world!&#8221;&lt;/greetings&gt; to the subscriber. Run it as many times as you want.</li>

<li>If everything goes well yo will see in the subscriber screen a message like this.
<pre name="code" class="xml">

subscriptions: &lt;subscription node=&#39;home/localhost/pub/updates&#39; jid=&#39;sub@localhost&#39; subscription=&#39;subscribed&#39; xmlns=&#39;http://jabber.org/protocol/pubsub&#39;/&gt;

events:
&lt;items node=&#39;home/localhost/pub/updates&#39;&gt;&lt;item id=&#39;3376&#39;&gt;&lt;greeting&gt;hello world!&lt;/greeting&gt;&lt;/item&gt;&lt;/items&gt;
----
</pre>
</li>
</ol>
<p>Good Luck!</p>

<p>For this test, I used the following versions:</p>
<ul>
<li>Operating System: Ubuntu 8.04</li>
<li>Ruby:  1.8.6</li>
<li>XMPP4R: 0.4</li>
<li>ejabberd: 1.1.4</li>
</ul>
