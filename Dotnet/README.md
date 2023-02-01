## Install Dotnet on Ubuntu & Centos

### Ubuntu
* Dotnet manual steps for ubuntu [Refer Here](https://learn.microsoft.com/en-us/dotnet/core/install/linux-ubuntu#install-the-sdk)
```
sudo apt-get update
sudo apt-get install -y dotnet-sdk-6.0
sudo apt-get install -y aspnetcore-runtime-6.0
sudo apt-get install -y dotnet-runtime-6.0
```
* If manual steps are working properly write a playbook.
* For writing playbook for these steps.
* Based on manual steps which module is suitable take that module.
```
---
- name: inatall dotnet
  hosts: all
  become: yes
  vars:
    dotnet_packages: 
      - dotnet-sdk-6.0
      - aspnetcore-runtime-6.0
      - dotnet-runtime-6.0
  tasks: 
    - name: install dotnet
      apt: 
        name: "{{ dotnet_packages }}"
        update_cache: yes
        state: present
```

### Centos
* Dotnet manual steps for centos [Refer Here](https://learn.microsoft.com/en-us/dotnet/core/install/linux-centos#centos-7)
```
sudo rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm
sudo yum install dotnet-sdk-7.0
sudo yum install aspnetcore-runtime-7.0
sudo yum install dotnet-runtime-7.0
```
* If manual steps are working properly write a playbook.
* For writing playbook for these steps.
* Based on manual steps which module is suitable take that module.
```
---
- name: inatall dotnet
  hosts: all
  become: yes
  vars:
    packages_url: https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm
    packages_dest: /tmp/packages-microsoft-prod.rpm
    dotnet_packages: 
      - dotnet-sdk-7.0
      - aspnetcore-runtime-7.0
      - dotnet-runtime-7.0
  tasks:
    - name: get url to /tmp/
      ansible.builtin.get_url:
        url: "{{ packages_url }}"
        dest: "{{ packages_dest }}"
    - name: packages-microsoft-prod
      ansible.builtin.yum:
        name: "{{ packages_dest }}"
        state: present
    - name: install dotnet
      ansible.builtin.yum: 
        name: "{{ dotnet_packages }}"
        state: present
```

* After executing both playbook combine as one.
```
---
- name: inatall dotnet
  hosts: all
  become: yes
  tasks:
    - name: get url to /tmp/
      ansible.builtin.get_url:
        url: "{{ packages_url }}"
        dest: "{{ packages_dest }}"
      when: ansible_facts['distribution'] == "CentOS"
    - name: packages-microsoft-prod
      ansible.builtin.yum:
        name: "{{ packages_dest }}"
        state: present
      when: ansible_facts['distribution'] == "CentOS"
    - name: install dotnet
      ansible.builtin.yum: 
        name: "{{ dotnet_packages }}"
        state: present
```

* After complication this playbook making role.
* For making a role we skeleton of role structure.
* For skeleton of role we need ansible control node for creating role structure.
```
ansible-galaxy role init <role_name>
```
* For get in to our physical machine we need sftp command
```
sftp username@publicip
sftp> get -r <role_name>
```
* After getting in to physical machine you have to breaking a playbook into multiple files of roles folder.
* After breaking playbook we need to create a yaml file and hosts for running the role.
* After than this all things are push in to git repositories.
* After than git clone into ansible control node and run a role.
