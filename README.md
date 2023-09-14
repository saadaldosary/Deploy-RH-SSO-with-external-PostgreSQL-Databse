# Deply RedHat Single Sign-On 

RH-SSO has **two** main ways to be deployed.
1. On Bare Metal/VM 
2. On RedHat Openshift Platform. 
	- if you opt for OCP deployment there are two ways to deploy the SSO Server
		1. Using SSO Operator 
		2. Using SSO Application Template 

**RH-SSO Operation modes:** 
1. Standalone: Single SSO instance 
2. Standalone cluster: Multiple SSO instances or more and managed independently. i.e it's possible to have multiple nodes but with different versions and configurations. This mode does not ensure replication since it is managed separately. Also, it is suitable in case of multiple applications have different compatibility requirements.
3. Domain clustered: Multiple SSO Instances managed by one single master SSO instance. When changes occur in the master node, it will be replicated to other nodes, thus ensuring the same configurations are being applied. 
4. Cross-Site Replication: Used to run RH-SSO in a cluster across multiple data centers, where each data center has its own cluster of servers. e.g. two geographically different sites run their own cluster of RH-SSO with replication. suitable cases: DR sites or HA Sites. 

**RH-SSO Authentication Methods:** 
1. Username/Password
2. 2FA e.g. OTP
3. Web Authentication: w3c standard.
4. Kerberos 
5. X.509 Certificate validation. 

**RH-SSO Web-Console types:**
1. Account console.
2. Admin Console.  

**RH-SSO Subsystems:**
1. Datasources: connection configuration to a pool of databases. 
2. Infinispan: Provide cache support for JBoss EAP to provide **local** cache which is stored locally, and **temporary** cache which handles user session, tokens, etc. 
3. Keycloak Server
4. Undertow: webserver and server container. 
