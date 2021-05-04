## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

 (Images/diagram_1.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the configuration file may be used to install only certain pieces of it, such as Filebeat.

 (Ansible/install-elk.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting unwanted access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data moving from across the Elastic Stack as well as system integrity and validity.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |   |
|----------|----------|------------|------------------|---|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |   |
| Web-1    | VM       | 10.0.0.4   | Linux            |   |
| Web-2    | VM       | 10.0.0.5   | Linux            |   |
| Web-3    | VM       | 10.0.0.6   | Linux            |   |
| ELKvm    | VM       | 10.1.0.4   | Linux            |   |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box virtual machine can accept connections from the Internet. Access to this machine is only allowed from whitelisted IP addresses.

Machines within the network can only be accessed by the Jump Box.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              |(Whitelisted Ip Address)|
| Web-1    | No                  | 10.0.0.1             |
| Web-2    | No                  | 10.0.0.1             | 
| Web-3    | No                  | 10.0.0.1             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows you to be able to update an entire fleet of machines without having to do them each manually.

The playbook implements the following tasks:
- Install docker.io
- Install python3-pip
- Install Docker Module
- Increase VM Virtual Memory
- Downloads and launches a docker elk container
- Enables docker service on boot


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

 (Images/screenshot1.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
 Web-1 10.0.0.4
 Web-2 10.0.0.5
 Web-3 10.0.0.6

We have installed the following Beats on these machines:
 Filebeat and Metricbeat

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the metricbeat-config.yml file to /etc/ansible/files/metricbeat-config.yml.
- Update the metricbeat-config.yml file to include the private IP of the ELK stack VM. 
- Run the playbook, and navigate to /etc/metricbeat to check that the installation worked as expected.

### Helpful commands to remember for dealing with containers

-connecting to jumpbox
ssh azadmin@ (jumpbox ip)

-show containers
sudo docker container list -a

-start container
sudo docker start (container name)

-connect to container
sudo docker attach (container name)

-ansible
cd /etc/ansible
