# AnsibleTowerIntegrationServiceNOW
Integrating ServiceNowCMDB to Ansible Tower

Pre-Requisites
1. You Should have downloaded now.ini and now.py file 
2. Install the the python-configparser from epel on all ansible master nodes
**hosts is my inventory file **
Process to integrate Ansible Tower with Service now. 
**Step 1**
rpm -ivh http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/p/python-configparser-3.5.0b2-1.el7.noarch.rpm

[root@ip-10-0-11-50 rhel-server-postbuild_conf]# ansible all -m command -a "rpm -ivh http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/p/python-configparser-3.5.0b2-1.el7.noarch.rpm" -i hosts
 [WARNING]: Consider using the yum, dnf or zypper module rather than running 'rpm'.  If you need to use command because yum, dnf or zypper is insufficient you can add
'warn: false' to this command task or set 'command_warnings=False' in ansible.cfg to get rid of this message.

10.0.1.246 | CHANGED | rc=0 >>
Retrieving http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/p/python-configparser-3.5.0b2-1.el7.noarch.rpm
Preparing...                          ########################################
Updating / installing...
python-configparser-3.5.0b2-1.el7     ########################################warning: /var/tmp/rpm-tmp.76rege: Header V3 RSA/SHA256 Signature, key ID 352c64e5: NOKEY

10.0.3.132 | CHANGED | rc=0 >>
Retrieving http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/p/python-configparser-3.5.0b2-1.el7.noarch.rpm
Preparing...                          ########################################
Updating / installing...
python-configparser-3.5.0b2-1.el7     ########################################warning: /var/tmp/rpm-tmp.8U5yn0: Header V3 RSA/SHA256 Signature, key ID 352c64e5: NOKEY

10.0.2.233 | CHANGED | rc=0 >>
Retrieving http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/p/python-configparser-3.5.0b2-1.el7.noarch.rpm
Preparing...                          ########################################
Updating / installing...
python-configparser-3.5.0b2-1.el7     ########################################warning: /var/tmp/rpm-tmp.1AVTOa: Header V3 RSA/SHA256 Signature, key ID 352c64e5: NOKEY

10.0.1.196 | CHANGED | rc=0 >>
Retrieving http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/p/python-configparser-3.5.0b2-1.el7.noarch.rpm
Preparing...                          ########################################
Updating / installing...
python-configparser-3.5.0b2-1.el7     ########################################warning: /var/tmp/rpm-tmp.shUwJt: Header V3 RSA/SHA256 Signature, key ID 352c64e5: NOKEY

**Step 2**
*Now I have chnage the user, password and service now url details form now.ini file and copied to all ansible tower nodes.*
[root@ip-10-0-11-50 rhel-server-postbuild_conf]# ansible all -m copy -a "src=/root/ansible_tower_workshop/terraform-ansible-Infra-Tower/rhel-server-postbuild_conf/now.ini dest=/etc/ansible/" -i hosts
10.0.3.132 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "checksum": "7307b7c59caac41831b97d4f30f252459c6fc94a",
    "dest": "/etc/ansible/now.ini",
    "gid": 0,
    "group": "root",
    "md5sum": "5985635046bdea172fca9e88e1ab5360",
    "mode": "0644",
    "owner": "root",
    "secontext": "system_u:object_r:etc_t:s0",
    "size": 1646,
    "src": "/root/.ansible/tmp/ansible-tmp-1559805618.83-232960311871181/source",
    "state": "file",
    "uid": 0
}
10.0.1.196 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "checksum": "7307b7c59caac41831b97d4f30f252459c6fc94a",
    "dest": "/etc/ansible/now.ini",
    "gid": 0,
    "group": "root",
    "md5sum": "5985635046bdea172fca9e88e1ab5360",
    "mode": "0644",
    "owner": "root",
    "secontext": "system_u:object_r:etc_t:s0",
    "size": 1646,
    "src": "/root/.ansible/tmp/ansible-tmp-1559805618.78-223217589022111/source",
    "state": "file",
    "uid": 0
}
10.0.1.246 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "checksum": "7307b7c59caac41831b97d4f30f252459c6fc94a",
    "dest": "/etc/ansible/now.ini",
    "gid": 0,
    "group": "root",
    "md5sum": "5985635046bdea172fca9e88e1ab5360",
    "mode": "0644",
    "owner": "root",
    "secontext": "system_u:object_r:etc_t:s0",
    "size": 1646,
    "src": "/root/.ansible/tmp/ansible-tmp-1559805618.82-251509177541745/source",
    "state": "file",
    "uid": 0
}
10.0.2.233 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": true,
    "checksum": "7307b7c59caac41831b97d4f30f252459c6fc94a",
    "dest": "/etc/ansible/now.ini",
    "gid": 0,
    "group": "root",
    "md5sum": "5985635046bdea172fca9e88e1ab5360",
    "mode": "0644",
    "owner": "root",
    "secontext": "system_u:object_r:etc_t:s0",
    "size": 1646,
    "src": "/root/.ansible/tmp/ansible-tmp-1559805618.82-121130619867471/source",
    "state": "file",
    "uid": 0
}

**Step 3**
Login to Ansible Tower ULR and add a custom scripts under setting -> Inventory Scripts -> Add 
Name= "ServiceNow" 
Description= "ServiceNowCustomScripts"
Organization= "Default"

Add **now.py** in *Custom Script table

**Step 4**
create an inventory under Inventory -> ADD -> Inventory
Details-> Name="ServiceNowInventory" Description="blank" Organization="Default"
Insights Credentails="blank" Instance Group="blank"
Variable="blank"
Save it

Now click on source and add 

Name: "ServiceNowInventory" Description="blank" Source="CustomScript"
Cridential="Blank" CustomInventoryScript="ServiceNowCustomScripts"  Verbosity="1"
Update Options:
   OverWirte
   updateOnlaunch

**Step 5**
Now Add glbal variable on path settings -> Jobs ->Extra Environment Variables
{
"NOW_INI": "/etc/ansible/now.ini"
}

**Step6**

Sync the sevicenow inventroy


