## JSSE
The Java Secure Socket Extension provides a set of packages that enable secure Internet communications.

* [Common keytool commands](#common-keytool-commands)
* [Import a certificate to Java](#import-a-certificate-into-java)

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

### Import a certificate into Java
Java's default truststore is in `C:\Program Files\Java\jdk1.8.0_211\jre\lib\security\cacerts`.


On Windows, add an `example.cer` using :
```
keytool -importcert -keystore "C:\Program Files\Java\jdk1.8.0_211\jre\lib\security\cacerts" -file "C:\Users\Documents\example.cer"
```

Note : the truststore default password is `changeit`.
