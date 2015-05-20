configuration of openedx
======

first, read [official document ](https://github.com/edx/configuration/wiki/Setting-Up-External-Authentication)

1.the roles shibboleth and apache will do most of the work,surely ,you can do it manually(refer to tasks in role)

the dir of role is /edx/etc/playbooks/role

just fill the main.yml(very important!, this will determine the initial configuration files) and run the following command(refer to configuration of sp)

    ansible-playbook ./run_role.yml -i "localhost," -c local -e role=shibboleth
    
if there are some errors abour undefined var in apache/templates/lms (i treat it as a bug),you should replace the var manually
(the configuration of var is in edxapp role)

2.enable shib for edx-platform

    vi /edxapp/lms/envs/common.py
    set 'AUTH_USE_SHIB','SHIB_DISABLE_TOS','RESTRICT_ENROLL_BY_REG_METHOD' true
    
3.set Advanced Option 'External Login Domain' to "shib:your idp url" in cms

4.click your course in lms,and click the 'sign' button,then it will redirect to youtidp.
(click the sign button on home page will not trigger shib-login)


