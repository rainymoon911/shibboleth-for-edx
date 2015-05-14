installation and configuration of sp
======

1.installation

there is a shib mod on apache,just install the mod

    sudo apt-get install libapache2-mod-shib2
    
    vi /etc/hosts
    //add the following code
    127.0.0.1 sp.edx.org  sp
    idp-url   idp.edx.org idp
    
test sp by accessing sp.edx.org/Shibboleth.sso/Status
