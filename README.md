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
  - i. ~/.ssh# ssh-keygen
  - ii. ~/.ssh# cat id_rsa.pub
 
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
Load balancers protects the system from DDoS attacks by shifting attack traffic.
Load Balancing contributes to the Availability aspect of security in regards to the CIA Triad.
What is the advantage of a jump box?

The advantage of a jump box is to give access to the user from a single node that can be secured and monitored.
The advantage of a JumpBox is the orgination point for launching Administrative Tasks. This ultimately sets the JumpBox as a SAW (Secure Admin Workstation). All Administrators when conducting any Administrative Task will be required to connect to the JumpBox (SAW) before perfoming any task/assignment.
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the changes to the Logs and system traffic.

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
For ELK VM Configuration:

- Copy the Ansible ELK Installation and VM Configuration
- Run the playbook using this command : ansible-playbook install-elk.yml

For FILEBEAT:
- Download Filebeat playbook usng this command:
  - curl -L -O https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml
- Copy the '/etc/ansible/files/filebeat-config.yml' file to '/etc/filebeat/filebeat-playbook.yml'
- Update the filebeat-playbook.yml file to include installer
  - curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb
- Update the filebeat-config.yml file /etc/ansible/files# nano filebeat-config.yml

output.elasticsearch:
  #Array of hosts to connect to.
 hosts: ["10.1.0.4:9200"]
  username: "elastic"
  password: "changeme” 

 setup.kibana:
  host: "10.1.0.4:5601"
  
- Run the playbook using this command ansible-playbook filebeat-playbook.yml and navigate to Kibana > Logs : Add log data > System logs > 5:Module Status > Check data to check that the installation worked as expected.

For METRICBEAT:
- Download Metricbeat playbook using this command:
  - curl -L -O https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > /etc/ansible/files/metricbeat-config.yml
- Copy the /etc/ansible/files/metricbeat file to /etc/metricbeat/metricbeat-playbook.yml
- Update the filebeat-playbook.yml file to include installer
  - curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb
- Update the metricbeat file rename to metricbeat-config.yml
  
  output.elasticsearch:
  #Array of hosts to connect to.
  hosts: ["10.1.0.4:9200"]
    username: "elastic"
    password: "changeme"

setup.kibana:
  host: "10.1.0.4:5601"
  
- Run the playbook, (ansible-playbook metricbeat-playbook.yml) and navigate to Kibana > Add Metric Data > Docker Metrics > Module Status to check that the installation worked as expected. 


- Playbook file:  Filebeat-configuration
- Where do you copy it: copy/etc/ansible/files/filebeat-config.yml to /etc/filebeat/filebeat.yml
- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on:  update filebeat-config.yml -- specify which machine to install by updating the host files with ip addresses of web/elk servers and selecting which group to run on in ansible.  
- Which URL do you navigate to in order to check that the ELK server is running? http://[your.ELK-VM.External.IP]:5601/app/kibana.

Using the Playbook-filebeat-playbook.yml

- Copy the filebeat.yml file to the /etc/ansible/files/ directory.
- Update the configuration file to include the Private IP of the Elk-Server to the ElasticSearch and Kibana sections of the configuration file.
- Create a new playbook in the /etc/ansible/roles/ directory that will install, drop in the updated configuration file, enable and configure system module, run the filebeat setup, and start the filebeat service.
- Create a new playbook in the /etc/ansible/roles/ directory that will install, drop in the updated configuration file, enable and configure system module, run the metricbeat setup, and start the metricbeat service.
- Run the playbooks, and navigate back to the installation page on the ELk-Server GUI, click the check data on the Module Status
- Click the verfiy incoming Data to check and see the receiving logs from the DVWA machines.

How to get Filebeat installer :

1. Login to Kibana > Logs : Add log data > System logs > DEB > Getting started
2.Copy: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

How to get the Metricbeat installer:

1. Login to Kibana > Add Metric Data > Docker Metrics > DEB > Getting Started
2. Copy: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb

-  We will create the my-playbook1.yml as our playbook.

-  For FILEBEAT: We will create filbeat-playbook.yml as our playbook.

-  For METRICBEAT: We will create metricbeat-playbook.yml as our playbook.


How to Download and Edit the Ansible Configuration file

- /etc/ansible# curl -L -O https://ansible.com/  > ansible.cfg
- /etc/ansible# nano ansible.cfg

- Press CTRL + W (to search > enter remote_user then change `remote_user = sysadmin`
Where : `RedAdmin` is the remote user that has control over ansible.

How to Edit the Ansible Hosts file in this directory /etc/ansible/hosts
#List the IP Addresses of your webservers
#You should have at least 2 IP addresses

[webservers]
10.0.0.4 ansible_python_interpreter=/usr/bin/python3
10.0.0.5 ansible_python_interpreter=/usr/bin/python3
10.0.0.6 ansible_python_interpreter=/usr/bin/python3

#List the IP address of your ELK server
#There should only be one IP address
[elk]
10.1.0.4 ansible_python_interpreter=/usr/bin/python3
Where: [webservers] and [elk] are the group of machines and each group has 1 or more members.

How to Create the ELK Installation and VM Configuration in the /etc/ansible/ directory:
See the final solution of the Ansible ELK Installation and VM Configuration

Specify a different group of machines as well as a different remote user
      - name: Config elk VM with Docker
        hosts: elk
        remote_user: RedAdmin
        become: true
        tasks:
        
Where: [elk] is the Virtual Machine hosts or the group of machine targetted for this installation and can only be done by a `sysadmin` remote_user

How to Copy the raw Filebeat Module Configuration file from web to the /etc/ansible/files directory:
- curl -L -O https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml'
  - Note : The filebeat-config.yml as our filebeat configuration file.
  
* See the final solution of the Filebeat Config file

hosts: ["10.1.0.4:9200"]
  username: "elastic"
  password: "changeme" 

setup.kibana:
  host: "10.1.0.4:5601"
Where: hosts: ["10.1.0.4:9200"] is the ELK VM that can install Filebeat

How to Copy the raw Metricbeat Module Configuration from web to the /etc/ansible/files/ directory:
- curl -L -O https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > /etc/ansible/files/metricbeat-config.yml'
  - Note : the metricbeat-config.yml as our metricbeat configuration file. 
  
* See the final solution of the Metricbeat Config file

hosts: ["10.1.0.4:9200"]
  username: "elastic"
  password: "changeme" 

setup.kibana:
  host: "10.1.0.4:5601"
  
Where: hosts: ["10.1.0.4:9200"] is the ELK VM that can install Metricbeat

- Which URL do you navigate to in order to check that the ELK server is running?
  - Test Kibana on web : http://[your.ELK-VM.External.IP]:5601/app/kibana
  - Test Kibana on localhost: sysadmin@10.1.0.4: curl localhost:5601/app/kibana


_Specific commands the user will need to run to download the playbook, update the files, etc._

- ssh RedAdmin@JumpBox(Public IP)
- sudo docker container list -a (locate your ansible container)
- sudo docker start container (name of the container)
- sudo docker attach container (name of the container)
cd /etc/ansible/ - ansible-playbook elk.yml (configures Elk-Server and starts the Elk container on the Elk-Server) wait a couple minutes for the implementation of the Elk-Server - cd /etc/ansible/roles/ - ansible-playbook filebeat-playbook.yml (installs Filebeat and Metricbeat) - open a new web browser (http://[your.ELK-VM.External.IP]:5601/app/kibana) This will bring up the Kibana Web Portal - check the Module status for file beat and metric beat to see their data receiving.

Linux Command List :
|                  COMMAND                      |                     PURPOSE                   |
|-----------------------------------------------|-----------------------------------------------|
|sudo apt-get update	                          | this will update all packages                 |
|sudo apt install docker.io                     | install docker application                    |
|sudo service docker start                      |	start the docker application                  |
|systemctl status docker                        |	status of the docker application              |
|sudo docker pull cyberxsecurity/ansible        |	download the docker file                      |
|sudo docker run -ti cyberxsecurity/ansible bash|	run and create a docker image                 |
|sudo docker start <image-name>                 |	starts the image specified                    |
|sudo docker ps -a                              |	list all active/inactive containers           |
|sudo docker attach <image-name>                |	effectively sshing into the ansible           |
|ssh-keygen                                     | create a ssh key                              |
|ansible -m ping all                            |	check the connection of ansible containers    |
|                                               |                                               |  
  
** You will need to ensure all files are properly placed before running the ansible-playbooks.






