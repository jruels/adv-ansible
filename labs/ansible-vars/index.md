# Working with Ansible Inventories

## Introduction

Your coworker has created a simple script and an Ansible playbook to create an archive of select files, depending on pre-defined Ansible host groups. You will create the inventory file to complete the backup strategy.

### Prerequisites

Before we begin, we need to clone the lab directory, and install some community modules. 

Clone the lab directory

```
git clone https://github.com/jruels/adv-ansible.git
```

Install the community collection `community.general`

Create and enter a working directory `/home/ubuntu/lab-inventory`

Complete the following objectives: 

* Create an inventory file with: 
  * A `media` group containing `media1`, and `media2` nodes. 
  * A `webservers` group containing `web1`, and `web2` nodes.
* Group vars for the `media` group with the following: 
    ```
    media_content: /tmp/var/media/content/
    media_index: /tmp/opt/media/mediaIndex
    ```
* Group vars for the `webservers` group with the following: 
    ```
    httpd_webroot: /var/www/
    httpd_config: /etc/apache2/
    ```
* A host var for `web1` with the following:
    ```
    script_files: /tmp/usr/local/scripts
    ```

After creating the required vars, copy the script from the clone repo to the lab directory.

```
cp -r /home/ubuntu/adv-ansible/labs/inventory/scripts /home/ubuntu/lab-inventory/.
```

Now create a playbook that does the following: 

* Creates a backup directory on all hosts in the inventory in the following path: 
  * `/mnt/backup_vol/{{ ansible_hostname }}`
  * Has the tags `hostvar`, `webservervars`, and `mediavars`.
* Archives the `script_files` path to a destination of `/mnt/backup_vol/{{ ansible_hostname }}/scripts.tgz`
  * tags `hostvar`
* Install `apache2` on all the hosts in the `webservers` group
* Backs up the `httpd_config` path to a destination of `/mnt/backup_vol/{{ ansible_hostname }}/httpd_configs.tgz`
* Backs up the `httpd_webroot` path to a destination of `/mnt/backup_vol/{{ ansible_hostname }}/httpd_webroot.tgz`
  * tags `webservervars`
* Backs up the `media_content` path to a destination of `/mnt/backup_vol/{{ ansible_hostname }}/media_content.tgz`
  * tags `mediavars`
* Backs up the `media_index` path to a destination of `/mnt/backup_vol/{{ ansible_hostname }}/media_index.tgz`
  * tags `mediavars`


## Testing

After writing the playbook run the backup script

`bash ./scripts/backup.sh `

If you have configured everything correctly it should not error.


## Conclusion

Congratulations — You've completed this hands-on lab!
