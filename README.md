**Work in progress**

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(Images/diagram_1.png)

These files have been tested and used to generate a live ELK deployment on Azure. While somewhat incomplete, it shows a basic understanding as to how the deployment works as pictured above.

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant, in addition to restricting traffic to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data moving from across the Elastic Stack and system integrity and validity.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |   |
|----------|----------|------------|------------------|---|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |   |
| Web-1    | VM       | 10.0.0.4   | Linux            |   |
| Web-2    | VM       | 10.0.0.5   | Linux            |   |
| Web-3    | VM       | 10.0.0.6   | Linux            |   |
| ELKvm    | VM       | 10.1.0.4   | Linux            |   |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from whitelisted ip addresses.

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows you to be able to update an entire fleet of machines without having to do them each manually.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(Images/screenshot1.png)

### Target Machines & Beats
This ELK server is configured to monitor virtual machines.

"
---
- name: download filebeat
  hosts: webservers
  become: yes
  tasks:

  - name: download filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

  - name: install filebeat deb
    command: dpkg -i filebeat-7.6.1-amd64.deb

  - name: drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

  - name: enable and configure system module
    command: filebeat modules enable system

  - name: setup filebeat
    command: filebeat setup

  - name: start filebeat service
    command: service filebeat start

  - name: enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes
"

### Using the Playbook
The playbook used to configure the ELKvm with docker is the following;

"
---
- name: Configure ELK VM with Docker
  hosts: elk
  remote_user: azadmin
  become: true
  tasks:

  - name: Install docker.io
    apt:
      update_cache: yes
      force_apt_get: yes
      name: docker.io
      state: present

  - name: Install python3-pip
    apt:
      force_apt_get: yes
      name: python3-pip
      state: present

  - name: Install Docker module
    pip:
      name: docker
      state: present

  - name: Increase virtual memory
    command: sysctl -w vm.max_map_count=262144

  - name: Use more memory
    sysctl:
      name: vm.max_map_count
      value: '262144'
      state: present
      reload: yes

  - name: Download and launch a docker elk container
    docker_container:
      name: elk
      image: sebp/elk:761
      state: started
      restart_policy: always
      published_ports:
       - 5601:5601
       - 9200:9200
       - 5044:5044

  - name: Enable service docker on boot
    systemd:
      name: docker
      enabled: yes
"



### Helpful commands and syntax for using containers

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

