# Quick Start Guide

- Install mariadb, or pgsql if you are using them.

- Create a db in mariadb or pgsql for mprov if you are using them

- Create a user and grant privs to your db

- Create an env.db file in the directory you are going to run install_mpcc.sh from

- Grab the install_mpcc.sh script and run it with the correct options

- Go to the server where you plan to run your first jobserver from (can be the same machine)

- Grab the install_mprov_jobserver.sh script

- Run the script, follow any suggested firewall changes if needed.

- Go to the mpcc and create an API key for the job server

- Configure the jobserver to run with the URL, IP Address, API Key and job modules you wish to use. (suggested: image-update, image-delete, image-server, dnsmasq)

- Run `systemctl enable mprov_jobserver --now`

- Go to the MPCC

- Add a network type of Ethernet
- Add a Network of network type ethernet for your mgmt lan
- Add a switch to the network, named the switch you are using, so that LLDP will work.  Add the ports your nodes will be attached to.
- Add an OS Repository pointing to the 'rocky-repos' rpm, ie https://distro.ibiblio.org/rocky/8/BaseOS/x86_64/os/Packages/r/rocky-repos-8.6-3.el8.noarch.rpm
- Add an OS Distro referencing the Repository you just created as the base.
- Add the following to the install kernel cmdline: "mprov_tmpfs_size=8G mprov_image_url=http://${next-server}/images/{{ nic.system.systemimage.slug }} mprov_initial_mods=e1000e,tg3 mprov_prov_intf=eth0"
	- This will allow a stateless node with 8G of ram tmpfs root
- Add the following scripts 
	- https://raw.githubusercontent.com/mprov-ng/mprov_scripts/master/image-gen/install_postboot_jobserver.sh as an image-gen script
	- https://raw.githubusercontent.com/mprov-ng/mprov_scripts/master/post-boot/nads.sh as a post-boot script

- Add a System Group for your compute nodes
- Add a System Image named `__nads__` for the node auto detection system and assign both the scripts you added above.
- Add a system image for your compute nodes, only add the "install_postboot_jobserver.sh" file
- Add your nodes, making sure to select the compute image, and in the network, set the hostname, IP and switch port correctly.  If you have LLDP enabled, the nodes can netboot into NADS and will be autodected.  Otherwise, you will need to enter the mac address for each host's management nic
