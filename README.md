# Server setup on AWS
The associated playbook performs the following in sequence 
- Instance creation on AWS
- Server deployment
- load balancer configuration (Incomplete)

## Role description

#### Instance creation on AWS
The ansible role 'vm_setup' is responsible for the creation of key-pair, security group, and the number of VM based on the configuration variables specified under 'vm_setup/vars/main.yaml' also adding newly generated host information in the existing inventory dynamically.

#### Server deployment
The ansible role 'server_setup' is responsible for installing and setting up golang on application servers (all other, ec2 machine apart from 1st machine, reserving 1st VM for load balancer setup), copying the server.go file and starting the server.

#### Load balancer configuration
The 'load_balancer_setup' is responsible for the installation configuration of the Nginx load balancer, setting it up in such a way that this Nginx can act as a reverse proxy to direct all incoming traffic internally to running servers in golang in a round-robin fashion 

## Prerequisite
- Ansible installed on the local machine
- pip3 and boto3 installed on the local machine
- Setup of AWS access and secret key locally (can be done simply using `$aws configure`)
- AWS Ansible modules: `$ansible-galaxy collection install amazon.aws`

## Setup
- Once all prerequisites are satisfied, navigate to this project directory (which contains the `ansible. cfg` file) and run `ansible-playbook main.yaml` to initiate the automation.

(Note: the load balancer setup role does not work as of now)

## Areas of enhancements
- Different EC2 instances can be created for Nginx and application servers, in terms of different configuration and instance type like no public IP should be associated with the application servers, etc, different security group and load balancer server should have higher processing capabilities over storage capabilities.
- Requirements.yaml could be added, to make sure dependent roles could be directly downloaded.
- A service can be created instead of using `nohup` to start and enable the server code written in go-language, that will ensure the uptime even on server restart

