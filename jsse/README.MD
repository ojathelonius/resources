## JSSE
The Java Secure Socket Extension provides a set of packages that enable secure Internet communications.

* [Common keytool commands](#common-keytool-commands)

### Common keytool commands

Force english language :
 ```
-J-Duser.language=en 
 ```
 
Generate keystore :
 ```
 keytool -genkeypair -alias key_name -keyalg RSA -keystore my_keystore.keystore
  ```

View keystore certificates :
 ```
keytool -list -v -keystore my_keystore.keystore -J-Duser.language=en
 ```

Export certificate (public key) from keystore :
 ```
keytool -exportcert -alias private_key_alias -file my_cert.cer -keystore my_keystore.keystore
 ```

Import certificate into truststore :
 ```
keytool -importcert -alias external_cert -keystore my_truststore.truststore -file external_cert.cer -noprompt
 ```

Delete trusted certificate :
 ```
keytool -delete -alias my_cert -keystore my_keystore
 ```
