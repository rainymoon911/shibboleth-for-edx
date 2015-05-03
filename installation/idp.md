
apache and tomcat:

    apt-get install apache2
    a2enmod ssl
    a2enmod proxy_ajp

    sudo apt-get install tomcat6
    
[*https://wiki.shibboleth.net/confluence/display/SHIB2/IdPApacheTomcatPrepare configuration of apache and tomcat]
    
    
    
jdk(oracle jdk):

    sudo apt-get install python-software-properties
    sudo add-apt-repository ppa:webupd8team/java
    sudo apt-get update
    sudo apt-get install oracle-java7-installer
    
configure jdk

    update-alternatives --config java
    
    
idp:
    
    wget http://shibboleth.net/downloads/identity-provider/2.4.4/shibboleth-identityprovider-2.4.4-bin.zip
    unzip shibboleth-identityprovider-2.4.4-bin.zip
    cd shibboleth-identityprovider-2.4.4
    JAVA_HOME=/usr/lib/jvm/java-7-oracle ./install.sh
    chown -R tomcat6:tomcat6 /opt/shibboleth-idp
