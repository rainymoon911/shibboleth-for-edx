[openldap on turkey](http://www.turnkeylinux.org/openldap)
======

1. the image has installed phpldapadmin,use this tool to manage openldap
2. test_ldap.py can help you test whether the openldap work corectly(modify the ip ,base DN and filter)
3. if there is no eduPerson schema on your ldap,eduPerson.ldif can help you,
   Put this file to /etc/ldap/schema/ and then execute the following comand to add this schema:

         ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/ldap/schema/eduperson.ldif
         
4. use createUser.ldif to create user.

         ldapadd -x -D “cn=admin,dc=openedx,dc=com” -W -f create_user.ldif

