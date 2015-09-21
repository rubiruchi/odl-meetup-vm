# odl-meetup-vm
This repo has a Vagrant setup for creating a VM to run OpenDaylight Lithium with Mininet.

I tested this against VirtualBox 4.3.r26 and Vagrant for MAC OSX 1.7.2. You can
create and launch the VM simply by installing the VirtualBox and Vagrant software,
cloning this repo, and doing:

    vagrant up



   cd distribution-karaf-0.3.1-Lithium-SR1/bin/
   rm -rf ../snapshots/ ../journal/ ../data/*
   ./karaf clean
   karaf: JAVA_HOME not set; results may vary

                                                                                           
<p>You should see the following banner appear:</p>

    ________                       ________                .__  .__       .__     __       
    \_____  \ ______   ____   ____ \______ \ _____  ___.__.|  | |__| ____ |  |___/  |_     
     /   |   \\____ \_/ __ \ /    \ |    |  \\__  \<   |  ||  | |  |/ ___\|  |  \   __\    
    /    |    \  |_> >  ___/|   |  \|    `   \/ __ \\___  ||  |_|  / /_/  >   Y  \  |      
    \_______  /   __/ \___  >___|  /_______  (____  / ____||____/__\___  /|___|  /__|      
            \/|__|        \/     \/        \/     \/\/            /_____/      \/          
                                                                                           

    Hit '<tab>' for a list of available commands
    and '[cmd] --help' for help on a specific command.
    Hit '<ctrl-d>' or type 'system:shutdown' or 'logout' to shutdown OpenDaylight.


<p>At the karaf console, enter the following<p>


    opendaylight-user@root>feature:install odl-l2switch-switch-ui
    opendaylight-user@root>log:tail | grep L2SwitchMainModule


You should eventually see the following
    L2SwitchMain (instance org....) initialized.

<p>navigate to http://192.168.50.70:8181/index.html (username/password is admin/admin)<p>

You should be able to see three openflow switches, labeled openflow:1,
openflow:2, and openflow:3

     sudo mn --controller=remote,ip=127.0.0.1 --mac --topo=linear,3 --switch ovsk,protocols=OpenFlow13
    *** Creating network
    *** Adding controller
    *** Adding hosts:
    h1 h2 h3 
    *** Adding switches:
    s1 s2 s3 
    *** Adding links:
    (h1, s1) (h2, s2) (h3, s3) (s1, s2) (s2, s3) 
    *** Configuring hosts
    h1 h2 h3 
    *** Starting controller
    *** Starting 3 switches
    s1 s2 s3 
    *** Starting CLI:
    mininet> pingall
    *** Ping: testing ping reachability
    h1 -> h2 h3 
    h2 -> h1 h3 
    h3 -> h1 h2 
    *** Results: 0% dropped (6/6 received)
    mininet> 
