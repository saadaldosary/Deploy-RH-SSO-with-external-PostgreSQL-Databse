# Deploy-RH-SSO-Openshift-Operator-with-external-PostgreSQL-Databse 
#### Prerequisites:
 - [ ] Installed Openshift cluster up and running 
 - [ ] External Postgres database with the following configuration:
	* *keycloak* database created.
	* Database is not bound to localhost.
	* Database is configured to allow connection to keycloak database. 
	* Host firewall set to allow incoming traffic. 

### Procedure: 
 1. login to Openshift UI as cluster admin
 2. Create a new project named sso 
 3. Form the sidebar,  select operators > operatorHub 
 4. Search for **Red Hat Single Sign-On Operator** and click install. This may take a few minutes to complete 
 5. From the sidebar click on Installed Operators and select Red Hat Single Sign-On Operator.
 6. Create an instance of Keycloak with the name keycloak. This may take a few minutes to complete.
 7.  Create a file named **keycloak-db-secret**  and past the following: 

```
kind: Secret
apiVersion: v1
metadata:
  name: keycloak-db-secret
  namespace: sso
stringData:
  POSTGRES_DATABASE: keycloak
  POSTGRES_EXTERNAL_ADDRESS: <Databse_IP>
  POSTGRES_EXTERNAL_PORT: '5432'
  POSTGRES_PASSWORD: postgres
  POSTGRES_USERNAME: postgres
type: Opaque
```
8. Replace the existing secret with the new one: 
``` 
oc replace -f keycloak-db-secret.yaml 
```
9. Edit the Keycloak custom resource definition
```
oc edit keycloak/keycloak 
```
10. Add the following lines to the resource: 
```
externalDatabase:
     enabled: true
```
11. the keycloak will automatically connect to the databse, in case you want to speed it up, delete the existing one.
```
oc delete keycloak-0
```
