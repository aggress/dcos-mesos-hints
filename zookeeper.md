## Zookeeper

### Backup ZK data

Use a customized Guano to be able to authenticate with Mesosphere ZK

1. `git clone git@github.com:adyatlov/guano.git`
2. `cd guano`
3. `mvn package`
4. `cd target`
5. `mkdir dump`
6. `cat /opt/mesosphere/etc/zk_super_credentials` remember this tuple for the next line
7. `java -jar guano.jar -s host:2181 -u <user> -p <password> -d "/" -o "dump"`