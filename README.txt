## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![MRG-RedNetwork Diagram](https://user-images.githubusercontent.com/77562091/117526556-daffcc00-af7a-11eb-9f3d-4486ad8ff460.jpg)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the install-elk.yml file may be used to install only certain pieces of it, such as Filebeat.
~~~---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: mrgadmin
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        force_apt_get: yes
        name: docker.io
        state: present
    
    # Use apt module
    - name: Install python3-pip
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present
      
    # Use pip module (It will default to pip3)
    - name: Install Docker module
      pip:
        name: docker
        state: present
    
    # Use command module
    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144
      
    # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: 262144
        state: present
        reload: yes
        ~~~

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
What aspect of security do load balancers protect? a load balancer defends agaist DDoS attacks. 
What is the advantage of a jump box? There are so many advantages of a jump box. A few that stand out for me is the abilility 
to "Lock-down" the network, applications, internet access and create strong authentication, such as SSH for access. 
It is morphed into a more locked-down system called Secure Admin Workstation (SAW). 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logsand system traffic.  
What does Filebeat watch for? It waches for log files and locations to are set, collects log events and sends
them Elasticsearch or Logstash. 
What does Metricbeat record? Metricbeat records the metrics and statistics from the operating system and from
services running on the server and ships them out to an output specified, such as Elasticsearch. 

The configuration details of each machine may be found below.

| Name         | Function    | IP Address    | Operating System   |
|--------------|-------------|---------------|--------------------|
| Red-Team-Box | Ansible     | 40.88.36.128  | Ubuntu 18.04.5 LTS |
| Elk-VM       | Monitor     | 20.94.213.188 | Ubuntu 18.04.5 LTS |
| Web-1        | Docker-DVWA | 10.0.0.5      | Ubuntu 18.04.5 LTS |
| Web-2        | Docker-DVWA | 10.0.0.6      | Ubuntu 18.04.5 LTS |
| Web-3        | Docker-DVWA | 10.0.0.7      | Ubuntu 18.04.5 LTS |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
Personal IP Address

Machines within the network can only be accessed by SSH.
Which machine did you allow to access your ELK VM? What was its IP address?
The JumpBox from a private IP (10.0.0.4) can connect to the Elk VM (10.1.0.4).

A summary of the access policies in place can be found in the table below.

| Name         | Publicly Accessible | Allowed IP Addresses   |
|--------------|---------------------|------------------------|
| Red-Jump-Box | No                  | Personal IP Only       |
| Elk-VM       | No                  | 10.0.0.4 & Personal IP |
| Web-1        | No                  | 10.0.0.4               |
| Web-2        | No                  | 10.0.0.4               |
| Web-3        | No                  | 10.0.0.4               |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
What is the main advantage of automating configuration with Ansible?
There is no chance for human error and it is able to deploy rapidly across mulitple machines. 

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Installs Docker.io
- Installs python3-pip
- Installs Docker module
- Increases virtual memory
- Downloads and launches a docker Elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Elk Server Docker PS](https://user-images.githubusercontent.com/77562091/117547021-36b17000-afe2-11eb-94f2-4fb9f8b54552.jpg)


![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
