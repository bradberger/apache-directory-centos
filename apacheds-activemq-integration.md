This will cover configuration of LDAP authentication in the broker.

## 1. Create the login configuration file

Create the login configuration file. Using a text editor, create the file
`login.config` under the directory `$ACTIVEMQ_HOME/conf`, which is in this
case `/opt/activemq/conf`. Make sure to replace the `connectionURL`,
`connectionUsername`, and `connectionPassword` with the appropriate values:

```java
LDAPLogin {
    org.apache.activemq.jaas.LDAPLoginModule required
        debug=true
        initialContextFactory=com.sun.jndi.ldap.LdapCtxFactory
        connectionURL="ldap://localhost:10389"
        connectionUsername="uid=admin,ou=system"
        connectionPassword=secret
        connectionProtocol=""
        authentication=simple
        userBase="ou=users,ou=system"
        userSearchMatching="(uid={0})"
        userSearchSubtree=false
        roleSearchMatching="(uid={1})"
        ;
};
```

## 2. Add the LDAP authentication plug-in to the broker configuration

To Add the LDAP authentication plug-in to the broker configuration, open the
broker configuration file, `$ACTIVEMQ_HOME/conf/activemq.xml`, with a text
editor and add the `jaasAuthenticationPlugin` element, as follows:

```xml
<beans>
  <broker ...>
    ...
    <plugins>
      <jaasAuthenticationPlugin configuration="LDAPLogin" />
    </plugins>
    ...
  </broker>
</beans>
```

The value of the configuration attribute, `LDAPLogin`, references the login
entry from the `login.config` file you just created.

## 3. Comment out the mediation router elements in the broker confiuration

If this section exists in the broker configuration file at
`$ACTIVEMQ_HOME/conf/activemq.xml` then comment it out. This is because Camel
route is not used in the current tutorial. If you left it enabled, you would
have to supply it with appropriate username/password credentials, because it
acts as a broker client.

```xml
<beans>
  <broker ...>
    ...
  </broker>

  <!--
  <camelContext>
    ...
  </camelContext>
  -->
  ...
</beans>
```
