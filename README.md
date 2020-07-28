Ssh into your server from windows with PuTTY or you can just use terminal if you have linux or mac.

$ ssh root@IPAddress

Welcome to Ubuntu 20.04 (GNU/Linux 2.6.32-042stab120.16 x86_64)

This is your user for logging in via SSH. You can always escalate to root once you get in there if you need to. Now I’ll edit parts of the ssh config at /etc/ssh/sshd_config:
# What ports, IPs and protocols we listen for
Port 22222
I’ve changed the ssh port to something different. Just make sure that you remember what it is, because that’s how you’ll be accessing your server via ssh.


We’ve made sure only a non-root user can log in via SSH and changed the SSH port. We’ve also given our new user, adalovelace, the power of sudo.
Now switch back to your user (su username) and install MHN. This is probably the easiest part, because you only need to type in a few lines. MHN will guide you through the rest of the setup.
First, make sure you have git installed:
root@IPAddress:/$ sudo apt-get install git -y
Then type in just a few lines (I’ve bolded the commands):
root@IPAddress:/$ cd /opt/
root@IPAddress:/opt$ sudo git clone https://github.com/GokhanSari06/mhn/
Cloning into 'mhn'...

root@IPAddress:/opt$ cd mhn/ 
root@IPAddress:/opt/mhn$ sudo ./install.sh

Now this is where the magic happens. Wait a few minutes to get to the configuration stage.
+ python generateconfig.py
Do you wish to run in Debug mode?: y/n n
Superuser email: your email address
Superuser password: make a really good password
Superuser password: (again): repeat your really good password

Server base url ["http://IPAddress"]: https://domain/ (or use IP)
Honeymap url ["https://domain:3000"]: https://domain:3000 (or use IP)

I suggest using HTTPS with a domain name. (you can set up that in a little while.)
Next you’ll be setting up your mailserver settings. Just use the default if you’d like.
Now you’ll be waiting for awhile because MHN will be loading in a bunch of rules. So this is an ideal time to paint your nails; by the time it’s done your nails will be dry. This is also a good time to set up your SSL cert, if you choose.*
It’s going to ask if you want to integrate with Splunk. I won’t be choosing this option, and might be using Graylog instead. You also have the option of installing ELK which I’m also passing up.
Now we are all done with the MHN install. Go on over to your domain name or the IP address and you can log in if you want!
Next, we will use UFW (a firewall) to restrict access to only users, honeypots, and you (the admin.)

— — — — — TL;DR end here — — — — —
UFW can be installed with apt-get:
root@IPAddress:/$ sudo apt-get install ufw
It’s fairly easy to use and the commands we need are well documented here.
MHN needs ports 3000 and 443 so we have to whitelist those. You will also need to add your ssh port(s). So for my server:

root@IPAddress:/$ sudo ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
root@IPAddress:/$ sudo ufw allow 443
[sudo] password for adalovelace: 
Rule added
Rule added (v6)
root@IPAddress:/$ sudo ufw allow 3000
Rule added
Rule added (v6)
root@IPAddress:/$ sudo ufw allow 2282
Rule added
Rule added (v6)
root@IPAddress:/$ sudo ufw allow from [my IP]
Rule added

Congratulations :)
Then you've created your Honeymap and admin page. Next step is creating honeypots. Now you can create honeypots by admin ui page on the options.

Take care, have a nice coding :)