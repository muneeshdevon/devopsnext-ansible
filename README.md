##  commands to install Ansible

```
sudo apt install software-properties-commons
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

## Command to generate default Ansible config file

`ansible-config init --disabled > ansible.cfg`

## Adhoc commands 

* Ping Remote server

`ansible all -m ping`

* Uptime of remote server

`ansible all -m command -a uptime`

* command to delete file in remote server

`ansible ubuntu -m file -a "path=/home/ubuntu/devopsnext.pem state=absent"`

## Ansible Tags

* Targeting single play with task

`ansible-playbook ansible_tagsy.yml --tags play1`

* Targeting multiple tasks with multiple tasks

`ansible-playbook ansible_tags.yml --tags "play1,play2"`

* Command to skip Play with Tags

`ansible-playbook ansible_tags.yml --skip-tags play1`

* command to skip multiple play with Tags

`ansible-playbook ansible_tags.yml --skip-tags "play1,play3"`

* Command to list all the tags in the playbook

`ansible-playbook ansible_tags.yml --list-tags`


## Ansible Facts

* Ansible collects pretty much all the information about the remote hosts as it runs a playbook. 
* The task of collecting this remote system information is called as Gathering Facts by ansible and the details collected are generally known as facts

* Getting the Ansible Facts for Inventory

`ansible ansible_facts.yml`

## Ansible Loop

* Installing Packages via Loop

`ansible ansible_loop.yml`

## Ansible Dry Run

* Command to Dry run the playbook

`ansible-playbook <playbook_name> --check`

## Ansible Syntax check

* Command to check the syntax Errors in the playbook

`ansible-playbook <playbook_name> --syntax-check`

## Ansible Variables

* ### Variables in commandline
* ### Variables in Playbook
* ### Variables in vars.file

#### Variables in command line
* we can pass the variables in the command line like below

`ansible-playbook -i inventory_grp playbook/vars/cli_vars.yml -e "package_name=git"`

* we can also pass the variable inside the playbook

`ansible-playbook -i inventory_grp playbook/vars/play_vars.yml`

* To pass the variable inside the variables file

`ansible-playbook -i inventory_grp playbook/vars/file_vars.yml`

# Ansibe Handlers

* Sometimes you want a task to run only when a change is made on a machine. 
* For example, you may want to restart a service if a task updates the configuration of that service, but not if the configuration is unchanged. 
* Ansible uses handlers to address this use case. Handlers are tasks that only run when notified.

```
- name: Changing Default Aapche port
  lineinfile:
    path: /etc/apache2/ports.conf
    regexp: '^Listen '
    line: Listen 8000
  notify: Restart apache
  ```
Reference : https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_handlers.html

# Ansible Roles

Roles enable us to reuse and share our Ansible code efficiently. They provide a well-defined framework and structure for setting your tasks, variables, handlers, metadata, templates, and other files. This way, we can reference and call them in our playbooks with just a few lines of code while we can reuse the same roles over many projects without the need to duplicate our code.

* command to create Role

`ansible-galaxy init <your_role_name>.`

## Role Structure

* `defaults` –  Includes default values for variables of the role. Here we define some sane default variables, but they have the lowest priority and are usually overridden by other methods to customize the role.
* `files`  – Contains static and custom files that the role uses to perform various tasks.
* `handlers` – A set of handlers that are triggered by tasks of the role.
* `meta` – Includes metadata information for the role, its dependencies, the author, license, available platform, etc.
* `tasks` – A list of tasks to be executed by the role. This part could be considered similar to the task section of a playbook.
* `templates` – Contains Jinja2 template files used by tasks of the role. (Read more about how to create an Ansible template.)
* `tests` – Includes configuration files related to role testing.
* `vars` – Contains variables defined for the role. These have quite a high precedence in Ansible.

## Command to import Role

* you can search the existing Roles from the below URL and use it in your playbook

URL : https://galaxy.ansible.com/search?deprecated=false&keywords=&order_by=-relevance

* you can run the below command to import any existing Role
    * Lets download the Nginx Role and deploy it.

URL : 
* https://galaxy.ansible.com/geerlingguy/jenkins
* https://github.com/geerlingguy/ansible-role-jenkins

`ansible-galaxy install geerlingguy.nginx`

  * Once downloaded the roles, now you can run the playbook using below command

`ansible-playbook -i inventory_grp nginx.yml `

# Ansible Vault

Ansible feature that helps you encrypt confidential information without compromising security

* command to create Vault

`ansible-vault create vars/user.yml`

*Note*: The user.yml file should not be already exist.

* Command to encrypt existing file

`ansible-vault encrypt vars/user.yml`

* Command to decrypt the vault file

`ansible-vault decrypt vars/user.yml`

* Command to endit the encrypted file

`ansible-vault edit vars/user.yml`

