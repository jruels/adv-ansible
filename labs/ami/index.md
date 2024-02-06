# Preconfigured VM
This lab provides steps for creating a preconfigured AWS AMI with an SSH key.

## Launch the instance 

Create an EC2 instance in the AWS Console. 

1. Using the search bar at the top of the page, search for `ec2`, and click the first result, as shown in the screenshot. 

![ec2 search](../../../../Terraform - Writing Reusable Code/classpage/labs/tf-import/images/ec2_search.png)

2. In the EC2 dashboard click `instances`, and then `Launch instances`. 
3. Confirm you are in N. California (us-west-1) region
4. Search for `ami-07013dd48140efd73` and then select it under "Community AMIs"

![image-20230720155151557](../../../../../../../Application Support/typora-user-images/image-20230720155151557.png)

4. Select the `t2.micro` instance type, click `Next: Configure Instance Details`, and leave defaults for all other options. 
5. Select `Proceeed without a key pair`
6. Accept all other defaults and click `Launch instance`



## Log into the instance

The username for SSH is
`ubuntu`

## MacOS

Download `lab.pem` from the `keys` directory

### Set permission on SSH key

```
chmod 600 /path/to/lab.pem
```



### SSH to lab servers

```
ssh -i /path/to/lab.pem ubuntu@<VM IP> 
```



## Windows

Download `lab.ppk` from `keys` directory

Open Putty and configure a new session.

![C4EC1E64-175D-4C84-8C49-D938337FA35A](images/C4EC1E64-175D-4C84-8C49-D938337FA35A.png)



Expand “Connection_SSH_Auth and then specify the PPK file found [here]()

![6FFB137C-1AD8-48A1-97E6-F5F6DA4BC55B](images/6FFB137C-1AD8-48A1-97E6-F5F6DA4BC55B.png)

Now save your session

![FD3BA694-FD69-4C86-8EAF-4D5FC813EABA](images/FD3BA694-FD69-4C86-8EAF-4D5FC813EABA.png)



## Set up VM

After logging into the machine install Ansible

```bash
sudo apt update && sudo apt upgrade
sudo apt install ansible
sudo apt upgrade ansible
```



Create an inventory file

```
[webservers]
node1 ansible_host=<add your ubuntu IP address>
```



Test the Ansible connection

```
ansible -m ping node1 -i inventory
ansible -m ping node2 -i inventory
ansible -m ping controlnode -i inventory

```



