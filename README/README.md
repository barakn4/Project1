	## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Images/Fullproject.jpg

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the beat install files may be used to install only certain pieces of it, such as Filebeat.

  - DVWA_playbook.yml
  - ELK_playbook.yml
  - filebeat_playbook.yml
  - metricbeat_playbook.yml

config files:
  
  - metricbeat-config.yml
  - filebeat-config.yml


This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant, in addition to restricting direct access to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box? they can protect you from a DoS attack, and not giving direct access to the webserver. the advantage of a jumpbox is the face that you can secure one node in the system higly and control the rest of the servers from there, and is the only one that can have SSH connection.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the Log files and system metric.
-  What does Filebeat watch for? Log files
-  What does Metricbeat record? metric of the system and services running.

The configuration details of each machine may be found below.

| Name       	| Function         	| IP address                                   	| Operating System     	|
|------------	|------------------	|----------------------------------------------	|----------------------	|
| Jump Box   	| Gateway          	| 13.82.136.116 (public) 10.0.0.5 (private)    	| Linux (Ubuntu 18.04) 	|
| ELK server 	| ELK stack server 	| 52.173.25.251 (public) 10.1.0.4 (private)    	| Linux (Ubuntu 18.04) 	|
| Web-1      	| DVWA server      	| 40.88.128.80 (LB public) 10.0.0.8 (private)  	| Linux (Ubuntu 18.04) 	|
| Web-2      	| DVWA server      	| 40.88.128.80 (LB public) 10.0.0.9 (private)  	| Linux (Ubuntu 18.04) 	|
| Web-3      	| DVWA server      	| 40.88.128.80 (LB public) 10.0.0.12 (private) 	| Linux (Ubuntu 18.04) 	|

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox,ELK and Load Balancer machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Whitelist:
	Jumpbox :
		Home_IP : SSH
	ELK:
		Home_IP: 5601
		Jumpbox: 22
	Load Balancer:
		Home_IP : 80
	

Machines within the network can only be accessed by jumpbox .
 Which machine did you allow to access your ELK VM? What was its IP address?
	I allowed the Jumpbox to access the ELK from within the Ansible container with SSH. ports 9200 and 5601 are open for the WEB-VMs to send beats to collect. port 5601 is open to Home_IP to access the dashboard. 

A summary of the access policies in place can be found in the table below.

| Name       	| Publicly Accessible  	| Allowed IP Addresses                                                             	|
|------------	|----------------------	|----------------------------------------------------------------------------------	|
| Jump Box   	| Yes                  	| 207.38.232.55 (Home IP)                                                          	|
| ELK server 	| Yes                  	| 207.38.232.55 (Home IP), VNet IPs (10.0.0.0/16)                                  	|
| Web-1      	| Yes (Through LB)     	| 40.88.128.80 (Load Balancer), 207.38.232.55 (Through LB), VNet IPs (10.0.0.0/16) 	|
| Web-2      	| Yes (through LB)     	| 40.88.128.80 (Load Balancer), 207.38.232.55 (Through LB), VNet IPs (10.0.0.0/16) 	|
| Web-3      	| Yes (Through LB)     	| 40.88.128.80 (Load Balancer), 207.38.232.55 (Through LB), VNet IPs (10.0.0.0/16) 	|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
 What is the main advantage of automating configuration with Ansible?_
	The main advantage of using Ansible to automate and configure is using the playbooks in order to configure and maintain a group of server with one file instead of going one by one with commands and reducing the probablity for problems and having all the servers configured exactly the same way.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._

- set vm.max_map_count to 262144 - in order for the elk container to run it needs the extra memory usage. ** if still not working after, make sure that vm.overcommit_memory = 0
- Install Docker , Pyhton3, pip-docker
- Download and install the elk stack container
- enable docker service for further use

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Images/DockerPS Output.jpg

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

Web-Vm1 - 10.0.0.8
Web-Vm2 - 10.0.0.9
Web-Vm3 - 10.0.0.12
	

We have installed the following Beats on these machines:
Filebeat
Metricbeat

These Beats allow us to collect the following information from each machine:

Metricbeat - Collects metric data from the servers, such as cpu usage, information about docker containers running and so on. 
Filebeat - collects data from the log files of the system, such as syslog events, ssh connection logs, sudo commands. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook and config files to /etc/ansible/roles (playboook) and /etc/ansible/files (config)
- Update the hosts file to include the ips of the webservers under that group. 
- Run the playbook, and navigate to http://<ELK.VM.External.IP>:5601/app/kibana to check that the installation worked as expected.

 Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_ the .yml files. you copy it into the /etc/ansible/ folder, preferably to the /roles/ folder.
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? updating the hosts file to include the ips under the webservers group. 
- _Which URL do you navigate to in order to check that the ELK server is running? http://<ELK.VM.External.IP>:5601/app/kibana

_