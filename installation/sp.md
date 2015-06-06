installation and configuration of sp
======

1.installation

there is a shib mod on apache,just install the mod

    sudo apt-get install libapache2-mod-shib2
    a2enmod shib2
    
    vi /etc/hosts
    //add the following code
    127.0.0.1 sp.edx.org  sp
    idp-url   idp.edx.org idp
    
test sp by accessing sp.edx.org/Shibboleth.sso/Status

2.configuration

(if you have changed the default port 8080 to 80, all the idp.edx.org should change to idp.edx.org:8080)
most configuration of sp is in shibboleth2.xml

2.1

    vi shibboleth2.xml
    //modify the entity id of sp
    <ApplicationDefaults entityID="http://sp.edx.org/shibboleth"
                         REMOTE_USER="eppn persistent-id targeted-id">
                         
    //add sso
    <SSO entityID="http://idp.edx.org:8080/shibboleth"
                 discoveryProtocol="SAMLDS" discoveryURL="https://ds.example.org/DS/WAYF">
              SAML2 SAML1
    </SSO>
    
    //add session initiator
    <SessionInitiator type="Chaining" Location="/Login" isDefault="true" id="Intranet"
		relayState="cookie" entityID="http://idp.edx.org:8080/idp/shibboleth" forceAuthn="true">
	    <SessionInitiator type="SAML2" acsIndex="1" template="bindingTemplate.html"/>
	    <SessionInitiator type="Shib1" acsIndex="5"/>
	</SessionInitiator>

2.2
not like idp-metadata.xml,sp doesn't generate default sp-metadata.xml

to generate your own sp-metadata.xml

    shib-keygen -h sp.edx.org(move the sp-key.pem,sp-cert.pem to the dir of sp)
    
[about shib-keygen](http://manpages.ubuntu.com/manpages/lucid/man8/shib-keygen.8.html)

generate sp-metadata.xml

    shib-metagen -h sp.edx.org> /etc/shibboleth/sp-metadata.xml

configure idp-metadata

    scp idp-metadata usrname@sp-ip:/etc/shibboleth
    
    vi shibboleth2.xml
    //add the following code
    <MetadataProvider type="XML" file="idp-metadata.xml"/>
    
2.3 configure the attribute-map.xml

    vi attribute-map.xml
    //add the following code
    <Attribute name="urn:oid:2.5.4.3" id="cn"/>
    <Attribute name="urn:oid:0.9.2342.19200300.100.1.3" id="mail"/>
    
2.4 use shib to protect source

    vi /etc/apache2/httpd.conf
    //add the following code
    <Location /secure>
    AuthType shibboleth
    ShibRequireSession On
    require valid-user
    </Location>

    cd /var/www/html(depend on your document root of apache)
    mkdir secure
    vi test.php
    //add the following code
    <?php print_r($_SERVER) ?>
    
access sp.edx.org/secure,you should redirect to idp and to identify,if success,click the test.php ,then
you can see the cn and mail value
 
