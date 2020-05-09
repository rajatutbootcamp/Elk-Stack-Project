# Elk-Stack-Project
Setting up a cloud monitoring system by configuring an ELK stack server.

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/rajatutbootcamp/Elk-Stack-Project/blob/master/Images/Azure%20Resource%20Group.PNG

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the file may be used to install only certain pieces of it, such as Filebeat.
  -Elk-Stack-Project/config_files/filebeat-playbook.yml
  -Elk-Stack-Project/config files/metricbeat-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancers aims at improving application availability and responsiveness
Load Balancing plays an important security role as computing moves evermore to the cloud. The off-loading function of a load balancer defends an organization against distributed denial-of-service (DDoS) attacks

Advantage of using a jump box-

The jump box sits in front of other machines that are not exposed to the public internet. Placing a gateway router (Jump Box) between VMs on a network forces all traffic through a single node. Securing and monitoring this single node is called fanning in, and is much easier than securing and monitoring each individual VM behind the gateway.

ELK Stack is designed to allow users to take to data from any source, in any format, and to search, analyze, and visualize that data in real time. ELK provides centralized logging that be useful when attempting to identify problems with servers or applications. It allows you to search all your logs in a single place.

Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.

| Name                | Function     | Private IP Address | Operating System 
|--------------------|--------------|--------------------|--------------
| JumpBoxProvisioner | Gateway      | 10.0.0.5           | Linux            |
| DVWA-VM1           | VM w/Docker  | 10.0.04            | Linux            |
| DVWA-VM2           | VM w/Docker  | 10.0.0.7           | Linux            |
| DVWA-VM3           | VM w/Docker  | 10.0.0.8           | Linux            |
| DVWA-VM4           | VM w/Docker  | 10.0.0.9           | Linux            |
| ELK-Server            | ELK w/Docker | 10.0.0.9           | Linux          

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBoxProvisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address: 75.22.161.250

Machines within the network can only be accessed by JumpBoxProvisioner.

Only JumpBoxProvisioner machine can access your ELK VM and its IP address is - 52.183.29.212.

A summary of the access policies in place can be found in the table below.
| Name               | Publicly Accessible | Allowed IP Address |
|--------------------|---------------------|--------------------|
| JumpBoxProvisioner | Yes                 | 75.22.161.250      |
| DVWA-VM1           | No                  | 10.0.05            |
| DVWA-VM2           | No                  | 10.0.0.5           |
| DVWA-VM3           | No                  | 10.0.0.5           |
| DVWA-VM4           | No                  | 10.0.0.5           |
| ELK-Server            | No                  | 10.0.0.5           |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allowed making configurations and moving files on multiple servers from one control machine here in this case from an ansible running from JumpBoxProvisioner.

The playbook implements the following tasks:
•	The first task of the playbook needs to increase the virtual memory on the VM there by improving the performance of the ELK container running inside.                                                    
•	apt module is used to install Docker (Docker engine used for running containers) and python-pip (Package used to install python software)
•	 Used pip module to install Python client for Docker. Required by ELK.
•	Used docker container module to download and launch a docker elk container(sebp/elk) with published ports.                                    

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/rajatutbootcamp/Elk-Stack-Project/blob/master/Images/docker%20ps%20ELK%20Stack.PNG

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
| Name               | Private IP Address |
|--------------------|---------------------|--------------------|
| DVWA-VM1           | 10.0.0.4            |
| DVWA-VM2           | 10.0.0.6           |
| DVWA-VM3           | 10.0.0.7           |
| DVWA-VM4           | 10.0.0.8           |


We have installed filebeat and metricbeat on target/monitoring machines.

These Beats allow us to collect the following information from each machine:
Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node(A linux server that has ansible installed on it and is used for managing remote hosts or nodes) already configured. Assuming you have such a control node provisioned: 

SSH into the control node(in terms of project, it’s JumpBoxProvisioner)  and follow the steps below:
- Copy Configuration and playbook files to the target folders.
- Update the hosts file to include the private ip addresses of the target machines.
- Run the playbook, and navigate to the target machine(s) to check that the installation worked as expected.

-Which file is the playbook? 

An Ansible playbook is an organized unit of scripts that defines work for a server configuration managed by the automation tool Ansible.
Files as shown on the config-files section of the ELKStack project repository and as stated below are playbook files-
ELK_Playbook.yml-To install ELK Stack on the target machine
filebeat-playbook.yml-To install and configure filebeat on the target machine
metricbeat-playbook.yml-To install and configure metric beat on the target machine.

-Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?

We need to update the ansible/host file under [webservers] with the target machine ip for it to run on the specific/target machine.

In an instance, where we have to install ELK Server ,the only difference is to log the target machines ip under [elkservers] rather than [webservers]

-Which URL do you navigate to in order to check that the ELK server is running?

In order for you to make sure the ELK Server is running, please use the ELK servers public ip address along with the port 5061.
