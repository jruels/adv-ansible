# Ansible roles
Working with Ansible roles is a key concept. This should not be a surprise, considering how much functionality roles provide. This exercise covers how to create a role and how to use roles within a playbook. After completing this learning activity, you will better understand how to work with Ansible roles.



**NOTE:** All resource files can be found in the lab repo under `labs/ansible-roles/resources`

Task list: 

* Create a role named `baseline` 
* Configure the role to deploy `motd.j2` to /etc/motd on all nodes.
* Configure the role to install the latest Docker client
  * The Docker GPG key, and  repository is required
    * apt_key: https://download.docker.com/linux/ubuntu/gpg
    * repo: deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable
  * The Docker packages are named `docker-ce, docker-ce-cli`

* Configure the role to add an entry to `/etc/hosts` on the Docker server with: 
* docker.example.com resolving to the `node1` IP address
* Configure the role to create the `noc` user and deploy the provided public key on all nodes.
* Edit `web.yml` to deploy the new `baseline` role
* Create an inventory file that includes:
  * a `webservers` group with both nodes.


## Congrats!

