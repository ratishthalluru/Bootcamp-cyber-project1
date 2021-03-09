## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(Diagrams/Network_Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Ansible files may be used to install only certain pieces of it, such as Filebeat.

 Ansible/filebeat-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
 - Load balancers protect application availability, allowing client requests to be shared across a number of servers
 - The advantage of a Jump Box is that it minimises the attack surface by ensuring remote connections to the cloud network come through a single VM. Additionally, remote         connections to the Jump Box can be monitored easily to identify unusual remote connections.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the configuration and system files.
 - Filebeat is used to monitor log files 
 - Metricbeat is used to collect operating system ans service statistics from mointored VMs

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.1.0.4   | Linux            |
| Web1.1   |  DVWA    | 10.1.0.5   | Linux            |
| Web2.1   |  DVWA    | 10.1.0.6   | Linux            |
| Web3.1   |  DVWA    | 10.1.0.7   | Linux            |
| ELK      |Log Server| 10.0.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 149.167.128.22

Machines within the network can only be accessed by the Jump Box.
- The Jump Box can access the ELK VM using SSH. The Jump Box's IP address is 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes (SSH)           |   149.167.128.22     |
| Web1.1   | Yes (HTTP)          |   149.167.128.22     |
| Web2.1   | Yes (HTTP)          |   149.167.128.22     |
| Web3.1   | Yes (HTTP)          |   149.167.128.22     |
| ELK      | Yes (HTTP)          |   149.167.128.22     |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Build and deployment is performed automatically, consistently and quickly
- Consistent, rapid configuration and depoloyment of virtual machines ensure all prescribed security meaures can be scripted to minimise attack surfaces while enabling   
  horizontal and elastic scaling by deployment to more or fewer virtual machines in a cluster as required to meet capacity demand.
- Facilitates OS and software updates

The playbook implements the following tasks:
The three playbooks above implement the following tasks:

**Playbook 1: pentest.yml**
pentest.yml is used to set up DMWA servers running in a Docker container on each of the web servies show in the diagram above. It implements the following tasks:
Installs Docker
Installs Python
Installs Docker's Python Module
Downloads and launches the DVWA Docker container
Enables the Docker service

**Playbook 2: install-elk.yml**
install-elk.yml is used to set up and launch the ELK repository server in a Docker Container on the ELK server. It implements the following tasks:
Installs Docker
Installs Python
Installs Docker's Python Module
Increase virtual memory to support the ELK stack
Increase memory to support the ELK stack
Download and launch the Docker ELK container

**Playbook 3: filebeat-playbook.yml**
filebeat-playbook.yml is used to deploy Filebeat on each of the web servers so they can be monitored centrally using ELK services running on Elk. It implements the following tasks:
Downloads and installs Filebeat
Enables and congigures the system module
Configures and launches Filebeat


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
 - Web1.1: 10.1.0.5
 - Web2.1: 10.1.0.6
 - Web3.1: 10.1.0.7

We have installed the following Beats on these machines:
 - Filebeat

These Beats allow us to collect the following information from each machine:
 - Filebeat collects and ships (sends to ELK for collation, persistence and reporting) logs from VMs running the Filebeat agent

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook files to ansible docker container.
- Update the ansible hosts file to include the following
- Run the playbook, and navigate to /etc/ansible/hosts to check that the installation worked as expected.
[webservers]
10.1.0.5 ansible_python_interpreter=/usr/bin/python3
10.1.0.6 ansible_python_interpreter=/usr/bin/python3
10.1.0.7 ansible_python_interpreter=/usr/bin/python3

[ELK]
10.0.0.4 ansible_python_interpreter=/usr/bin/python3

Update the Ansible configuration file /etc/ansible/ansible.cfg and set the remote_user parameter to the admin user of the web servers.

**Running the Playbooks**
 - Start an ssh session with the Jump Box ~$ ssh <sysadmin>@<Jump Box Public IP>
 - Start the Ansible Docker container ~$ sudo docker start <Ansible Container>
 - Attach a shell to the Ansible Docker container with the command ~$ sudo docker attach <Ansible Container Name>
 - Run the playbooks with the following commands:
   - ansible-playbook /etc/ansible/pentest.yml
   - ansible-playbook /etc/ansible/install-elk.yml
   - ansible-playbook /etc/ansible/roles/filebeat-playbook.yml

 - After running the playbooks and observing no errors in the output, navigate to Kibana to check that the installation worked as expected by viewing Filebeat data and    
   reports in the Kibana Dashboard
 - Kibana can be accessed at http://<elk-server-ip>:5601/app/kibana
