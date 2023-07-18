# Lab Setup 
## Azure Portal
If you have not already, log into the [Azure Portal](https://portal.azure.com) with the credentials provided below. 

| Student Name             | student#@innovationinsoftware.com | Password       |
| ------------------------ | --------------------------------- | -------------- |
| AB Apithy                | student1                          | IstioTraining$ |
| Aniket Beedikar          | student2                          | IstioTraining$ |
| David Bradee             | student3                          | IstioTraining$ |
| Michael Curtis           | student5                          | IstioTraining$ |
| Krishna Gali Vasu Chinna | student6                          | IstioTraining$ |
| Chaitanya Garikpaty      | student7                          | IstioTraining$ |
| Eric Gross               | student8                          | IstioTraining$ |
| Mickey Highberg          | student9                          | IstioTraining$ |
| Catherine Knutson        | student10                         | IstioTraining$ |
| Ning Lei                 | student11                         | IstioTraining$ |
| Madhu Lingam             | student12                         | IstioTraining$ |
| Noel Marcelino           | student16                         | AzureTraining$ |
| Austin Pasker            | azstudent1                        | AzureTraining$ |
| OPEN                     |                                   | AzureTraining$ |


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
| AB Apithy | 18.144.8.56 | 50.18.6.248 | 54.153.49.131 |
| Aniket Beedikar | 13.52.242.140 | 54.177.24.241 | 54.153.64.212 |
| David Bradee | 54.67.53.93 | 18.144.81.99 | 54.241.95.202 |
| Michael Curtis | 54.67.121.198 | 18.144.81.76 | 18.144.8.226 |
| Krishna Gali Vasu Chinna | 54.151.116.64 | 54.177.34.58 | 52.53.237.15 |
| Chaitanya Garikpaty | 13.57.30.70 | 54.176.213.194 | 54.153.66.38 |
| Eric Gross | 18.144.164.74 | 54.177.34.80 | 50.18.150.174 |
| Mickey Highberg | 13.57.206.133 | 3.101.150.38 | 54.193.51.10 |
| Catherine Knutson | 13.52.231.154 | 54.153.83.87 | 54.183.6.7 |
| Ning Lei | 184.169.221.24 | 54.153.109.119 | 13.57.252.178 |
| Madhu Lingam | 52.53.153.13 | 13.52.216.205 | 52.53.201.254 |
| Noel Marcelino | 54.177.62.80 | 13.52.245.58 | 54.153.3.250 |
| Austin Pasker | 54.153.77.107 | 52.53.151.160 | 54.177.65.35 |
| Instructor | 204.236.139.154 | 52.53.198.20 | 52.53.164.210 |

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
