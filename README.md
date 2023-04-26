# Deply RedHat Single Sign-On 

RH-SSO has **two** main way to be deployed.
1. On Bare Metal/VM 
2. On RedHat Openshift Platform. 
	- if you opt for OCP deployment there are two ways to deploy SSO Server
		1. Using SSO Operator 
		2. Using SSO Application Template 

**RH-SSO Operation modes:** 
	1. Standalone: Single SSO instance 
	2. Standalone cluster: Multiple SSO instances or more and managed independantly.
	3. Domain clusterd: Multiple SSO Instances managed by one single master SSO instance  
	4. Cross-Site Replication: Used to run RH-SSO in a cluster across multiple data centers, where each data center has its own cluster of servers. 

**RH-SSO Authintication Methods:** 
1. Username/Password
2. 2FA e.g. OTP
3. Web Authintication: w3c standard.
4. Kerberos 
5. X.509 Certificate validation. 

**RH-SSO Web-Console types:**
1. Account console.
2. Admin Console.  

**RH-SSO Subsystems:**
1. Datasources: connection configuration to a pool of databases. 
2. Infinispan: Provide cache support for Jboss EAP to provide **local** cache which stored locally, and **temprory** cache which handle user session, tokens, etc. 
3. Keycloak Server
4. Undertow: webserver and server container.  
