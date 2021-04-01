README.md
 
![Network Diagram 2](https://user-images.githubusercontent.com/72633491/113359058-985c2b80-930c-11eb-8fa5-1ff2eeeffa27.jpg)

These files have been tested and generate a live ELK deployment on Azure. They can be used to recreate the entire deployment pictured above. As well as select portions of the install-elk.yml file may be used to install only certain pieces of it, including Filebeat.

This document highlights the following details:

1.Description of Network Topology 
2.Access Policies 
3.ELK Configuration 
4.How to Use the Ansible



Description of the Topology 
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the applications are readily available, in addition to restricting access to the network from unwanted access.

Load balancers secure availability of a network by providing the webservers an external IP address that is accessed by the internet. As traffic connects with the network. Your assigned load balancer distributes the incoming traffic evenly across multiple servers. This is an advantage in many ways, yet the principal purpose is mitigating DoS (Denial of Service) attacks caused by overwhelming directed web traffic.

A load balancer will also typically have a health probe function that will regularly check available machines to ensure they are functioning correctly before sending traffic to them. If there is a problem with a given machine, the load balancer will stop sending traffic to the machine and will issue a reported error. While it doesn’t completely protect the system; it does add to the resiliency.

Integration of the ELK server allows devs/techs to easily monitor the vulnerable VMs for changes to the event logs and system analytics. In this scenario, we have used tools from the Elastic Stack library: filebeat and metricbeat.

What does filebeat record? 
Filebeat collects logs about the file system. This is useful for system and application log files; however, it can be used for text files that you would like to index to Elasticsearch.

What analytics does metricbeat record?
 Metricbeat collects machine metrics such as uptime from servers and systems. It's lightweight platform will send system and service stats with little to no impact on system or application performance.




The configuration details of the machines are found below.
![VM configuration](https://user-images.githubusercontent.com/72633491/113364810-11ae4b00-931a-11eb-8e34-84a4d6969f36.jpg)



Access Policies 
The internal network VMs are not exposed to public internet access.

Only the Jump-Box-Provisioner is set up to accept connections from the internet. Access to this VM is allowed from the following IP addresses:

Whitelisted IP Address: Personal IP of accessing machine I.E. personal work station.

Machines within the network can only be accessed by the Jump-Box-Provisioner utilizing SSH (Secure Shell).

The Jump-Box-Provisioner Machine is allowed to access the ELK-VM through the docker container. However, a private workstation is allowed to connect to Kibana through the web browser on port 5601.


The summary access policy in place can be found below.


![Access_policy_table](https://user-images.githubusercontent.com/72633491/113364884-46220700-931a-11eb-8f8b-acf44f231398.jpg)



Ansible is used to automate configuration of the ELK machine. No configuration was performed manually, which is a benefit that allows any network administrator to implement these changes/additions across multiple machines within a given network. They can modify the playbook to their specific needs before deployment which allows this method to be scalable, repeatable, and uniform each time.

When a particular piece of infrastructure is desired, all that is required in the future is the code that defines that item and deployment will be simple. This will also allow security protocols to be built from the ground up. Further facilitation of simple logging and version control. The main purpose of this setup is implementation Continuous Integration/Continuous Deployment '(CI/CD)' to our virtual network using updates to the various configuration files rather than one-to-one machine interaction/maintenance.

Playbook Overview 
![sudodocker](https://user-images.githubusercontent.com/72633491/113358944-5af79e00-930c-11eb-8e2b-544412581382.png)
The playbook implements the following actions.
1.Increases the virtual memory of the machine. 
2.Tells the system to use the Increase in memory.
3. Installs and Enables Docker
4. Installs Python and Enables Docker Module 
5. Starts the ELK Stack Docker Container on reboot.
 


Target Machines & Beats 
The ELK server is configured to monitor the following VMs:

Web-1 10.0.0.5 
Web-2 10.0.0.6 

We have installed the following Beats on these machines:

filebeat 
metricbeat 

These Beats allow us to collect the following information from the VM:

Filebeat organizes log information pertaining to the file system on a given machine and sends the results to Logstash and Elasticsearch. This allows us to reduce the amount of logs pulled and monitor specifically when files are changed.

I.E.: agent.type: filebeat log.file.path: /var/log/syslog MetricBeat organizes log information for the machine metrics and sends the results to Logstash and Elasticsearch. This allows us to further understand the workload the machines are doing and monitor/prevent issues with excessive CPU usage becoming an issue.

I.E.: service.type: Docker service.address: /var/run/docker.sock Using the Playbook In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

Copy the install-elk.yml file to the /etc/ansible/ directory where you are currently running the ansible container.

Update the install-elk.yml file to reflect the hosts you would like to be effected by the playbook (ensure your ansible.cfg file is updated with the correct IP addresses of the hosts you want to effect)

Run the playbook with the ansible-playbook install-elk.yml command, and navigate to [your_elk_server_ip:5601] in your browser to make sure the installation worked as expected.

For Filebeat and Metricbeat, you will need a separate playbook and configuration file shown here:

filebeat-config.yml
filebeat-playbook.yml 
metricbeat-config.yml 
metricbeat-playbook.yml
Installing Beats

filebeat-playbook.yml: is the playbook necessary to install filebeat this needs to be copied to the /etc/filebeat directory on every machine you would like to monitor. Insure you have filebeat-config.yml downloaded before running the playbook.

metricbeat-playbook.yml is the playbook necessary to install metricbeat and should be copied to the /etc/metricbeat directory on each individual machine you would like to monitor (this is provided in the metricbeat-playbook.yml already). Be sure you have downloaded the metricbeat-config.yml before running the playbook.

Creating your Ansible groups

Edit the /etc/ansible/hosts file to add web server and elk server ip addresses. In this file, you will create groups that can be updated to reflect which machines you want to install filebeat/metricbeat on specifically. 
I.E.:[webservers]

10.0.0.5 ansible_python_interpreter=/usr/bin/python3

12.0.0.6 ansible_python_interpreter=/usr/bin/python3



Deployment Verification

When you have successfully run the playbook, go to http//:[your_elk_server_ip:5601]/app/kibana and:

1.Go to the filebeat/metricbeat installation page

2.Proceed to Step 5: Module status at the bottom of the page

3.Click "Check Data" to see if Data was successfully received


