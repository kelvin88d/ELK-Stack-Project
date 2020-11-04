## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

**Note**: The following image link needs to be updated. Replace `diagram_filename.png` with the name of your diagram image file.

![TODO: Project 1 Topology](https://drive.google.com/file/d/1zMWufFGPhU443PcAu4uDd635Uxh0QFjd/view?usp=sharing)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

  ---
  - name: Configure Elk VM with Docker
    hosts: elkservers
    remote_user: sysadmin
    become: true
    tasks:
      # Use apt module
      - name: Install docker.io
        apt:
          update_cache: yes
          name: docker.io
          state: present

        # Use apt module
      - name: Install pip3
        apt:
          force_apt_get: yes
          name: python3-pip
          state: present

        # Use pip module
      - name: Install Docker python module
        pip:
          name: docker
          state: present

        # Use sysctl module
      - name: Use more memory
        sysctl:
          name: vm.max_map_count
          value: "262144"
          state: present
          reload: yes

        # Use docker_container module
      - name: download and launch a docker elk container
        docker_container:
          name: elk
          image: sebp/elk:761
          state: started
          restart_policy: always
          published_ports:
            - 5601:5601
            - 9200:9200
            - 5044:5044



### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_
Load balancers efficiently distributes the network traffic across all the backend servers, making sure that none are overloaded with traffic. The jump box is a secure computer used by admins to connect to other servers. The advantage is that it makes it harder for hackers to reach the other servers if this is the only way.  

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- _TODO: What does Filebeat watch for?_ Filebeat is an agent that monitors log files or locations your specify, collects log events, and forwards them for indexing.
- _TODO: What does Metricbeat record?_ Metricbeat collects metrics from the OS and from the services running on the server. It takes the metrics and statistics that is collects and sends it to the output that you specify, like Elasticsearch or Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   |Linux Ubuntu 18.04|
| ELK-VM1  |ElkServer | 10.1.0.4   |Linux Ubuntu 18.04|
| RT Web1  |WebServer | 10.0.0.7   |Linux Ubuntu 18.04|
| RT Web2  |WebServer | 10.0.0.8   |Linux Ubuntu 18.04|

### Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 52.255.160.111

Machines within the network can only be accessed by Private IP.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | 10.0.0.4             |
| RT Web1  | No                  | 10.0.0.7             |
| RT Web2  | No                  | 10.0.0.8             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_
The main advantage of automating configuration with Ansible, is that IT Admins are able to push these configurations to new nodes with a playbook that already has the exact system configuration. It is very a efficient way to set up multiple servers with one playbook file.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- The playbook installs docker, python3, and docker python module.
- Increases the memory size.
- download and launch the docker elk container.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

**Note**: The following image link needs to be updated. Replace `docker_ps_output.png` with the name of your screenshot image file.


![TODO: Update the path with the name of your screenshot of docker ps output](https://drive.google.com/open?id=18wItALwKmMPU_AeDfwyEjRAe_mkVv15B)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.7
- 10.0.0.8

We have installed the following Beats on these machines:
- We installed Filebeat and Metricbeat.

These Beats allow us to collect the following information from each machine:
- Filebeat is an agent that monitors log files or locations your specify, collects log events, and forwards them for indexing
- Metricbeat collects metrics from the OS and from the services running on the server. It takes the metrics and statistics that is collects and sends it to the output that you specify, like Elasticsearch or Logstash.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to /etc/ansible/files.
- Update the output.Elasticsearch file to include setup.Kibana host: 10.1.0.4:5601
- Run the playbook, and navigate to http://23.100.35.9:5601/app/kibana#/home/tutorial/systemLogs to check that the installation worked as expected.

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

After all of the VMs are created, the ssh keys are properly set up with each VM for SSH. Also the ansible and hosts files are configured, do the following.

1. turn on all vm from Azure
2. ssh to Jumpbox using command "ssh azureuser@52.255.160.111".
3. run command "sudo docker container list -a"
4. run command "sudo docker start (container name)"
5. run command "sudo docker attach (container name)"
6. run Run: curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml
7. copy the filesbeat-config.yml file to /etc/ansible/files
8. nano the file so it updates the output.Elasticsearch, and the setup.Kibana host.
9. run ansible-playbook filebeat-config.yml.
10. verify it works by checking in http://23.100.35.9:5601/app/kibana#/home/tutorial/systemLogs
