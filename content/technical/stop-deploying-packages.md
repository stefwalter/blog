Title: Stop deploying packages
Date: 2016-04-18 13:35
Tags: linux, technical, kubernetes
Slug: stop-deploying-packages
Category: Linux
Summary: Packages shouldn't be assembled as you deploy a system. You shuold treat tools that assemble packages while deploying a production system are legacy tools.

Packages shouldn't be assembled as you deploy a system to production. You shouldn't use tools that assemble packages while deploying a production system. Here's why.

<div style="text-align: center">
<img alt="Be prepared to stop" src="../images/be-prepared-to-stop.jpg">
</div>

Packages are for when you're building a system or developing an application. Whether your building it to tinker, contribute, or building a system that will later be deployed into production. Tools like ```apt-get install``` or ```yum install``` or ```npm install``` or any of another hundred other package installers shouldn't be run on your in-production systems. Use those tools as part of your build process.

<h3 style="text-align: center">Packages are build components</h3>

This applies to Linux distribution packages, RPMs, NPM packages, Ruby Gems, JAR files, and so on. Those packages are build components of an application or operating system. Those packages go into your images, your containers, your applications, and your file system trees.

When it comes time to deploy those systems, services or applications, don't assemble these parts all over again. Use what you assembled previously during development, the parts you integrated and tested. Deploy exactly those bits.

An example: Any production system that was broken by the [NPM fiasco with left-pad in March 2016](http://www.theregister.co.uk/2016/03/23/npm_left_pad_chaos/) got this wrong. Build systems should have broken, not deployments.

<h3 style="text-align: center">It wasn't always this way</h3>

Do you remember a time when you built your Linux or BSD systems from source every time on every machine? In fact, you can still do that today. There is even fancy tooling that lets you assemble stuff on your system from source with just a couple commands. But we moved beyond that. Binary packages were born. We wanted a reproducible, verifiable, composable abstraction.

To this day, source code is still really important and a vital part of development, troubleshooting, not to mention a corner stone of [free software](http://www.gnu.org/philosophy/free-sw.en.html#content).

We're moving on again. Packages are becoming build components, and not the deployable atoms of a system. For some people this is old news ... but others haven't yet gotten the memo.

<h3 style="text-align: center">What's the alternative?</h3>

Modern systems and services are deployed as images, file system trees, or the entire "userland" of the operating system, as seen in containers.

This holds whether we're talking about modern mobile applications, Docker containers, or operating systems like [Atomic Host](http://www.projectatomic.io/) or [CoreOS](https://coreos.com/) or [ChromeOS](https://www.chromium.org/chromium-os), plans like [XDG apps](https://en.wikipedia.org/wiki/Xdg-app), and the [list goes on](http://0pointer.net/blog/revisiting-how-we-put-together-linux-systems.html) and on. Even certain languages like [Golang](https://golang.org/) (annoyingly) produce an entire userland statically linked as their build output.

<h3 style="text-align: center">What about upgrades?</h3>

Thanks for asking. But again, friends don't let friends assemble software on their production systems. If you're running ```apt-get update``` or ```yum update``` on a production system you're asking for trouble. Packages are part of your build process, whether an update or a new installation.

<h3 style="text-align: center">Is it solved?</h3>

This move beyond packages is not done. Stands to reason, or I wouldn't be writing about it. Many of these post-package solutions have poor inventory, security updates, updating mechanisms.

But the mindshift has already happened. The movement is underway and has significant momentum. By thinking about things in terms of the new reality we finish building out the remaining gray areas.

Packages are development tools.

<sup>Note: I changed a bit of the wording, after getting feedback from initial reviewers. Hopefully it helps the idea carry through better.</sup>
