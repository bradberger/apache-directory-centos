To create new users on the Apache Directory server, using a
graphical interface, you can use [this tutorial](https://access.redhat.com/documentation/en-US/Fuse_ESB/4.3.1/html/Apache_ActiveMQ_Security_Guide/files/LDAP-AddUserEntries.html)
courtesy of RedHat.

After creating the users, you'll need to assign specific ActiveMQ topic
permissions. There are more details  [here](https://access.redhat.com/documentation/en-US/Fuse_ESB_Enterprise/7.1/html/ActiveMQ_Security_Guide/files/Auth-JAAS-LDAPAuthentPlugin.html)
that may apply depending on usage scenarios.

The `userBase` specified in the `/opt/activemq/conf/login.config` configuration
file specifies the particular subtree of the DIT (directory information tree)
to search for user entries.

To determine which roles a user has based on the DIT, you can use:

- the `userRoleName` attribute
- a combination of the role options (`roleBase`, `roleSearchMatching`, `roleSearchSubtree` and `roleName`)
- both of the above

For example, by setting the `userBase` to `ou=User,ou=ActiveMQ,ou=system`, the
search for user entries is restricted to the subtree beneath that node.
