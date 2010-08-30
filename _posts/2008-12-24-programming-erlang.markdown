--- 
wordpress_id: 75
layout: post
title: Programming Erlang
excerpt: My review of "Programming Erlang".
wordpress_url: http://keoko.wordpress.com/?p=75
---
My current interests in XMPP leaded me to <a href="http://www.ejabberd.im/">ejabberd</a> (XMPP server) and this to some XEPs <a href="http://xmpp.org/extensions/xep-0060.html">PubSub</a> and POSH, and this to wondering about using XMPP as a queue system. Then I discovered some discussions comparing AMQP vs XMPP and realized that both <a href="http://www.rabbitmq.com/">RabbitMQ</a> (an implementation of AMQP) and ejabberd were programmed in Erlang. I was astonished that three of the currently most interesting projects were implemented in the same language (if we include <a href="http://couchdb.apache.org/">couchDB</a>).

This is why I decided to read <a href="http://www.pragprog.com/titles/jaerlang/programming-erlang">"Programming Erlang"</a>.  I've nearly read half of the book and my expectations has been really exceeded:
<ul>
<li>smooth pace of the book goes from basic knowledge to the hardest part (sequential programming -&gt; advanced sequential programming -&gt; concurrent programming -&gt; distributed programming -&gt; programming multicore CPUs).<li>
<li>clear and easy to understand examples.</li>
<li>some complete examples such as an IRC Lite implementation gives you the concepts in action.
<li>interesting concepts: functional programming, concurrency, distributed programming, fault-tolerant programs.
</ul>

As missing parts (not sure if they are cover at the end of the book), I found out the following:
<ul>
<li>a bit of history of Erlang. Why erlang?</li>
<li>Comparison between Erlang and other concurrency oriented languages such as Scala, Clojure, Stackless Python, ...
</ul>

To sum up, really a <b>MUST READ</b> if you are curious about other programming languages apart from Java, Python or Ruby.
