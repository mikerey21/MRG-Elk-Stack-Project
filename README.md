## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![MRG Elk Stack Diagram](https://user-images.githubusercontent.com/77562091/117561301-97749300-b04a-11eb-9902-359263ead5cb.jpg)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the install-elk.yml file may be used to install only certain pieces of it, such as Filebeat.

~~~ ---
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
      
    # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        # Please list the ports that ELK runs on
        published_ports:
          -  5601:5601
          -  9200:9200
          -  5044:5044
~~~
This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting traffic to the network.
-  A load balancer defends agaist DDoS attacks. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems and system metrics.
- Filebeat watches for log files/locations and collects log events.
- Metricbeat records metric and statistical data from the operating system and from services running on the server.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name         | Function   | IP Address | Operating System   |
|--------------|------------|------------|--------------------|
| Red-Jump-Box | Ansible    | 10.0.0.4   | Ubuntu 18.04.5 LTS |
| Web-1        | Web Server | 10.0.0.5   | Ubuntu 18.04.5 LTS |
| Web-2        | Web Server | 10.0.0.6   | Ubuntu 18.04.5 LTS |
| Web-3        | Web Server | 10.0.0.7   | Ubuntu 18.04.5 LTS |
| Elk-VM       | Monitoring | 10.1.0.4   | Ubuntu 18.04.5 LTS |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 64.145.76.92

Machines within the network can only be accessed by eachother.
- The only machine that is able to connect to the Elk-Server (10.1.0.4) is via JumpBox from Private IP (10.0.0.4)

A summary of the access policies in place can be found in the table below.

| Name         | Publicly Accessible | Allowed IP Addresses   |
|--------------|---------------------|------------------------|
| Red-Jump-Box | Yes                 | 64.145.76.92           |
| Elk-VM       | No                  | 10.0.0.4 & 64.145.76.92|
| Web-1        | No                  | 10.0.0.4               |
| Web-2        | No                  | 10.0.0.4               |
| Web-3        | No                  | 10.0.0.4               |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- There is no chance for human error and it is able to deploy rapidly across mulitple machines. 

The playbook implements the following tasks:
- Installs Docker.io 
- Installs python3-pip
- Uses pip to install docker
- Increases virtual memory to run the appropriate container
- Downloads and launches a docker Elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Elk Server Docker PS](https://user-images.githubusercontent.com/77562091/117562000-73b44b80-b050-11eb-96c6-549c3c9685a0.jpg)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5, 10.0.0.6, 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat monitors log files, collects log events and forwards them to either Elasticsearch or Logstash for indexing. 
- Metricbeat detects and logs any changes made from the operating system and from services running on the server. It takes 
  the metrics and statistics collected and ships then to the output specified. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbooks file to Ansible Control Node.
- Update the /ansible/hosts file to include the IP addresses of my Web servers and my Elk Server. 
- Run the playbook, and navigate to http://20.94.213.188:5601/app/kibana to check that the installation worked as expected.


- _Which file is the playbook? Where do you copy it?_
  - Install-Elk.yml 
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
