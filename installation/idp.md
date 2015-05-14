
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

our system is internal,so i don't use tsl/ssl(change https to http in configuration file)

3.1 install idp

[download source](http://shibboleth.net/downloads/identity-provider/)
    
    download the source
    unzip shibboleth-identityprovider-2.4.4-bin.zip
    cd shibboleth-identityprovider-2.4.4
    JAVA_HOME=/usr/lib/jvm/java-7-oracle ./install.sh
    chown -R tomcat6:tomcat6 /opt/shibboleth-idp

test idp by accessing localhost:8080//idp/profile/Status

the default port for idp is 8080,if you don't change the port,you may need to change all the idp.edx.org to idp.edx.org:8080

the files you need to change are relying-party.xml,idp-metadata.xml(and the configuration of sp )

3.2 configure metadata from sp(about how to generate sp-metadata,refer to the configuration of sp)

(the name of sp-metadata on idp server must be the same of the metadata on sp server)

    scp sp-metadata.xml username@idp-ip:/opt/shibboleth-idp/metadata
    
    vi /conf/relying-party.xml
    //add the following code
    <metadata:MetadataProvider xsi:type="FilesystemMetadataProvider"
		xmlns="urn:mace:shibboleth:2.0:metadata" id="SPMETADATA"
		metadataFile="/opt/shibboleth-idp/metadata/sp-metadata.xml" />
		
3.3 configure ldap for authentication

firetly use the /ldap/test_ldap.py to ensure the ldap work corectly

	vi handler.xml
	//add the following code
	<!--  Username/password login handler -->
	<ph:LoginHandler xsi:type="ph:UsernamePassword"
		jaasConfigurationLocation="file:///opt/shibboleth-idp/conf/login.config">
	<ph:AuthenticationMethod>urn:oasis:names:tc:SAML:2.0:ac:classes:
		PasswordProtectedTransport</ph:AuthenticationMethod>
	</ph:LoginHandler></code>
		
	vi attribute-resolver.xml
	//add the following code
	<resolver:DataConnector id="myLDAP" xsi:type="dc:LDAPDirectory"
        ldapURL="ldap://your ldap url" 
        baseDN="ou=Users,dc=openedx,dc=com" 
        principal="cn=admin,dc=openedx,dc=com"
        principalCredential="password">
        <dc:FilterTemplate>
            <![CDATA[
                (uid=$requestContext.principalName)
            ]]>
        </dc:FilterTemplate>
    	</resolver:DataConnector>
    	
    	vi attrbute-filter.xml
    	//add the following code
    	<afp:AttributeRule attributeID="email">
	    <afp:PermitValueRule xsi:type="basic:ANY" />
    	</afp:AttributeRule>

	<afp:AttributeRule attributeID="commonName">
	    <afp:PermitValueRule xsi:type="basic:ANY" />
	    
change the attribute you want to release and the policy of that,but the attribute must correspond to 
the attrbute-map.xml on sp server

