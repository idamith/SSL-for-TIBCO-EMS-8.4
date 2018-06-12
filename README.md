## POC on enable SSL\TLS for JMS (Tibco EMS)

This POC is to prove the ability of implementing SSL for those JMS services with Tibco EMS 8.4

# Prerequisites & limitations

Note: this document address only one-way SSL. 
Trust store password has left plain for this demo purpose however, it should be encrypted before release to production.

Prerequisites: 
•	Upgrade current Tibco EMS Simulator to 8.3 or above
•	Prepare JMS Server certificate
•	Set up SSL in Tibco EMS Simulator
•	Set up SSLQueueConnectionFactory on Tibco EMS Simulator 
•	Verify Tibco EMS Simulator SSL URL and port is accessible from client machine
•	Upgrade Tibco Client Java to Java 8
•	Upgrade Tibco Client tibjms.jar to 8.3 or above
•	Upgrade Tibco Client to JMS 2.0
•	Prepare Trust Store (JKS). Ensure server root certificate is in trust store.

# Implementation 

Upgrade current Tibco EMS Simulator to 8.3 or above

Download an install latest Tibco EMS Server. At the moment this document is written, version is 8.4.
Click [here](https://www.tibco.com/resources/product-download/tibco-enterprise-message-service-community-edition-free-download-windows) for download latest TIBCO EMS for Windows.
Click [here](https://docs.tibco.com/products/tibco-enterprise-message-service) for TIBCO EMS documentations.

# Prepare JMS Server certificate

For simulation purpose those sample certs come with TIBCO EMS installation shall be used. Those certs are located at %TIBCO_HOME%\ ems\8.4\samples\certs

# Set up SSL in Tibco EMS Simulator

Set up to be done in the file %TIBCO_HOME%\ ems\8.4\bin\tibemsd.conf

[IMAGE]

# Set up SSLQueueConnectionFactory on Tibco EMS Simulator 

Set up to be done in the file %TIBCO_HOME%\ ems\8.4\bin\factories.conf

[IMAGE]

# Verify Tibco EMS Simulator SSL URL and port is accessible from client machine

Check the accessibility with tibemsadmin.exe located at %EMS_HOME%\bin. Note, this admin should run on the client machine. Let the user name and password be as default.

[IMAGE]

# Java, Tibco & JMS library upgrade

•	Confirm JDK and JRE is Java 8
•	Remove javax.jms.jar & tibjms.jar from existing classpath
•	Copy new tibjms.jar, jms-2.0.jar from %EMS_HOME%\lib and add to the classpath

# Prepare Trust Store (JKS). Ensure server root certificate is in trust store.

•	Create empty Trust Store (JKS).
o	Use following command to create the store: -
keytool -genkey -alias server-alias -keyalg RSA -keypass changeit -storepass changeit -keystore truststore.jks

o	Use following command to remove default certificate from the truststore: -
keytool -delete -alias server-alias -keystore truststore.jks

o	Use following command to verify the content of the truststore: -
keytool -list -keystore truststore.jks

[IMAGE]

•	Ensure server root certificate is in trust store.
o	Import Tibco EMS server certificate to the truststore. User following command: -
keytool -import -v -trustcacerts -alias latte-tibco-srv -file %ems_home%\samples\certs\server_root.cert.pem -keystore truststore.jks
o	Use following command to verify the content of the truststore: -
keytool -list -keystore truststore.jks

[IMAGE]









