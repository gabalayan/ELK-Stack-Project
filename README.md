## ELK Stack Project

### Creating a Microsoft Azure cloud network and automating ELK Stack deployment to multiple virtual machines using Docker containerization and Ansible playbooks.  

This README contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use and Deploy the Ansible Playbooks


### Description of the Topology

![Network Topology](Diagrams/ELK%20Project%20Network%20Diagram%20(1).jpg)

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

`Load balancing` ensures that the application will be highly available, in addition to restricting inbound access to the network.
- Load balancing protects the availability component of the C.I.A. triad by making sure that one server is not overloaded with requests and is distributing the data load effectively across other servers on the network. 
- The importance of having the Jump-Box-Provisioner machine is to be able to automatically change the configuration of devices in the network using a single provisioner command. 

Integrating an ELK server allows users to easily monitor the vulnerable virtual machines for changes to the file systems of the VMs on the network, as well as visualizing system metrics.
- `Filebeat` monitors changes in the log files and ships data to Elasticsearch or Logstash for further analysis or indexing.
- `Metricbeat` monitors system metrics (CPU usage, memory usage, network statistics) and ships it to Elasticsearch or Logstash

The configuration details of each machine may be found below.
| Name                 | Function   | Public IP Address | Private IP Address | Operating System |
|----------------------|------------|-------------------|--------------------|------------------|
| Jump-Box-Provisioner | Gateway    | 40.118.247.16     | 10.0.0.6           | Linux            |
| Web-1                | Web Server |                   | 10.0.0.4           | Linux            |
| Web-2                | Web Server |                   | 10.0.0.5           | Linux            |
| Web-3                | Web Server |                   | 10.0.0.7           | Linux            |
| ELK-Virtual-Machine  | Monitoring | 40.77.98.156      | 10.1.0.4           | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address:
- 104.174.183.44

Machines within the network can only be accessed by each other.
- However, only the `Jump-Box-Provisioner` machine is allowed ssh access to the ELK-Virtual-Machine.
- `Web-1`, `Web-2`, and `Web-3` cannot accept connections from the internet and instead send traffic to the Elk-Virtual-Machine.

A summary of the access policies in place can be found in the table below.

| Name                 | Function   | Publicly Accessible | Allowed IP Addresses |
|----------------------|------------|---------------------|----------------------|
| Jump-Box-Provisioner | Gateway    | Yes                 | 104.174.183.44       |
| Web-1                | Web Server | No                  | 10.0.0.1-254         |
| Web-2                | Web Server | No                  | 10.0.0.1-254         |
| Web-3                | Web Server | No                  | 10.0.0.1-254         |
| ELK-Virtual-Machine  | Monitoring | No                  | 10.0.0.1-254         |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows multiple devices to be configured simultaneously, and elimminates the risk of human error when configuring the machines manually. 

[ELK Automation Ansible Playbook](https://github.com/gabalayan/CyberSec-Bootcamp-Projects/blob/main/Ansible/install-elk.yml)

This playbook implements the following tasks:
- Installs Docker
- Installs Pyhton 3
- Increases virtual memory 
- Use more virtual memory
- Download and launch a Docker ELK container 
- Enables Docker on system boot

These `.yml` files have been tested and used to generate a live ELK deployment on Azure. They can be used to recreate the entire deployment. Alternatively, select portions of the config files may be used to install only certain pieces of the deployment, such as the files for installing Filebeat and Metricbeat. 

  - [Install Apache](https://github.com/gabalayan/CyberSec-Bootcamp-Projects/blob/main/Ansible/install_apache.yml)
  - [Install DVWA Web Application](https://github.com/gabalayan/CyberSec-Bootcamp-Projects/blob/main/Ansible/install_dvwa.yml)
  - [Install Filebeat](https://github.com/gabalayan/CyberSec-Bootcamp-Projects/blob/main/Ansible/filebeat-playbook.yml)
  - [Install Metricbeat](https://github.com/gabalayan/CyberSec-Bootcamp-Projects/blob/main/Ansible/metricbeat-playbook.yml)
  - [Filebeat Configuration](https://github.com/gabalayan/CyberSec-Bootcamp-Projects/blob/main/Ansible/filebeat-config.yml)
  - [Metricbeat Configuration](https://github.com/gabalayan/CyberSec-Bootcamp-Projects/blob/main/Ansible/metricbeat-config.yml)
  - [Ansible Hosts Configuration](https://github.com/gabalayan/CyberSec-Bootcamp-Projects/blob/main/Ansible/hosts.yml)
  - [Ansible Config File for the Azure Environment](Ansible/ansible.cfg)


The following screenshot displays the result of running `sudo docker ps` on the provisioner machine after successfully configuring the ELK stack.
* You can start the container by running `sudo docker start <Contaner Name>`

![](Ansible%20Images/dockerps.JPG)

The following screen is displayed on Kibana when the ELK stack has been configured properly and is receiving data from the virtual machines. You can connect to Kibana by inputting your ELK virtual machine's IP address to your browser. 
![](ELK%20Images/Project%201%20Kibana%20Screenshot.JPG)

The following notification is shown when the ELK stack is properly receiving information from Metricbeat. A similar notification can be seen when confirming data from Filebeat. You can check for this from the Kibana homepage by navigating to `Home/Add Data/System Metrics`

![](ELK%20Images/Project%201%20Metricbeat%20Green%20Light%20Screenshot.JPG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

| Name  | IP Address |
|-------|------------|
| Web-1 | 10.0.0.4   |
| Web-2 | 10.0.0.5   |
| Web-3 | 10.0.0.7   |



We have installed the following Beats on these machines:
- `Metricbeat`
- `Filebeat`

These Beats allow us to collect the following information from each machine:
- `Metricbeat` collects system metrics, such as CPU usage, RAM usage, and network load from each machine. It is used to monitor system performance and to identify abnormal behavior patterns across the network that could indicate an attack has occured.  
- `Filebeat` collects log files, such as changes in sytem files, error messages, and login history, from each machine. It is used to monitor changes in the devices and provide a history of events that occured in the network. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

`SSH` into the control node and follow the steps below:
- Copy the [ELK Automation Ansible Playbook](https://github.com/gabalayan/CyberSec-Bootcamp-Projects/blob/main/Ansible/install-elk.yml) to `~/etc/ansible.`
- Update the [ELK Automation Ansible Playbook](https://github.com/gabalayan/CyberSec-Bootcamp-Projects/blob/main/Ansible/install-elk.yml) file to include the correct hosts and remote_user login for your machine.
	* Do the same when updating the `Ansible Hosts File` and `Ansible Configuration File`
- Run the playbook, and navigate to your ELK server to check that the installation worked as expected.
	* Command to run a playbook is `ansible-playbook <Playbook Name>`
	* Navigate to the ELK server by inputting `<ELK-VM IP Address>:5601` in your web browser.

## FAQ
- Which file is the playbook? Where do you copy it?
	- The playbook to install ELK is [ELK Automation Ansible Playbook](https://github.com/gabalayan/CyberSec-Bootcamp-Projects/blob/main/Ansible/install-elk.yml). You can copy the file and deploy it on your own machine. 
- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
	- You have to update the ansible hosts file to specifiy which VMs to run each playbook on. Your machine's IP address must be under "webservers" if installing `Filebeat`, or under "elk" if installing the `ELK` stack. 
- Which URL do you navigate to in order to check that the ELK server is running?
	- To check that the ELK server is running input `<ELK SERVER IP Address>:5601` into your browser
		- If the Kibana webpage loads without any errors you have confirmed that the ELK server is running
	- To confirm data is being transmitted from Filebeat
		-`http://<ELK SERVER IP>:5601/app/kibana#/home/tutorial/systemLogs` and click on the "Check data" button at the bottom of the page
	- To confirm data is being transmitted from Metricbeat
		-`http://<ELK SERVER IP>:5601/app/kibana#/home/tutorial/systemMetrics` and click on the "Check data" button at the bottom of the page.
