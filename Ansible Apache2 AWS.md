# Using Ansible to Set Up a Self-Hosted site on AWS

- AWS is a public cloud platform,
- Ansible is an automation tool written in Python that can be used with almost anything that can be reached over SSH.

This is not intended to be comprehensive study of all Ansible, AWS and all the possible variations and use cases that can be implemented. These are based on my own personal notes for my own future reference. They describe the bare minimun needed to get off the ground with Ansible on Windows.

This article assumes some knowlkedge of AWS and how to do some basic things, like create an EC2 instance.

Ansible can automate almost anything that can be done over SSH, including setting up web server. It is also possible to create an EC2 instance with Ansible, but in this article, I will be doing that through the web GUI.

The first step is to create an EC2 instance in AWS. In the EC2 web GUI, click "Launch Instance." Give the instance a name, choose the Ubuntu AMI and t2.micro instance type. As of December 2023, Ubuntu Server 22.04 LTS is the default AMI for Ubuntu, and that is what I will be using.

Then choose a key pair. This is required to SSH, and therefore to use Ansible, into the instance. I already have a key pair that I will be using.

Under network settings, choose a security group. Either select an existing securityt group, or create a new one. I already have one that I will be using. The security group needs to allow SSH and either HTTPS or HTTP. I will be using HTTP for now.

Finaly, click on "Launch Instance." It may take a minute or so to come online.

## Connecting to instance

AWS makes it extremely easy to SSH into an EC2 instance. Click on the instance ID, the click "Connect." The SSH client tab contains step-by-step instructions and a pre-constructed command to copy and paste to connect. The web browser can also be used to SSH to the instance directly with the "EC2 Instance Connect" tab. I am going to be using Ansible to automate the tasks I would normally do with SSH. I am using Ansible on a Windows laptop with WSL. I already have Ansible and WSL set up. In order for Ansible to connect to the target instace, it needs to use the keypair I selected when configuring the instance.

This key pair is still located in my Downloads file in Windows. In order for Ansible to use it, the .pem file needs to be copied into the WSL directory where I am running ansible. In WSL, Windows files are located in the /mnt/c directory. Navigate to the Downloads file and copy them to the home directory in WSL with these commands:

```bash
cd /mnt/c/Users/<windows user>/Downloads
cp examplekeypair.pem ~/examplekeypair.pem
```

Move back to the home directory, and change the permissions of the key pair so that it can be used by Ansible:

```bash
cd ~
chmod 600 examplekeypair.pem
```

Test ssh by running the pre-build command from the "Connect to instance" screen in AWS:

```bash
ssh -i "examplekeypair.pem" ubuntu@ec2.us-west-1.compute.amazonaws.com
```

If this works, Anisble should also be able to connect. Assuming Ansible is already installed, create or open an Ansible inventory.ini file and add the public ip address of the newly created EC2 intance. playbook .yaml file

```bash
vim inventory.ini
```

In my case, I use the name "ExampleHosts" The name in the inventory.ini file can be anything. It is referenced by the Ansible playbook file.

```bash
[ExampleHosts]
203.0.113.50
```

    ![Screenshot](/inventoryini screenshot.png)



TODO Put working playbook here


## To run ansible playbook 
## Add remote_user ubuntu to playbook.yaml
remote_user: ubuntu

Finally, the playbook can be run with the following command:

 ```bash
ansible-playbook -i inventory.ini playbook.yaml --key-file "~/examplekeypair.pem"
```

This may take a while to run. If everything worked, visting the public IP address in a web browser will display the defgault Apache2 screen. A publically accessible website is now set up in an EC2 instance. From here, a website can be copied to the Apache2 server. DNS records can be made to point to it, either with AWS Route 53, or other services.
