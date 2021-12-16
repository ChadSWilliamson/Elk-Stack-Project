# Elk-Stack-Project-1
Elk Stack Deployment

![image](https://user-images.githubusercontent.com/89936268/146249090-8ffe8f07-9ef5-42a7-9ff1-bb09996d2eec.png)




These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YML file may be used to install only certain pieces of it, such as Filebeat.

- Ansible Playbook
- Ansible Hosts
- Ansible Configuration
- Ansible ELK Installation and VM Configuration
- Ansible Filebeat Playbook
- Ansible Filebeat Config file
- Ansible Metricbeat Playbook
- Ansible Metricbeat Config file

Download the ansible.cfg configuration file on this website https://ansible.com/ and edit or copy Ansible Configuration to your /etc/ansible directory

- For ansible.cfg edit:
cd /etc/ansible/	
nano ansible.cfg
CTRL + W > enter remote_user
change `remote_user = RedAdmin

Assign username and SSH Public Key for Web1, Web2, ELK Virtual Machine in Azure GUI

- Web1 / Web2 / ELK Server > Reset Password > Reset SSH Public Key
  username: RedAdmin
  SSH Key : copy id_rsa.pub from the ansible control node in .ssh/ directory. 
- To get the SSH Key run this command:
  i. ~/.ssh# ssh-keygen
 ii. ~/.ssh# cat id_rsa.pub
 
This document contains the following details:

- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly efficient, in addition to restricting traffic to the network.

What aspect of security do load balancers protect?

Load balancers protects the system from DDoS attacks by shifting attack traffic.
Load Balancing contributes to the Availability aspect of security in regards to the CIA Triad.
What is the advantage of a jump box?

The advantage of a jump box is to give access to the user from a single node that can be secured and monitored.
The advantage of a JumpBox is the orgination point for launching Administrative Tasks. This ultimately sets the JumpBox as a SAW (Secure Admin Workstation). All Administrators when conducting any Administrative Task will be required to connect to the JumpBox (SAW) before perfoming any task/assignment.
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the changes to the Logs and system traffic.

What does Filebeat watch for?
Filebeat watches for any information in the file system which has been changed and when it has.
Filebeat watches for log files/locations and collects log events
What does Metricbeat record?
Metricbeat takes the metrics and statistics that collects and ships them to the output you specify.
Metricbeat records metric and statistical data from the operating system and from services running on the server.


The configuration details of each machine may be found below.

| Name      | Function    | IP Address | Operating System |
|-----------|-------------|------------|------------------|
| Jump Box  | Gateway     | 10.0.0.4   | Linux (Ubuntu)   |
| Web-1     | Webserver 1 | 10.0.0.5   | Linux (Ubuntu)   |
| Web-2     | Webserver 2 | 10.0.0.5   | Linux (Ubuntu)   |
| ELK-Server| ELK-Server  | 10.1.0.4   | Linux (Ubuntu)   |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Personal IP address

Machines within the network can only be accessed by the jump box VM.
- The Elk Machine has access from personal IP address through port 5601.

A summary of the access policies in place can be found in the table below.

| Name         | Publicly Accessible | Allowed IP Addresses |
|--------------|---------------------|----------------------|
| Jump Box     | No                  | Personal             |
| Load Balancer| Yes                 | Open                 |
| Web-1        | No                  | 10.0.0.5             |
| Web-2        | No                  | 10.0.0.6             |
| ELK-Server   | No                  | 10.1.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Services running can be limited, system installation and updates can be streamlined, and processes become more replicable.

The playbook implements the following tasks:

- Install docker.io
- Install Python-pip
- Install docker container
- Launch docker container: elk
- Command: sysctl -w vm.max_map_count=262144 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![image](https://user-images.githubusercontent.com/89936268/146257058-baf20791-6f66-4684-9df2-395ee6ab3693.png)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1 (10.0.0.5)
- Web 2 (10.0.0.6)

We have installed the following Beats on these machines:
- FileBeat
- Metric Beat

These Beats allow us to collect the following information from each machine:
- Filebeat is a log data shipper for local files. Installed as an agent on your servers, Filebeat monitors the log directories or specific log files, tails the files, and forwards them either to Elasticsearch or Logstash for indexing. An example of such are the logs produced from the MySQL database supporting our application.

- Metricbeat collects metrics and statistics on the system. An example of such is cpu usage, which can be used to monitor the system health.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to /etc/ansible.
- Update the the configuration file to include the webservers and ElkVM (private Ip address.
- Run the playbook, and navigate to ElkVM to check that the installation worked as expected. /etc/ansible/host should include:
- Run the playbook, and SSH into the Elk vm, then run docker ps to check that the installation worked as expected. Playbook: install_elk.yml Location: /etc/ansible/install_elk.yml Navigate to http://[your.ELK-VM.External.IP]:5601/app/kibana to confirm ELK and kibana are running. 
- You should see the following:
![image](https://user-images.githubusercontent.com/89936268/146253587-a70b5070-a836-4ebb-a32a-5d0e964e5575.png)

_Answer the following questions to fill in the blanks:_
- Which file is the playbook? The Filebeat-configuration
- Where do you copy it? copy/etc/ansible/files/filebeat-config.yml to /etc/filebeat/filebeat.yml
- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?      update filebeat-config.yml -- specify which machine to install by updating the host files with ip addresses of web/elk servers and selecting which group to run on in ansible.  
- Which URL do you navigate to in order to check that the ELK server is running? http://[your.ELK-VM.External.IP]:5601/app/kibana.

Using the Playbook-filebeat-playbook.yml

- Copy the filebeat.yml file to the /etc/ansible/files/ directory.
- Update the configuration file to include the Private IP of the Elk-Server to the ElasticSearch and Kibana sections of the configuration file.
- Create a new playbook in the /etc/ansible/roles/ directory that will install, drop in the updated configuration file, enable and configure system module, run the filebeat setup, and start the filebeat service.
- Create a new playbook in the /etc/ansible/roles/ directory that will install, drop in the updated configuration file, enable and configure system module, run the metricbeat setup, and start the metricbeat service.
- Run the playbooks, and navigate back to the installation page on the ELk-Server GUI, click the check data on the Module Status
- Click the verfiy incoming Data to check and see the receiving logs from the DVWA machines.


_Specific commands the user will need to run to download the playbook, update the files, etc._

- ssh RedAdmin@JumpBox(Public IP)
- sudo docker container list -a (locate your ansible container)
- sudo docker start container (name of the container)
- sudo docker attach container (name of the container)
cd /etc/ansible/ - ansible-playbook elk.yml (configures Elk-Server and starts the Elk container on the Elk-Server) wait a couple minutes for the implementation of the Elk-Server - cd /etc/ansible/roles/ - ansible-playbook filebeat-playbook.yml (installs Filebeat and Metricbeat) - open a new web browser (http://[your.ELK-VM.External.IP]:5601/app/kibana) This will bring up the Kibana Web Portal - check the Module status for file beat and metric beat to see their data receiving.

** You will need to ensure all files are properly placed before running the ansible-playbooks.






