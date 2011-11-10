In the progress of software development, we spend more time debugging than coding.  
So, to some extent, more easier the way we debug in, more efficiently we develop our applications.  

Accordingly, when developing the applications with node, we also wanna find an elegant way to debug it.
Commonly, we can debug node apps with the _console_ object which exposes several methods used to put some log in the console, 
which more or less takes some effects. This is maybe enough for some small applications but not qualified for large scale applications definitly.
Because this way is neither directviewing nor convenient.  

[joyent](http://joyent.com/) suggests [using eclipse as a node debugger](https://github.com/joyent/node/wiki/Using-Eclipse-as-Node-Applications-Debugger).
Java programmers using eclipse as the development IDE are familiar with this way of debugging.
On the one side eclipse has a debug view in which you can set breakpoints, look into the status of variables in the runtime
and even control the execution flow step by step. This is very powerful. And on the other side, joyent's debugging way is to 
communite with the node app via its remote debug port(default debug port of node is _5858_ ) and make the debug possible, which
is the same way as debugging remote Java programs.  

Here I will introduce another debug tool for node named [node-inspector](https://github.com/dannycoates/node-inspector).
It uses the same way to enable the debug work that it listens the node debug port via [socket.io](http://socket.io)
and uses the browsers' developer tool to debug the node applications.  

The following is the brief tutorial of using node-inspector:  

1.  Prepare a test node application(we can just pick up the example from joyent):  
<pre><code>// dbgtest.js

var sys=require('sys');
var count = 0;

sys.debug("Starting ...");

function timer_tick() {
  count = count+1;
  sys.debug("Tick count: " + count);
  if (count === 10) {
    count += 1000;
    sys.debug("Set break here");
 }
 setTimeout(timer_tick, 1000);
}

timer_tick();</code></pre>

2.  Start this node application in debug mode:  
    *  node --debug[=port] dbgtest.js (default port is 5858)  
    *  node --debug-brk[=port] dbgtest.js(default port is 5858. And this way will force the application to stop at the first line)  
    Here we use second way(--debug-brk). Because we can even know how node internally works when running our applications via this way.  
    When you see this log displayed in the console, it means node runs your code in a debug mode and has stopped your application at the first line:  
    <pre><code>debugger listening on port 5858</code></pre>

3.  Start _node-inspector_:  
Simply run this command: _node-inspector_ in the console, and it works if such log is shown:  
<pre><code>info  - socket.io started
visit http://0.0.0.0:8080/debug?port=5858 to start debugging</code></pre> 

4.  Use browser developer tool to debug the node application(Here we use chrome's developer tool):  
Creat a new tab or window, and type _http://localhost:8080/debug?port=5858_, then you can see a familiar debug view 
which is same as it we have to debug front-end JavaScript code:  
![Debug View](http://farm7.static.flickr.com/6235/6249843270_42121ed082_z.jpg)  

Now, the code has stopped at the first line. And I think there is no need to tell you how to debug it via developer tool. You are must very good at it. Right?!  

Enjoy it!
 