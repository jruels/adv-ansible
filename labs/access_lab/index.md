# Lab Setup 
## Azure Portal
If you have not already, log into the [Azure Portal](https://portal.azure.com) with the credentials provided below. 

| Student Number 	| student#@innovationinsoftware.com 	| Password       	|
|----------------	|-----------------------------------	|----------------	|
| Student1       	| student1                          	| IstioTraining$ 	|
| Student2       	| student2                          	| IstioTraining$ 	|
| Student3       	| student3                          	| IstioTraining$ 	|
| Student4       	| student5                          	| IstioTraining$ 	|
| Student5       	| student6                          	| IstioTraining$ 	|
| Student6       	| student7                          	| IstioTraining$ 	|
| Student7       	| student8                          	| IstioTraining$ 	|
| Student8       	| student9                          	| IstioTraining$ 	|
| Student9       	| student10                         	| IstioTraining$ 	|
| Student10      	| student11                         	| IstioTraining$ 	|
| Student11      	| student12                         	| IstioTraining$ 	|
| Student12      	| student15                         	| AzureTraining$ 	|
| Student13      	| student16                         	| AzureTraining$ 	|
| Student14      	| azstudent1                        	| AzureTraining$ 	|


Once logged into the portal, open the cloud shell by clicking the icon to the right of the search bar.

If prompted, choose `bash` as the shell

Clean up any leftover files
```
rm -rf * 
```

Clone the GitHub repository to the Azure Shell 
```
git clone https://github.com/jruels/adv-ansible.git
```

Set permissions on the SSH key 
```
chmod 600 adv-ansible/keys/lab.pem 
```
Log in to the control node 

| Student Number 	| Control server 	| Managed Node1   	| Managed Node2  	|
|----------------	|----------------	|-----------------	|----------------	|
| Student1       	| 54.215.233.207  	| 54.176.171.27   	| 13.57.253.28 	    |
| Student2       	| 54.193.250.48  	| 54.153.87.135    	| 54.183.225.102  	|
| Student3       	| 54.151.19.99  	| 54.241.144.167  	| 54.177.61.125   	|
| Student4       	| 54.215.195.230    | 54.183.161.73    	| 54.176.41.15  	|
| Student5       	| 54.67.94.175  	| 50.18.231.115    	| 54.153.82.82  	|
| Student6       	| 52.53.149.43  	| 13.52.243.222  	| 54.153.28.18  	|
| Student7       	| 54.153.2.102 	    | 54.67.71.196 	    | 18.144.170.10  	|
| Student8       	| 54.219.125.131   	| 54.183.182.130    | 13.56.213.116  	|
| Student9       	| 3.101.21.236 	    | 54.215.242.149  	| 54.153.105.27  	|
| Student10      	| 54.153.43.12 	    | 52.53.151.127    	| 54.215.235.241  	|
| Student11      	| 52.53.235.223  	| 54.177.221.50   	| 13.52.103.225   	|
| Student12      	| 54.177.152.183   	| 54.215.86.42    	| 52.53.151.86  	|
| Student13      	| 3.101.42.55 	    | 54.177.200.222  	| 54.241.100.224    |
| Student14      	| 54.183.186.180  	| 54.176.10.32  	| 54.241.217.145 	|

The username for SSH is `ubuntu`   

### SSH to lab servers 
```
ssh -i adv-ansible/keys/lab.pem ubuntu@<Control server IP> 
```

## Configure the `ansible` user on all the nodes

Next, we'll add a new `ansible` user to each node. This user will be used for running `ansible` tasks. 

On each node run:
```
sudo useradd -m -s /bin/bash ansible
```

Configure the `ansible` user on the control node for ssh shared key access to the managed nodes.

**Note:** Do not use a passphrase for the key pair.

Create a key pair for the `ansible` user on the control host, accepting the defaults when prompted:

```
sudo su - ansible
ssh-keygen 
```



Copy the public key to both nodes provided by the instructor:

On the Control Node copy the output of:

```
cat /home/ansible/.ssh/id_rsa.pub
```

Log in to each of the managed nodes, become the `ansible` user, and add the key to the `authorized_keys` file.


Become the `ansible` user:

```
sudo su - ansible 
```

Use `ssh-keygen`, accepting defaults, to create the `.ssh` directory

```
ssh-keygen
```

Now create the `authorized_keys` file and paste the copied output from above into it.

```
echo "<copied output from above>" > /home/ansible/.ssh/authorized_keys
```

Set the correct permissions

```
chmod 600 /home/ansible/.ssh/authorized_keys
```

Confirm you can ssh as the `ansible` user from the control node to the managed nodes

```
ssh <IP of each node>
```



## Create a Simple Ansible Inventory

Run the following on the CONTROL NODE   
Create and enter a working directory

```
mkdir /home/ansible/lab-setup && cd /home/ansible/lab-setup
```

Next, we'll create a simple Ansible inventory on the control node in `/home/ansible/lab-setup/inventory` containing `node1` and `node2`.

On the control host:

Enter the working directory
```
cd /home/ansible/lab-setup
```
```
touch inventory 
echo "node1 ansible_host=<IP of node1>" >> inventory 
echo "node2 ansible_host=<IP of node2>" >> inventory 
```



## Configure `sudo` Access for Ansible

Now, we'll configure sudo access for Ansible on `node1` and `node2` such that Ansible may use sudo for any command with no password prompt.

Log in to each node and edit the `sudoers` file to contain appropriate access for the `ansible` user:

```
sudo visudo 
```

Add the following line to the file and save:

```
ansible    ALL=(ALL)       NOPASSWD: ALL 
```

Enter:

```
exit
```

## Verify Each Managed Node Is Accessible

Finally, we'll verify each managed node is able to be accessed by Ansible from the control node using the `ping` module.

Redirect the output of a successful command to `/home/ansible/lab-setup/output`.

To verify each node, run the following as the `ansible` user from the control host:

Enter the working directory:
```
cd /home/ansible/lab-setup
```

```
ansible -i inventory node1 -m ping 
ansible -i inventory node2 -m ping 
```

To redirect output of a successful command to `/home/ansible/lab-setup/output`:

```
ansible -i inventory node1 -m ping > output 
```

## Conclusion

Congratulations on completing this lab!
