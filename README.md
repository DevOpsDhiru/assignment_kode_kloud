- Author's Name: Dhirendra Thapaliya                  <br>
- Author's e-mail: dev.ops.dhiru@gmail.com            <br>
- Author's git: https://github.com/DevOpsDhiru/       <br>

# Name: Simple application deployment (Load Balancer environment) using ansible

# This is one of the assignments of KodeKloud

This repository contains the solution of the final assignment of kode_kloud of "Advance Ansible by Mumshad Manambeth" <br>
<br>

* Used modules: <br>
    |==================|=============================|==================================|<br>
    |"- yum"...........|..........."- pip"...........|..........."- set_fact"...........|<br>
    |"- template"......|..........."- systemd".......|..........."- service"............|<br>
    |"- yum_repository"|..........."- git"...........|..........."- copy"...............|<br>
    |"- shell".........|..........."- mysql_db"......|..........."- mysql_user".........|<br>
    |==================|=============================|==================================|<br>
    
* Used parameters (other than default): <br>

    |==================|=============================|==================================|<br>
    |"- remote_src"....|..........."- when"..........|..........."- run_once"...........|<br>
    |"- delegate_to"...|..........."- backup"........|..........."- recurse"............|<br>
    |"- update"........|.............................|..................................|<br>
    |==================|=============================|==================================|<br>
<br>    

# Condition / Environment and Assumptions
<br>

*   We are not dealing with selinux, we just put them in permissive or disabled mode before running this playbook. (not included in this playbook)
*   We are not dealing with firewall or iptables. If there is any, we assume that this communication is allowed. 
    If we want to use IPTABLES, please use my IPTABLES playbook on git.

*   Ansible config file i.e **ansible.cfg** is kept in the main play directory which makes **"hosts"** file in the same level as **default inventory.**
*   We are not dealing with TLS or SSL communication. This is test playbook, works in port 80 only.
    If we want to update this to TLS or SSL, please use my Self-Signed-Certificate playbook on git which is public and then change the template files accordingly.
<br>    

*   Inventory hostfile should be defined as "lb" and "node". It is not namdatory, however, is hardcoded in the playbook. 
    Should we require to change it, we should considering the words in the following files and locations: <br>
        **(1)**      inventory hostfile (normally hosts in the main playbook.) : [lb]  and [node] <br>
        **(2)**      main file of the role "python_and_dependencies";  relative path = roles/python_and_dependencies/tasks/main.yml; line#: 10 and 18 <br>
        **(3)**      main file of the role "applications"; relative path =  roles/applications/tasks/main.yml; line#: 10, 19, 26, 35 and 40 <br>
        **(4)**      main file of the role "nginx"; relative path =  roles/nginx/tasks/main.yml; line#: 17, 19, and 20 <br>
        **(4)**      main file of the role "database"; relative path =  roles/applications/tasks/main.yml; line#: 6, 13, 19, and 28 <br>
<br>        

Therefore we could change the group names in the inventory. But if we change, we need to define those names in the group_vars with the respective file as group names such as lb.yml and node.yml. It is recommended to copy the same files and modify the variables per your requirement. <br>
<br>

* The environment has Load Balancer and Node:  <br>
    where: one LB and two Nodes. <br>
           But the playbook is dynamic and we can add as many nodes as we want, but requires few updates in : <br>
           # "UPDATING PLAYBOOK" <br>
           **(1)** the lb templates i.e. (lb.conf.j2) where we have to define upstream servers. Copy line#6 and paste it just below it and change it to node3, node4 etc<br>
           **(2)** After using that variables in the templates, we should define that variable in the playbook. <br>
           #           I am not defining this variables in the group_vars because the hostnames should be taken from "node" section and used in the "lb" section.. <br>
           #           therefore want to be sure that those variables are available for all group; therefore I am using set_fact inside playbook. <br>
           # <br>
           #           Opem main.yml from nginx role. Relative path should be "roles/nginx/tasks/main.yml" <br>
           #               copy and paste line#20 just below it and change it to node2, node4 etc... as per your requirement.  <br>
           #               We should keep in mind that we are defining the variables that we have used in step #1 of this "UPDATING PLAYBOOK" section. <br>

# How to run?: 

* Define Variables in node.yml file of "group_vars" directory
* Define inventory hostname. Recommended to use "hosts" file in the same directory.
* Run the ansible playbook "ansible-playbook" assignment.yml -K"    # Here if we wish to  give the inventory hostname we can use -i <inventoryFile>
*
* and test the application by browsing http://<ip_of_lb>
*

# limitation: 
* mysql database creation task is giving error on centos 7 - expecting to get this bug fixed asap by ansible.