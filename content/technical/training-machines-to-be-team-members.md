
Training Machines to be Team Members

My team has done something amazing: We’ve built “robot” contributors to an Open Source project. “Cockpituous”, the Cockpit project’s #5 contributor, is actually our machine team members.

In fact the project would be dead without its bot contributors. The fundamental architecture of Cockpit would be impossible to execute without machines on our team. As humanity advances, I believe we are hitting boundaries which require us to work with bots tightly integrated into our teams in order to progress. 

As a system gets more complex, the entropy introduced by humans reaches a critical point. This results in stagnation or failure of a team, company or project. The effort of a solely human team does not scale past a certain point.

TLDR; There’s a lot of content here. Stay true to these two fundamentals your tests and automation will show results:

Make changing your tests, bots and automated jobs as easy as changing your software.

Integrate your test feedback, bots and automated jobs into your team’s existing workflow and process.

Some automation looks for profit and inefficiencies: eliminating humans to reduce costs. What I describe falls in another bucket: Where we simply stagnate without it.

My experience is in software, and training machines to be part of software teams. These principles can be applied elsewhere, but in the software development world we have no excuse: We speak the language of machines. 





What is “Cockpit” and why should I care?
Tests are food for machines

What makes a machine a Team Member?
Humans train the Machines every day
Pair Programming with Machines
Machine’s Learning from Human Team Members Behavior

Cloning your bots

Organic self-acting bots

XXXX Bots  processes, don’t reinvent them

Test flakes are Awesome

Make your bots communicate like Humans

Distributed and scalable bots


The fundamental architecture of Cockpit pulls together 100 different components subsystems and APIs of upteen varying Linux operating systems and does so without a midtier layer.

Bots do the mundane tasks that would otherwise use up the time of human contributors. The boot and test our software tens of thousands of times a day. You can see the bots self-organizing, finding bugs, contributing code changes, testing many thousands of times a day, releasing the software into myriad Linux distros and containers weekly. They work in a completely distributed, organic way, and run in containers on Openshift.

We have humans pair-programming with bots, moving with a pace that would be unthinkable otherwise. 

In 
In my team machines aka. bots are a fundamental part of our team. 



TLDR;





Anyone on your team must be able to update tests, automation and bots as easily as they update the software.






XXX we call them bots

My team has done just that. We've trained machines to act as team members. Here's how we know:

Reason: Contributions

The fifth highest contributor on the Cockpit team, as measured in commits to the code base is our bots. We literally have cases where:

 1. A bot will open an issue
 2. Other bots figuring out that they can perform a task or fix for that issue.
 3. A pull request with a change to the codebase is proposed by a bot.
 4. Bots come and test that pull request across umpteen operating systems and browsers.
 5. Finally a human gets involved and checks out that pull request, and either merges it or throws it away.

The bots go by the name of "cockpituous" on GitHub, and you can see them here in the contributor graphs:

[Image]

But that's just the tip of the iceberg, most of the bots work team don't show up in those pretty graphics. They do the menial tasks in the project that would otherwise waste the time of the human contributors. People spend up to half their week at work doing routine boring work. This is what the machines do:

Testing of Cockpit across 10 operating systems 15,000 times a day

This means booting and installing Cockpit tens of thousands of times a day. The operating systems and variants are: Fedora Atomic Host, Fedora Server, Red Hat Enterprise Linux 7 (released), RHEL (development), RHEL Atomic Host, Debian Testing, Debian 9, Ubuntu LTS, Ubuntu 17.04, CentOS 7

Testing Cockpit on 3 different browsers thousands of times a day

Again the bots start up browsers and test Cockpit in them. The browsers are: Firefox, Chrome, Internet Explorer

Weekly releases of Cockpit into 5 Linux distributions

Once a human signs a tag in git, bots come and do all the work to push that out as a release across various distributions: Fedora, RHEL, Debian, Ubuntu, Atomic Host

This includes building tarballs, RPMs, Deb files, building changes for all the distributions, publishing the change, announcements, and more.

This work used to take a human more than a day for each release.

Building and pushing container images of Cockpit

Again, publishing container images of Cockpit, its Kubernetes and Openshift dashboards is all handled by bots. Driven by the

Uploading documentation

Machines build and publish this guide: http://cockpit-project.org/guide/

Updating test root images

All that heavy testing of Cockpit requires periodic updates of test root images of each operating system. Each of those updates uncovers umpteen bugs in the respective operating system, which the bots test for, and raise in a pull request.

Pulling in translations to Cockpit

The bots retrieve updates to translations regularly and incorporate them into Cockpit in a pull request.

Updating and testing javascript dependencies

Cockpit depends on all sorts of javascript libraries. Javascript libraries are notoriously unpredictable. Each one needs to be tested carefully to ensure it works as expected.

Tracking known issues in other software

Not as intelligent and creative team members, but team members nonetheless. The bots do the mindnummingly boring tasks, that humans on typical teams have to spend their time on.


Reason: Cockpit would be dead

Cockpit would be DOA without machines on our team.

Cockpit is a Linux session in a web browser. Javascript code in the browser talks directly with around 100 different APIs and commands on the system. And yet Cockpit aims to be compatible with various Linux systems and their umpteen versions.

Without machines aggressively testing and gating this massive matrix of software the project would be completely doomed.


Reason: Pair Programming with Machines

XXXX

Reason: The team stops without the machines

XXXX

Reason:  XXX









= What makes a Machine a Team Member? =

If you've looked over the sort of tasks that the bots on the Cockpit team do, you'd think they're pretty mundane scripts and glue.

By the way this is true of any natural process. If you look to closely at plants or wildlife, all you see is goo. It's when you take a step back, that you see what's really happening.

And it's true, that the bots carry out their tasks using basic scripts and commands. Many of them shared with other projects.

So what makes them special? It's one special ingredient:

XXXXX the team members can train them XXXXXX

Shared responsibility for the bots



== Reason: The team can train them ==

== They adapt to the changes on the team ==




= Tests are Basic Training for Machines =



= Humans train the Machines every Day =



= Feedback from the Tests make them Grow =
 * Push test results into a human's workflow



Test flakes are food not poison

The bane of every continuous integration system are “flakes” aka. “false positives”. These are those nasty, annoying failures that happen intermittently during testing. Murphy’s law dictates that they always happen at the most painful moments.

An active testing system of sufficient size and complexity will have flakes. If your testing system doesn’t have flakes, you’re not testing hard enough. So how do we turn this latent horror XXX into something useful?

Flakes rough fall into three categories. It’s common to have an even split between the following:
Infrastructure problems
Bugs in your tests
Bugs in your project or software

Let's look at these in reverse order.

XXXX Mutations
Bugs in your project or software
If you have effective continuous integration, you have an army of machines testing your project. They’re gonna find bugs. These are bugs that happen once every thousand or ten-thousand test runs. They’re real bugs that’ll show up at a user’s system or customer’s site.

Think of your testing like a fuzzer. Fuzz testing 

XXX Infrastructure problems are easy to fix and if not, i’ve got bad news for you

XXX Bugs in your tests


= Automate Existing Processes, don't Reinvent them =
Then you’ll have time to reinvent them
Make the machines act like human team members

= Distributed scalable machines =


= Make your machines communicate via Human Processes =
XXX Make it possible for a human to impersonate a machine.



= Organic, self-acting machines =
XXX Machines that carry around their own brain




= Cloning your team members =
Cloning the team mindset



= Applying machine learning to your Team =
Flakes 
Learning from humans
Good actors

