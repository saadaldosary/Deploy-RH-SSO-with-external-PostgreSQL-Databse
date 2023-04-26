# RH-SSO Baremetal/VM Deployment 

#### RH-SSO Server 
1. Download Zip Compressed file from: 
`https://access.redhat.com/jbossnetwork/restricted/listSoftware.html?downloadType=distributions&product=core.service.rhsso`
2. Unzip the downloaded file:
 `unzip <package_name> -d /opt/`
3.  Change the ownership as applicable:
 `chown -R /opt/rh-sso-7.6`
4. Add admin user: 
`bin/add-user-keycloak.sh -u <username> -p <password>`
5. To Start SSO instance: 
`cd /opt/rh-sso-7.6; bin/standalone.sh` 

### Connect To External Postgresql Database
#### Postgresql-Server
##### Prerequisites:
1. A running Postgresql database server.

##### Procedure: 
1. In the Postgresql server: 
```
    sudo -i 
    su postgres
    psql 
    create database keycloakdb;
    create user rhsso with superuser;
    alter user rhsso with password 'password';
    grant all on database keycloakdb TO rhsso;
    \q 
    exit
```
 
     
2. Allow external access to keycloakdb: 
```
    vim /var/lib/postgresql/data/pg_hba.conf
    #At the bottom of the page, add external connection to keycloakdb 
    host    keycloakdb             rhsso             < SSO-Server-IP >/32            md5
    save and exit
    systemctl restart postgresql-server.service 
```

#### RH-SSO Server 
**Postgres Module** 
 1. Make new directory as follow: 
 `mkdir -p /opt/rh-sso-7.6/modules/system/layers/keycloak/org/postgresql/main`
2.  Download Postgresql jdbc jar file from here: 
`https://jdbc.postgresql.org/download/` 
3. Copy the downloaded file to main folder: 
`cp postgresql-XX.X.X.jar /opt/rh-sso-7.6/modules/system/layers/keycloak/org/postgresql/main/`
4. Make sure permissions set correctly and ownership of the file to the right user. 
5. Create module file and add the following:
`vim /opt/rh-sso-7.6/modules/system/layers/keycloak/org/postgresql/main/module.xml`
```
<?xml version="1.0" encoding="UTF-8"?>
<module xmlns="urn:jboss:module:1.3" name="org.postgres">
		<resources>
			<resource-root path="<JAR FILE NAME>"/>
		</resources>
		<dependencies>
			<module name="javax.api"/>
			<module name="javax.transaction.api"/>
		</dependencies>
</module>
```


**Add Postgres Module and Driver**
1. Find the sso-extension.cli script in this repo
2. Copy and Paste sso-extension.cli script to your SSO instance:
`vim /opt/rh-sso-7.6/bin/sso-extension.cli`
3. Fill up the script file according to your setup. 
4. Run sso-extension.cli:
`bin/jboss-cli.sh --file=/opt/rh-sso-7.6/bin/sso-extensions.cli` 


#### Run RH-SSO As systemd Service: 
1. Edit the given template and uncomment/add configuration accordingly: 
`vim bin/init.d/jboss-eap.conf`

```JBOSS_HOME="/opt/rh-sso-7.6" 
JBOSS_USER= < username > 
JBOSS_MODE= < operating mode > 
JBOSS_CONSOLE_LOG="/var/log/jboss-eap/console.log"
JBOSS_OPTS="-b 0.0.0.0"
```

3. Copy the template to /etc/default: 
`sudo cp /opt/rh-sso-7.6/bin/init.d/jboss-eap.conf /etc/default`
4. Apply correct execution permissions:
`sudo chmod 774 /etc/init.d/jboss-eap-rhel.sh`
5. Apply Selinux correct tag:
`sudo restorecon /etc/init.d/jboss-eap-rhel.sh`
6. Configure systemd:
```
sudo chkconfig --add jboss-eap-rhel.sh
sudo systemctl daemon-reload 
sudo service jboss-eap-rhel start
sudo chkconfig jboss-eap-rhel.sh on
```
##### Access the web console on: localhost:8443
