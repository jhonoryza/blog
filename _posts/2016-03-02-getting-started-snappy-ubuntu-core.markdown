---
layout: post
title: "getting started snappy ubuntu core"
date: "2016-03-02 13:49:06 +0700"
---
Let's see how it feels to get things done the Snappy way. You need to be logged in to your Ubuntu Core instance to try these commands, they won't work on a traditional apt-get or deb-based Ubuntu system!

We'll start by checking which version of Ubuntu Core we're on.

    $ snappy info
    release: ubuntu-core/15.04/stable
    architecture: armhf
    frameworks:
    apps:

This is a pristine system, freshly installed with no frameworks and no apps. The "release" tells you that you are running the latest Ubuntu Core's stable release, which is the recommended image channel for starting with Snappy. Note that In the future, we'll have LTS versions of Ubuntu Core too.

    $ snappy list -v

Name | Date | Version Developer
ubuntu-core | 2015-06-11 3 | ubuntu*   
ubuntu-core | 2015-06-11 3 | ubuntu    
beagleblack | 2015-06-11 1.7.1 | *         

Again, this is a pristine system so it only has two components installed, the base system layer (ubuntu-core) and the beagleblack appliance.

The active flag (\*) tells you that this version of the component is what is currently running. If you do an update, you will get the newer version of the system to be active when you reboot. (In the case above you see two entries of ubuntu-core, one being active. This is because we have one installed on each a- and b-partition.

Since this is a snappy system, the old mechanisms for package install and updates don't work!

    $ apt-get update
    Ubuntu Core does not use apt-get, see 'snappy --help'
    $ sudo apt-get install docker
    Ubuntu Core does not use apt-get, see 'snappy --help'

OK, we're not in Kansas any more :)

Let's go ahead and see if there is an updated system for us. The update‐versions command will check the store for newer versions of any installed components. If you are fully up to date, you will see the same versions listed which are currently installed:

    $ snappy list -uv

Name   |     Date   |    Version
ubuntu-core | 2015-06-11 | 3       
beagleblack | 2015-06-11 | 1.7.1

You might see an updated system, but leave it for now, we'll have some fun with it later. For now, let's take a look for a package that we can install.

    $ sudo snappy install fizzler
    Installing fizzler
    fizzler failed to install: snappy package not found

Looks like there's a gap in the market! What about something popular, like Docker?

    $ snappy search docker

Name  | Version |  Summary
docker | 1.6.0.002 | Docker  

Looks like there is a docker package for Ubuntu Core, let's check it out!

    $ sudo snappy install docker
    Installing docker
    Starting download of docker
    8.36 MB / 8.36 MB [==========================================================] 100.00 % 419.70 KB/s
    Done

Name        | Date     |  Version |  Developer
ubuntu-core | 2015-06-11 | 3     |    ubuntu    
docker      | 2015-06-11 | 1.6.1.002 |           
beagleblack | 2015-06-11 | 1.7.1 |           

OK, so we got a nice progress meter, and now we know that Docker is installed. Let's verify that by asking for all the components again:

    $ snappy list

Name    |    Date    |   Version  | Developer
ubuntu-core | 2015-06-11 | 3     |    ubuntu    
docker      | 2015-06-11 | 1.6.1.002           
beagleblack | 2015-06-11 | 1.7.1               

And there we have a clear picture: we have the base Ubuntu Core system, and docker. What else can we install?

    $ sudo snappy install hello-world
    Installing hello-world.canonical
    Starting download of hello-world.canonical
    32.73 KB / 32.73 KB [========================================================] 100.00 % 617.46 KB/s
    Done

Name     |   Date    |   Version |  Developer
ubuntu-core | 2015-06-11 | 3     |    ubuntu    
docker      | 2015-06-11 | 1.6.1.002 |           
hello-world | 2015-06-11 | 1.0.17  |  canonical
beagleblack | 2015-06-11 | 1.7.1    |             

Now, let's get that overview again:

    $ snappy info
    release: ubuntu-core/15.04/stable
    architecture: armhf
    frameworks: docker
    apps: hello-world

So docker is actually a framework, and hello‐world is an app.

Both are installed with snappy. The difference between them has to do with security and isolation — frameworks extend the base system, so they have to have custom crafted security profiles. Apps are isolated from one another by default, they have limited permissions to poke around on the system, which means they don't need any manual review, they can go straight from the vendor to you, directly.

This was easy, wasn't it?

Interested in exploring more of the snappy world? You can use snappy tool to search and find new apps and frameworks currently available for your system. Give it a try:

    $ snappy search webserver

Name          |       Version | Summary              
go-example-webserver | 1.0.7 |  go-example-webserver     
xkcd-webserver   |    0.5   |  xkcd-webserver       

Go ahead and play with those. Point a browser at the virtual machine (if you are running locally with KVM we might have mapped the VM port 80 to your localhost port 8090, hot top!).

Let's talk a bit about updating. Snappy has a simple "update" command to install a newer version of any component. Let's give it a go. If there is an available update for ubuntu-core, you would see something like this:

    $ sudo snappy update ubuntu-core
    Installing ubuntu-core (4)
    Syncing boot files
    Starting download of ubuntu-core
    8.81 KB / 8.81 KB [==========================================] 100.00 % 463 B/s
    Apply done
    Updating boot files
    8.81 KB / 8.81 KB [==========================================] 100.00 % 412 B/s
    Done

Name     |   Date   |    Version Developer
ubuntu-core | 2015-06-11 4   |    ubuntu!   
Reboot to use the new ubuntu-core.

Where do we stand now? The system update mechanism allows two or three builds of Ubuntu to be installed at the same time, one currently running and the others ready for immediate use on reboot. So this is how we see what will become active on reboot:

    $ snappy list -v

Name      |  Date    |   Version |  Developer  
ubuntu-core | 2015-06-11 | 3 |        ubuntu*    
ubuntu-core | 2015-06-11 | 4    |     ubuntu!    
docker      | 2015-06-11 | 1.6.1.002  | *          
hello-world | 2015-06-11 | 1.0.17  |  canonical*
beagleblack | 2015-06-11 | 1.7.1 |    *       
   
Reboot to use the new ubuntu-core.

You can see that the base system image "ubuntu-core" has been installed, but it is not activated yet. You will have to reboot your system at your leisure to activate the new core.

    $ sudo reboot

SSH back to the instance after reboot. It won't take long, this is snappy Ubuntu! Now take a look at the versions of your software, and you'll see that a different version of ubuntu-core is active:

    $ snappy list -v

Name     |   Date   |    Version |  Developer  
ubuntu-core | 2015-06-11 | 4  |       ubuntu*    
ubuntu-core | 2015-06-11 | 3   |      ubuntu     
docker      | 2015-06-11 | 1.6.1.002 | *          
hello-world | 2015-06-11 | 1.0.17  |  canonical*
beagleblack | 2015-06-11 | 1.7.1   |  *          

You have the new base system running but you can still go back. With snappy systems, returning to the preview version of a component is as simple as:

    $ sudo snappy rollback ubuntu-core

Setting ubuntu-core to version 3

Name       |     Date  |     Version Developer
ubuntu-core | 2015-06-11 | 2       ubuntu!   

Reboot to use the new ubuntu-core.

    $ snappy list -v

Name        | Date   |    Version |  Developer  
ubuntu-core | 2015-06-11 | 4 |        ubuntu*    
ubuntu-core | 2015-06-11 | 3  |       ubuntu!    
docker      | 2015-06-11 | 1.6.1.002 | *          
hello-world | 2015-06-11 | 1.0.17  |  canonical*
beagleblack | 2015-06-11 | 1.7.1   |  *    

Reboot to use ubuntu-core version 3 .

    $ sudo reboot

... and we are now back on our previous version of ubuntu-core.
