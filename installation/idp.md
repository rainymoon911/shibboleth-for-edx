
1.apache and tomcat:

    apt-get install apache2
    a2enmod ssl
    a2enmod proxy_ajp

    sudo apt-get install tomcat6
    
[configuration of apache and tomcat](https://wiki.shibboleth.net/confluence/display/SHIB2/IdPApacheTomcatPrepare) 
    
    vi /etc/hosts
    //add the following code
    127.0.0.1 idp.edx.org sp
    
    vi /tomcat6/apache2.conf
    //add the following code
    ServerName idp.edx.org
    
2.jdk(oracle jdk--officially recommended):

    sudo apt-get install python-software-properties
    sudo add-apt-repository ppa:webupd8team/java
    sudo apt-get update
    sudo apt-get install oracle-java7-installer
    
configure jdk

    update-alternatives --config java
    
    
3.idp:

3.1 install idp

[download source](http://shibboleth.net/downloads/identity-provider/)
    
    download the source
    unzip shibboleth-identityprovider-2.4.4-bin.zip
    cd shibboleth-identityprovider-2.4.4
    JAVA_HOME=/usr/lib/jvm/java-7-oracle ./install.sh
    chown -R tomcat6:tomcat6 /opt/shibboleth-idp
    
3.2 configure metadata from sp(about how to generate sp-metadata,refer to the configuration of sp)

(the name of sp-metadata on idp server must be the same of the metadata on sp server)

    scp sp-metadata.xml username@idp-ip:/opt/shibboleth-idp/metadata
    
    vi /conf/relying-party.xml
    //add the following code
    <metadata:MetadataProvider xsi:type="FilesystemMetadataProvider"
		xmlns="urn:mace:shibboleth:2.0:metadata" id="SPMETADATA"
		metadataFile="/opt/shibboleth-idp/metadata/sp-metadata.xml" />
		


