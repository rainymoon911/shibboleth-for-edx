here are some useful infos that maybe helpful to debug
======

1.[official troubleshooting](https://wiki.shibboleth.net/confluence/display/SHIB2/NativeSPTroubleshootingCommonErrors#NativeSPTroubleshootingCommonErrors-Unabletolocatemetadataforidentityprovider(https://identities.supervillain.edu/idp/shibboleth).)

2.use [AACLI](https://wiki.shibboleth.net/confluence/display/SHIB2/AACLI) to test idp dependently
  
if you have configure ldap on idp correctly,the following command should return the user infos from ldap

    ./aacli.sh --configDir=/opt/shibboleth-idp/conf/ --principal=zyu
    
3.if you just have a idp or sp,use [testshib](http://www.testshib.org/) to test

4.log file

sp:/var/log/shibd.log

idp:/opt/shibboleth-idp/logs/idp-process.log
    

    
