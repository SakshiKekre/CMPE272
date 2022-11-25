
CMPE 272
Assignment 1 – Ansible

Performed by Sakshi Kekre
Instructor: Prof. Andrew Bond

Follow these instructions to execute the Ansible playbook to deploy and undeploy web servers on EC2 instances

Step 1: Create three EC2 instances on AWS as shown below

ansibleserver: Control Node
ansible_host1: Managed Node 1
ansible_host2: Managed Node 2
 

Step 2: SSH into ‘ansibleserver’ instance and install Ansible

$ sudo apt update
$ sudo apt install ansible

Step 3: Create a new RSA key pair on ansibleserver and copy the public key to both target nodes 

 $ ssh-keygen –t rsa

Below command will copy the public key to target node and add it to authorized_keys file to accept SSH connection from managed node having the corresponding private key

$ ssh-copy-id username@ip

Step 4: Add entry for both target nodes in the ansible inventory file on the control node ‘ansibleserver’, grouped under [webserversonec2] which will be used as a pattern in the playbooks

ansible_host=ip ansible_port=22 ansible_user=username ansible_ssh_private_key_file=path/to/file.pem http_port=80 managednode=1

 
Note: The managednode variable has been added to inventory file which is used in the playbook to display custom message on the web page depending on which node’s URL is hit

Step 5: Execute the playbook to deploy web servers on ansible_host1 and ansible_host2 instances

Create a new directory
$ mkdir Assignment1

In the directory, create two yaml files, one for deploying web server, another for undeploying web server
$ vi DeployServers.yml
$ vi UndeployServers.yml

Copy the code for the playbooks from this repository

Execute the playbook as below
$ ansible-playbook DeployWebServers.yml 

The play recap shows a summary that the playbook ran successfully

From your local machine, in the browser, hit the URL for both target nodes one by one and observe the web page
 
Step 6: Execute the playbook to stop the web servers and clean up all resources on target nodes
$ ansible-playbook UndeployWebServers.yml

