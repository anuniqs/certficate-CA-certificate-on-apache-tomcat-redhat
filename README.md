### Self signed - 

SIte address : https://tomcat.apache.org/tomcat-8.5-doc/ssl-howto.html

[root@192 bin]# keytool -genkey -alias alias_name -keyalg RSA -keysize 2048 -validity 10000 -v -keystore "/home/anup/java-11/bin/mykeystore.jks" -ext SAN=dns:localhost,ip:192.168.43.135


[root@192 apache-tomcat-9.0.41]# nano conf/server.xml

    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
 

    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               maxThreads="150" SSLEnabled="true"
               compression="on" scheme="https" secure="true"
               keyAlias="alias_name" keystoreFile="/home/anup/java-11/bin/my-release-key.jks"
               keystorePass="Orbit@2020" clientAuth="false"
               SSLVerifyClient="none" sslProtocol="TLS" />


### Creating a CA-Signed Certificate for the Tomcat Server - 

1. To generate a CSR, run the following command:

keytool -certreq -keyalg RSA -alias tomcat -file C:\somename.csr -keystore C:\mykeystore.jks -validity <daysValid> -ext SAN=dns:<domainname>

2. Upload the CSR to the CA website, indicate the type of Tomcat server, and submit for signing.

3. Download the root, intermediate, and issued server/domain certificates.

4. Import each signed certificate that is issued by the CA using the following commands:

**Root certificate:** keytool -import -alias root -keystore C:\mykeystore.jks -trustcacerts -file C:\valicert_class2_root.crt

**Intermediate certificate:** keytool -import -alias intermed -keystore C:\mykeystore.jks -trustcacerts -file C:\gd_intermediate.crt

**Issued server/domain certificate:** keytool -import -alias tomcat -keystore C:\mykeystore.jks -trustcacerts -file C:\server_certificate_whatevername.crt

5. Close the command line.
