Name: Dhirendra Thapaliya
Email: dev.ops.dhiru@gmail.com
Git: https://github.com/DevOpsDhiru/assignment_kode_kloud.git


# assignment of KodeKloud
This repository contains the solution of the final assignment of kode_kloud of "Advance Ansible by Mumshad Manambeth" 

Used modules:
    - yum
    - pip
    - set_fact
    - template
    - systemd
    - service
    - yum_repository
    - git
    - copy
    - shell
    - mysql_db
    - mysql_user

parameters (other than default):
    - remote_src
    - when
    - run_once
    - delegate_to
    - backup
    - recurse
    - update

# Condition / Environment and Assumptions
*   We are not dealing with selinux, we just put them in permissive mode.
*   We are not dealing with firewall or iptables. If there is any, we assume that this communication is allowed.
*   We are not dealing with TLS or SSL communication. This is test playbook, works in port 80 only.
    If we want to update this to TLS or SSL, please use my playbook at Self-Signed-Certificate on git which is public. 
    
*   Inventory hostfile should be defined as "lb" and "node"; however, is not namdatory. Because, "group name" - 'lb' and 'node' are not hardcoded in the playbook. 
    It is used as   variable i.e. {{lb}} and {{node}}

Therefore we could change the group names in the inventory. But if we change, we need to define those names in the group_vars with the respective file as group names such as lb.yml and node.yml. It is recommended to copy the same files and modify the variables per your requirement.


* The environment has Load Balancer and Node: 
    where: one LB and two Nodes.
           But the playbook is dynamic and we can add as many nodes as we want, but requires few updates in :
           # "UPDATING PLAYBOOK"
                1) the lb templates i.e. (lb.conf.j2) where we have to define upstream servers. copy line#6 and paste it just below it and change it to node3, node4 etc.
                2) After using that variables in the templates, we should define that variable in the playbook.
                #           I am not defining this variables in the group_vars because the hostnames should be taken from "node" section and used in the "lb" section..
                #           therefore want to be sure that those variables are available for all group; therefore I am using set_fact inside playbook.
                #
                #           Opem main.yml from nginx role. Relative path should be "roles/nginx/tasks/main.yml"
                #               copy and paste line#20 just below it and change it to node2, node4 etc... as per your requirement. 
                #               We should keep in mind that we are defining the variables that we have used in step #1 of this "UPDATING PLAYBOOK" section.

