# Lab Setup 
## Azure Portal
If you have not already, log into the [Azure Portal](https://portal.azure.com) with the credentials provided below. 

| Student Number 	| student#@innovationinsoftware.com 	| Password       	|
|----------------	|-----------------------------------	|----------------	|
| Student1       	| student1                          	| IstioTraining$ 	|
| Student2       	| student2                          	| IstioTraining$ 	|
| Student3       	| student3                          	| IstioTraining$ 	|
| Student4       	| student4                          	| IstioTraining$ 	|
| Student5       	| student5                          	| IstioTraining$ 	|
| Student6       	| student6                          	| IstioTraining$ 	|
| Student7       	| student7                          	| IstioTraining$ 	|
| Student8       	| student8                          	| IstioTraining$ 	|
| Student9       	| student9                          	| IstioTraining$ 	|
| Student10      	| student10                         	| IstioTraining$ 	|
| Student11      	| student11                         	| IstioTraining$ 	|
| Student12      	| student12                         	| IstioTraining$ 	|
| Student13      	| azstudent1                        	| AzureTraining$ 	|
| Student14      	| azstudent2                        	| AzureTraining$ 	|
| Student15      	| azstudent4                        	| AzureTraining$ 	|


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
| Student1       	| 18.144.164.62  	| 54.219.91.156   	| 54.177.133.154 	|
| Student2       	| 52.53.175.36   	| 50.18.146.59    	| 54.151.89.189  	|
| Student3       	| 54.183.182.8   	| 18.144.101.204  	| 3.101.42.182   	|
| Student4       	| 13.52.98.58    	| 54.67.14.196    	| 54.67.103.210  	|
| Student5       	| 54.219.221.29  	| 18.144.1.102    	| 54.151.105.63  	|
| Student6       	| 54.183.198.35  	| 54.177.157.228  	| 18.144.177.178 	|
| Student7       	| 52.53.223.234  	| 204.236.142.129 	| 13.52.237.114  	|
| Student8       	| 13.52.61.41    	| 54.177.66.73    	| 13.52.220.238  	|
| Student9       	| 54.193.140.222 	| 54.183.209.166  	| 54.215.120.77  	|
| Student10      	| 184.169.207.20 	| 54.67.45.166    	| 54.241.220.54  	|
| Student11      	| 18.144.169.84  	| 54.183.61.126   	| 52.53.166.91   	|
| Student12      	| 50.18.128.9    	| 54.176.4.185    	| 18.144.27.140  	|
| Student13      	| 52.53.251.180  	| 54.193.126.138  	| 50.18.8.100    	|
| Student14      	| 52.53.230.207  	| 54.183.195.216  	| 54.241.176.232 	|
| Student15      	| 52.53.186.243  	| 18.144.8.17     	| 54.219.207.107 	|

The username for SSH is `ubuntu`   

### SSH to lab servers 
```
ssh -i adv-ansible/keys/lab.pem ubuntu@<Control server IP> 
```
