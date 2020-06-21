Temp folder generation and cleanup
=========

This Role will help you to manage your temp file properly.

When running ansible playbooks, it may generate a temp files during the processing. 
this role will help you manage those temp files and perform the cleanup for you.

Requirements
------------

No requirements required.

Role Variables
--------------

+ "Created localhost temp folder with path '{{ TEMP_FOLDER_LOCAL }}'"
+ "Created linux temp folder with path '{{ TEMP_FOLDER_LINUX }}'"
+ "Created windows temp folder with path '{{ TEMP_FOLDER_WINDOWS }}'"

Dependencies
------------

No dependencies required.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

STEP01: For ansible, define Inventory "servers", if is ansible tower or ansible awx, define the inventory and credential accordingly

```yaml
[lo]
127.0.0.1 ansible_user=root ansible_password=password ansible_port=22

[servers]
10.68.33.104 ansible_ssh_user="admin" ansible_ssh_pass="password" ansible_ssh_port=22
10.68.33.107 ansible_ssh_user="admin" ansible_ssh_pass="password" ansible_ssh_port=5985 ansible_connection="winrm" ansible_winrm_server_cert_validation=ignore
```

STEP02: Install role from ansible-galaxy
First is to defin the current path
```yaml
[Alick@AWX ~]# pwd
/root
```
Then use following command to install roledevnetsg.temp_folder_generation_and_cleanup
```yaml
[Alick@AWX ~]# ansible-galaxy install devnetsg.temp_folder_generation_and_cleanup --roles-path /root
```

STEP03: Creat main.yml to invoke the role to run this playbook.

Please take note this role does not need gather_facts to be enable.
but in this example if need to display facts through jinja2, we have enabled facts to print out the output for your info.
```yaml
---
- name: Testing role to perform temp folder to generation and cleanup
  hosts: servers
  gather_facts: yes
  roles:
    - devnetsg.temp_folder_generation_and_cleanup
```
STEP04: Define initial variable in temp_folder_generation_and_cleanup/vars/main.yml and edit the value as you like.
```yaml
---
folder_prefix: temp-devnetsg
```

Example Playbook Output
-----------------------
```yaml
[Alick@AWX ~]# ansible-playbook main.yml

PLAY [servers] ***********************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************
ok: [10.68.33.104]
ok: [10.68.33.107]

TASK [temp_folder_generation_and_cleanup : Test connectivity of linux hosts] *********************************************************************************
ok: [10.68.33.104]
fatal: [10.68.33.107]: FAILED! ...ignoring

TASK [temp_folder_generation_and_cleanup : Test connectivity of windows hosts] *******************************************************************************
fatal: [10.68.33.104]: FAILED! ...ignoring
ok: [10.68.33.107]

TASK [temp_folder_generation_and_cleanup : Temp folder generation for localhost] *****************************************************************************
included: /root/temp_folder_generation_and_cleanup/tasks/temp_folder_generation/temp_folder_generation_for_localhost.yml for 10.68.33.107, 10.68.33.104

TASK [temp_folder_generation_and_cleanup : Temp folder generation tasks -  generate temp folder path for localhost] ******************************************
ok: [10.68.33.107 -> localhost] => { "msg": "/tmp/temp-devnetsg-571c3e4f-26f8-5d50-8fb2-01079ca9e7f5" }

TASK [temp_folder_generation_and_cleanup : Temp folder generation tasks -  generate temp folder for localhost] ***********************************************
changed: [10.68.33.107 -> localhost]

TASK [temp_folder_generation_and_cleanup : Temp folder generation tasks -  generate variable for localhost] **************************************************
ok: [10.68.33.107]
ok: [10.68.33.104]

TASK [temp_folder_generation_and_cleanup : Temp folder generation for linux/unix] ****************************************************************************
skipping: [10.68.33.107]
included: /root/temp_folder_generation_and_cleanup/tasks/temp_folder_generation/temp_folder_generation_for_linux.yml for 10.68.33.104

TASK [temp_folder_generation_and_cleanup : Temp folder generation tasks -  generate temp folder path for linux] **********************************************
ok: [10.68.33.104] => { "msg": "/tmp/temp-devnetsg-1d244512-e9e2-5512-902d-b32e5116e814" }

TASK [temp_folder_generation_and_cleanup : Temp folder generation tasks -  generate temp folder for linux] ***************************************************
changed: [10.68.33.104]

TASK [temp_folder_generation_and_cleanup : Temp folder generation tasks -  generate variable for linux] ******************************************************
ok: [10.68.33.104]

TASK [temp_folder_generation_and_cleanup : Temp folder generation for windows] *******************************************************************************
skipping: [10.68.33.104]
included: /root/temp_folder_generation_and_cleanup/tasks/temp_folder_generation/temp_folder_generation_for_windows.yml for 10.68.33.107

TASK [temp_folder_generation_and_cleanup : Temp folder generation tasks -  generate temp folder path for windows] ********************************************
ok: [10.68.33.107] => { "msg": "C:\\temp-devnetsg-dc81c30a-5d02-5829-a8d1-67fc68da1a8f" }

TASK [temp_folder_generation_and_cleanup : Temp folder generation tasks -  generate temp folder for windows] *************************************************
changed: [10.68.33.107]

TASK [temp_folder_generation_and_cleanup : Temp folder generation tasks -  generate variable for windows] ****************************************************
ok: [10.68.33.107]

TASK [temp_folder_generation_and_cleanup : Generate data output] *********************************************************************************************
included: /root/temp_folder_generation_and_cleanup/tasks/generate_data_output/generate_data_output.yml for 10.68.33.107, 10.68.33.104

TASK [temp_folder_generation_and_cleanup : Generate data output - create a jinja2 template to use variable] **************************************************
changed: [10.68.33.107 -> localhost]

TASK [temp_folder_generation_and_cleanup : Generate data output - convert yml to variable] *******************************************************************
ok: [10.68.33.107 -> localhost]

TASK [temp_folder_generation_and_cleanup : Temp folder creation - display on screen] *************************************************************************
ok: [10.68.33.107 -> localhost] => {
    "msg": {
        "data": [
            {
                "machine_name": "WIN-5313RG98JM1", 
                "machine_temp_folder_path": "C:\\temp-devnetsg-dc81c30a-5d02-5829-a8d1-67fc68da1a8f", 
                "machine_type": "windows"
            }, 
            {
                "machine_name": "Al-TERAFORM-CENTOS-2", 
                "machine_temp_folder_path": "/tmp/temp-devnetsg-1d244512-e9e2-5512-902d-b32e5116e814", 
                "machine_type": "linux"
            }
        ]
    }
}

TASK [temp_folder_generation_and_cleanup : Temp folder cleanup for localhost] ********************************************************************************
included: /root/temp_folder_generation_and_cleanup/tasks/temp_folder_cleanup/temp_folder_cleanup_for_localhost.yml for 10.68.33.107, 10.68.33.104

TASK [temp_folder_generation_and_cleanup : Temp folder cleanup tasks -  cleanup temp folder for localhost] ***************************************************
changed: [10.68.33.107 -> localhost]

TASK [temp_folder_generation_and_cleanup : Temp folder cleanup for linux/unix] *******************************************************************************
skipping: [10.68.33.107]
included: /root/temp_folder_generation_and_cleanup/tasks/temp_folder_cleanup/temp_folder_cleanup_for_linux.yml for 10.68.33.104

TASK [temp_folder_generation_and_cleanup : Temp folder cleanup tasks -  cleanup temp folder for linux/unix] **************************************************
changed: [10.68.33.104]

TASK [temp_folder_generation_and_cleanup : Temp folder cleanup for windows] **********************************************************************************
skipping: [10.68.33.104]
included: /root/temp_folder_generation_and_cleanup/tasks/temp_folder_cleanup/temp_folder_cleanup_for_windows.yml for 10.68.33.107

TASK [temp_folder_generation_and_cleanup : Temp folder cleanup tasks -  cleanup temp folder for windows] *****************************************************
changed: [10.68.33.107]

PLAY RECAP ***************************************************************************************************************************************************
10.68.33.104               : ok=13   changed=2    unreachable=0    failed=0    skipped=2    rescued=0    ignored=1   
10.68.33.107               : ok=19   changed=5    unreachable=0    failed=0    skipped=2    rescued=0    ignored=1  
```

License
-------

BSD

Author Information
------------------

Created by Alick for fun.
