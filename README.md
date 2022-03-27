# Cybersecurity-BootCampProj1
This is a repo of what I've done in my class so far 

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Project 1 Diagram

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Elk playbook file may be used to install only certain pieces of it, such as Filebeat.

---

- name: Configure Elk VM with Docker

hosts: elk

remote\_user: azadmin

become: True

tasks:

- name: Increase vitual memory

command: sysctl -w vm.max\_map\_count=262144

- name: use more memory

sysctl:

name: vm.max\_map\_count

value: &quot;262144&quot;

state: present

reload: yes

- name: Increase virtual memory on restart

shell: echo &quot;vm.max\_map\_count=262144&quot; \&gt;\&gt; /etc/sysctl.conf

- name: Install docker.io

apt:

update\_cache: yes

name: docker.io

state: present

- name: pip3

apt:

force\_apt\_get: yes

name: python3-pip

state: present

- name: Install Docker python module

pip:

name: docker

state: present

- name: download and launch docker web container

docker\_container:

name: elk

image: sebp/elk:761

state: started

restart\_policy: always

published\_ports:

- 5601:5601

- 9200:9200

- 5044:5044

This document contains the following details: - Description of the Topologu - Access Policies - ELK Configuration - Beats in Use - Machines Being Monitored - How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D\*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network. Load balancers are able to mitigate DoS attacks on a server, creating a buffer of security in access to the server. It creates a pool of servers in which it can distribute the traffic in order to not be overwhelmed. A Jump Box like what we used in this project is to provide an additional layer between the loadbalancer pool and the internet. _It acts as an access point to a secure zone._

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the operating system and services running and system logs. Metricbeat will provide metrics on how your computer is operating while Filebeat provides system with a logging agent and will generate and tail them.

The configuration details of each machine may be found below.

| Name | Function | IP Address | Operating System |
| --- | --- | --- | --- |
| Jump Box | Gateway | 104.210.63.170 | Linux |
| --- | --- | --- | --- |
| Web 1 | Virtual Machine | 10.0.0.5 | Linux |
| Web 2 | Virtual Machine/GW | 10.0.0.6 | Linux |
| Elk 1 Server | Web Server | 10.1.0.4 | Linux |

### Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

136.36.254.183
24.10.191.75

Machines within the network can only be accessed by an SSH TCP protocol- _To Access the Headless Elk server you would need to SSH from the WEB 2 machine at 10.0.0.6_

A summary of the access policies in place can be found in the table below.

| Name | Publicly Accessible | Allowed IP Addresses |
| --- | --- | --- |
| Jump Box | Yes | 136.36.254.183 or 24.10.191.75 |
| --- | --- | --- |
| Web 1Web 2ELK | NoNoYes kibana | 10.0.0.410.0.0.4 or 10.0.0.510.0.0.6 or Two prior PUB Ips |
|
 |
 |
 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because on restarts it can update the system just by running the playbooks. So you don&#39;t have to manually reconfigure every time you wish to start the Containers.

The playbook implements the following tasks:

- Configure the VM to use more memory
- Install Docker container and launch on start, using the elk:761 image. Then start with a port map
- Install Python on docker with python pip software required for control of Docker containers

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.

_README/Images/ELK-1 SERVER Running_

### Target Machines &amp; Beats

This ELK server is configured to monitor the following machines: 10.0.0.5, 10.0.0.6

We have installed the following Beats on these machines: - _Metricbeat and FIlebeat_

These Beats allow us to collect the following information from each machine: - _MetricBeat will collect Metrics on the DVWA site such as container info, usage, packets,etc.. Filebeat will be collecting system logs of the site and its usage._

### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below: - Copy the file to Filebeat-playbook.yml/metricbeat-playbook.yml. - Update the Configuration file to include the IP addresses you want to connect to/monitor.- Run the playbook using ansible-playbook {playbook name} and navigate to http://20.25.236.146:5601/app/kibana to check that the installation worked as expected.
