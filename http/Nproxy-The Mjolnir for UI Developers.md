As a UI developer for enterprise-level softwares, I have to do a bunch of building tasks everyday:

* update code
* compile Java code
* download/update packages via maven or solve dependency via ivy
* ...
* merge/compress static files
* deploy to server
* ...

That's a really crappy thing and costs lots of time, especially with terrible network and building issues. Sometimes, I have to wait for building for a long time and get a "Build Fail" message at last. Then what I can do is to copy the damn error track and sent it to the whole team to ask for help and wait, wait ,wait. 

The following chart is the reflection of my work flow:

     TimeLine                  open
         +       Computer <-------------  Me(UI Develoepr)
         |          +                            +
         |          |                            |
         |          v                            v
         |    +-------------+            +----------------+
         |    | update code |            |                |
         |    +-------------+            | have a cup of  |
         |          +                    | coffee         |
         |          |                    |                |
         |          v                    | have another   |
         |    +-------------+            | cup of coffee  |
         |    | build task#1|            |                |
         |    |-------------|            | have lots of   |
         |    | build task#2|            | coffee         |
         |    +-------------+            |                |
         |    |     ...     |            | I hate coffee  |
         |    |-------------|            |                |
         |    | build task#n|            |      ...       |
         |    +-------------+            |                |
         |          +                    |      ...       |
         |          |                    |                |
         |          v                    |      ...       |
         |    +-------------+            |                |
         |    |Build Success|            | have ... coffee|
         |    +-------------+            +----------------+
         |                                       +
         |                                       |
         |                                       v
         |                               +----------------+
         |                               |     coding     |
         |                               +----------------+
         |                                       +
         |    +----------------+                 |
         |    | ant deploy-js  |<----------------+
         |    +----------------+
         |           +
         |           |                   +----------------+
         |           +------------------>|     coding     |
         |                               +----------------+
         |                                       +
         |    +----------------+                 |
         |    | ant deploy-css |<----------------+
         |    +----------------+
         |
         |
         |          ...                         ...
         |
         v

Actually, these building tasks are necessary for Back-End Developers but not for UI Developers. What we cares are only three things: HTML, CSS and JavaScript. All of them are dynamic and don't require most of these building tasks. 

As you can see in the chart, everytime I change my code, I have to run deploy task to do something like merging several js files into one, adding i18n messages referred by JavaScript files and deploying it to jboss server, which is the most painful things to me. 

What I expect is to change the code, refresh the page and then it comes with all my changes.

Think about proxy? Bingo. It is exactly proxy that can save "my life". I can replace files from server requested by browser with local ones. So, I start finding proxy softwares which meet my requirement. Here lists my basic requirement:

* **Support Mac, Linux and Windows**: Mac is mandatory. I work on it. Windows is recommended, for I'll debug some issues in IE. If Mac and Windows are supported, why just leaving Linux alone?
* **Support replacing single file(html,css,js,images)**
* **Support replacing combo files with separated source files**: Most JavaScript and CSS files will be merged and compressed in production, which makes it hard to debug.Proxy should replace merged file with its source files(local files)

There are four proxy softwares caught my eyes: [Fiddler](http://www.fiddler2.com/fiddler2/), [Charles](http://www.charlesproxy.com/), [Rythem](http://www.alloyteam.com/2012/05/web-front-end-tool-rythem-1/) and [Tinyproxy](https://banu.com/tinyproxy/). I compared them according to my requirements:

     +---+---------+---------+---------+-----------+
     | R | Fiddler | Charles | Rythem  | TinyProxy |
     |---------------------------------------------|
     | A |  Win    |    Y    | Mac&Win | Mac&Linux |
     |---------------------------------------------|
     | B |   Y     |    Y    |   Y     |    N      |
     |---------------------------------------------|
     | C |   N     |    N    |  Y/N    |    N      |
     +---------------------------------------------+

     A: Support Mac, Linux and Windows
     B: Support single file prelacing
     C: Support combo file replacing
     Y: Yes
     N: No
     Y/N: Not fully supported

* Rythem is developed with Qt, it should have supported Linux, but I find it is hard to install in Linux. It provides installer for Mac and Windows only.
* Rythem supports group replacing but not any files from different directories.

Though these proxies have such drawbacks as listed in the above table, each one has its own advantage. For instance, Fiddler is a really great tool to inspect http/https request, it does good in listing request line and inspecting request header and body.

As shown in the above table, combo file replacing are all not support. In practice, this functionality is really useful to debug in the production. This is the reason why I wanna introduce [Nproxy](http://goddyzhao.me/nproxy) here. 

[Nproxy](http://goddyzhao.me/nproxy) is a cli proxy tool specialized in file replacing supporting both single file and combo file. And of course it meets all of my requirements. And it'll support plugins for some special cases.

More details informations about how to use Nproxy can be found [there](http://goddyzhao.me/nproxy).

Nproxy is just like a Mjolnir to me and Using nproxy improves my work efficiency to a large extent. 

     TimeLine                   open
     +             Computer <-------------  Me(UI Develoepr)
     |                +                            +
     |                |                            |
     |                v                            v
     |   +--------------------------+      +-------------------+
     |   | update code if necessary |      | have a cup of tea |
     |   +--------------------------+      +-------------------+
     |                +                            +
     |                |                            |
     |                v                            v
     |   +--------------------------+      +-------------------+
     |   | build task if necessary  |      | have a cup of tea |
     |   +--------------------------+      +-------------------+
     |                +
     |                |
     |                v
     |   +--------------------------+
     |   | replacing file via nproxy|
     |   +--------------------------+
     |                +                    +-------------------+
     |                +------------------> |      coding       |
     |                                     +-------------------+
     |                                              +
     |                                              |
     |                                              v
     |                                     +-------------------+
     |                                     |      coding       |
     |                                     +-------------------+
     |                                              +
     |                                              |
     |                                              v
     |                                     +-------------------+
     |                                     | focus on coding   |
     |                                     +-------------------+
     |
     v


As you can see from the above chart, there is no need to deploy during the development. And I don't need to update code and build tasks until other colleagues change some code my code depends on. Cool, right!

Last but not least, [Nproxy](http://goddyzhao.me) is open sourced. Any contributions and suggestions are all welcomed!
