## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

<img width="960" alt="network-diagram" src="https://github.com/EmmanuellaD/Elk-Stack-Project/blob/master/Diagrams/network_diagram.png">

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

   /etc/ansible/elk-playbook.yml

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
- What aspect of security do load balancers protect?
   	- Load balancers protects the system from DDoS attacks by shifting attack traffic.
  	- Load Balancing contributes to the Availability aspect of security in regards to the CIA Triad.
 
- What is the advantage of a jump box?
   	- The advantage of a jump box is to give access to the user from a single node that can be secured and monitored.
   	- The advantage of a JumpBox is the orgination point for launching Administrative Tasks. This ultimately sets the JumpBox as a Secure station. All Administrators when conducting any Administrative Task will be required to connect to the JumpBox before perfoming any task/assignment.


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- What does Filebeat watch for?
   	- Filebeat watches for any information in the file system which has been changed and when it has.  
   	- Filebeat watches for log files/locations and collects log events
- What does Metricbeat record?
  	- Metricbeat takes the metrics and statistics that collects and ships them to the output you specify.
   	- Metricbeat records metric and statistical data from the operating system and from services running on the server.


The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| Web-1    | Server   | 10.0.0.6   | Linux            |
| Web-2    | server   | 10.0.0.7   | Linux            |
|Elkmachine|elk-server| 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_

Machines within the network can only be accessed by _____.
- Which machine did you allow to access your ELK VM? What was its IP address?
JumpBox VM, its private Ip address(Vnet IP)-10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 |  142.117.203.83      |
| Web-1    | No                  |  10.0.0.4            |
| Web-2    | No                  |  10.0.0.4            |
|ElkMachine| No                  |  10.0.0.4
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- it allows you to deploy to multiple servers using a single playbook

The playbook implements the following tasks:
The playbook implements the following tasks:
- Install docker.io
- Install Python-pip
- Install docker container
- Launch docker container: elk
- Command: sysctl -w vm.max_map_count=262144

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- - Web-1(10.0.0.6) Web-2(10.0.0.7)

We have installed the following Beats on these machines:
        -  Filebeat
    	-  Metricbeat

These Beats allow us to collect the following information from each machine:
    - Filebeat monitors log files or locations you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
   	- Metricbeat collects metrics from the operating system and from services running on the server.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to /etc/ansible.
- Update the config file to include the webservers and the elk machine
- Run the playbook, and navigate to elk vm to check that the installation worked as expected.
/etc/ansible/host should include:

[webservers]
[10.0.0.6] ansible_python_interpreter=/usr/bin/python3
[10.0.07] ansible_python_interpreter=/usr/bin/python3

[elk]
[10.1.0.4] ansible_python_interpreter=/usr/bin/python3

- Which file is the playbook? Where do you copy it?
    	 /etc/ansible/file/filebeat-config.yml 
- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
        edit the /etc/ansible/hosts file to add webserver/elkserver ip addresses
- Which URL do you navigate to in order to check that the ELK server is running?
    	 http://[your.ELK-VM.External.IP]:5601/app/kibana


_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
- ssh azadmin@JumpBox(Public IP)
	- sudo docker container list -a (locate your ansible container)
	- sudo docker start container (name of the container)
	- sudo docker attach container (name of the container)
cd /etc/ansible/
	- ansible-playbook elk.yml (configures Elk-Server and starts the Elk container on the Elk-Server) wait a couple minutes for the implementation of the Elk-Server
	- ansible-playbook filebeat-playbook.yml (installs Filebeat)
    - ansible-playbook metricbeat-playbook.yml (installs Metricbeat)
	- open a new web browser (http://[your.ELK-VM.External.IP]:5601/app/kibana) This will bring up the Kibana Web Portal
	- check the Module status for file beat and metric beat to see their data receiving.
