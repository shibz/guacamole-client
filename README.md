guacamole-client fast-connect-mod
=================================
This is a fork of the guacamole-client project that greatly simplifies the configuration and use of Guacamole.  This 
removes the Guacamole login page and removes the requirement to pre-configure each and every RDP/VNC/SSH server in the
Guacamole configuration file.  Now users can specify the address that they want to connect to on-the-fly.

Purpose
-------
The purpose of this modification is to allow Guacamole to become fully self-service.  This mod removes all reliance on
a systems administrator to modify configuration files and gives the users the power to connect to specific IP addresses
or host names.

Authentication
--------------
Since I have removed the Guacamole login page, any user with access to the Guacamole web page can connect to any 
RDP/VNC/SSH server on your network.  This is insecure!  It is up to the Administrator to implement the authentication 
method.  For example, here is the configuration for an Apache reverse proxy that authenticates against Active 
Directory, although any Apache authentication will work.

```apache
<Location />
        AuthType Basic
        AuthName "Network Credentials Required"
        AuthBasicProvider ldap

        AuthLDAPURL "ldap://addc1.domain.company-name.com addc2.domain.company-name.com/DC=domain,DC=company-name,DC=com?sAMAccountName"
        AuthLDAPBindDN "CN=Guacamole,OU=SomeOU,DC=domain,DC=company-name,DC=com"
        AuthLDAPBindPassword ServiceAccountPasswordGoesHere
        require valid-user

        ProxyPass ajp://127.0.0.1:8009/guacamole/ max=20 flushpackets=on
        ProxyPassReverse ajp://127.0.0.1:8009/guacamole/
</Location>
```

Mod Author
----------
This fork of Guacamole-client was modified by Nathan Przybyszewski.  For the original Guacamole project, visit https://github.com/glyptodon/guacamole-client
