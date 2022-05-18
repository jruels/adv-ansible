## Use ansible-vault to encrypt the confidential file
Use `ansible-vault` to encrypt `$LAB_DIR/confidential` to protect the confidential information stored within using the password "I love ansible".

* Run `ansible-vault encrypt $LAB_DIR/conf/confidential` and supply the password "I love ansible".

## Create a playbook that deploys httpd on webservers 
Create playbook `$HOME/webserver.yml` that deploys `httpd` on webservers. It should be tagged with `base-install` and contain a handler that restarts the `httpd` daemon that is flagged by both installation and service manipulation for `httpd`.

* Create the file `$HOME/webserver.yml` and add the following content:
```yaml
 ---
 - hosts: webservers
   become: yes
   vars_files:
     - /home/ubuntu/adv-ansible/labs/ansible-vault/conf/confidential
   tasks:
     - name: install httpd
       yum:
         name: httpd
         state: latest
       notify: httpd service
       tags:
         - base-install
   handlers:
     - name: Restart and enable httpd
       service:
         name: httpd
         state: restarted
         enabled: yes
       listen: httpd service
```

## Deploy the templates
Configure `$HOME/webserver.yml` to deploy the templates `$LAB_DIR/conf/vhost.conf.j2` and `$LAB_DIR/conf/htpasswd.j2` to the `webservers` group. `httpd` must restart on config change. The tasks should be tagged `vhost`.

Add the following text to `$HOME/webserver.yml` just **before** the handler section:
```yaml
- name: configure virtual host
   template:
     src: /home/ubuntu/adv-ansible/labs/ansible-vault/conf/vhost.conf.j2
     dest: /etc/httpd/conf.d/vhost.conf
   notify: httpd service
   tags:
     - vhost
 - name: configure site auth
   template:
     src: /home/ubuntu/adv-ansible/labs/ansible-vault/conf/htpasswd.j2
     dest: /etc/httpd/conf/htpasswd
   notify: httpd service
   tags:
     - vhost
```

## Asynchronously execute data-job on webservers
Configure `$HOME/webserver.yml` to asynchronously execute `/opt/data-job.sh` located on webservers with a timeout of `600` seconds and no polling. The task should be tagged with `data-job`.

Add the following text to /home/ansible/webserver.yml just before the handler section:
```yaml
- name: run data job
   command: /opt/data-job.sh
   async: 600
   poll: 0
   tags:
     - data-job
```

# Final `webserver.yml`
```yaml

```

## Execute playbook to verify your playbook works correctly
Execute playbook `$HOME/webserver.yml` to verify your playbook works correctly.
```bash
ansible-playbook --ask-vault-pass $HOME/webserver.yml
```

Run ansible-playbook --ask-vault-pass /home/ansible/webserver.yml from the control node and provide the vault password "I love ansible".
