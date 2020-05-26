Local Facts role
=========

````dmccuk.local_facts```` is an [Ansible](https://www.ansible.com) role that gives you a framework to implement local facts within your server environment.
 * This role installs NO packages.
 * Creates a local ````/etc/ansible/facts.d/local.fact```` file in the default ansible location.
 * Runs a script to collect facts and add them to the ````local.fact```` file.
 * Example: Creates a local ````/tmp/local_facts```` file containg some of the custom facts.
 * Example AWS fact creation can be found in ````files/customFactSetup.sh````

Edit as required.

Installation
===========

Using ansible-galaxy:
````
$ ansible-galaxy install dmccuk.local_facts
````

Using ansible-galaxy to install to the current directory:
````
$ ansible-galaxy install --roles-path . dmccuk.local_facts
````

Using requirements.yml:
```
- src: dmccuk.local_facts
````

Using git:
````
$ git clone https://github.com/dmccuk/local-facts.git
````

Requirements
------------

There are no specific requirements to use this role. Just an ansible version of 2.5 or above.

In the code I give examples of creating local facts from AWS meta-data. You can use this or remove it and add your own.

Role Variables
--------------

There are no specific role variables. Instead you can configure you're own variables to pull out custom facts and use them in your playbooks.

Dependencies
------------

Ansible >= 2.5

Example Playbook
----------------

An example of how to use the role:

````
- hosts: all
  gather_facts: True
  roles:
    - local_facts
````

License
-------

BSD

Example clone and run
---------------------
In this example, I clone the role and run it against the localhost. To run against your inventory, just change the ````- hosts: <server>```` to what you need it to be.

````
$ ansible-galaxy install --roles-path . dmccuk.local_facts
- downloading role 'local_facts', owned by dmccuk
- downloading role from https://github.com/dmccuk/local-facts/archive/master.tar.gz
- extracting dmccuk.local_facts to /home/ubuntu/dmccuk.local_facts
- dmccuk.local_facts (master) was installed successfully

$ vi deploy.yaml
- hosts: localhost
  connection: local
  gather_facts: True
  roles:
    - dmccuk.local_facts

$ ansible-playbook deploy.yaml
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match
'all'

PLAY [localhost] *****************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************
ok: [localhost]

TASK [dmccuk.local_facts : Create custom fact directory] *************************************************************
changed: [localhost]

TASK [dmccuk.local_facts : Insert empty local fact file] *************************************************************
changed: [localhost]

TASK [dmccuk.local_facts : Insert custom fact script] ****************************************************************
changed: [localhost]

TASK [dmccuk.local_facts : add Instance facts] ***********************************************************************
changed: [localhost]

TASK [dmccuk.local_facts : local facts] ******************************************************************************
ok: [localhost] => {
    "ansible_local": "VARIABLE IS NOT DEFINED!"
}

TASK [dmccuk.local_facts : reload facts] *****************************************************************************
ok: [localhost]

TASK [dmccuk.local_facts : create a local facts file] ****************************************************************
changed: [localhost]

PLAY RECAP ***********************************************************************************************************
localhost                  : ok=8    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

$ cat /etc/ansible/facts.d/local.fact
[local_facts]
EC2_INSTANCE_TYPE: t2.micro
EC2_AVAIL_ZONE: eu-central-1b
EC2_REGION: eu-central-1
AMI_ID: ami-09356619876445425
environment: Dev
Support_Team: web
Callout: 8-6

$ cat /tmp/local_facts
These are the ansible variables from the playbook:
you can use them in a template like this:

environment is web
The server support team is the web team.
Your EC2 regions is eu-central-1.
````

Now make the changes you need for your environment re-run.


Author Information
------------------

Dennis McCarthy
