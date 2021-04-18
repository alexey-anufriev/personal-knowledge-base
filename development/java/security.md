# Security

## Common

A **denial of service** attack is about overloading the system with too much data or too many requests to process legitimate incoming requests.

## Security Manager

To add another policy file in addition to the default JRE’s, thus adding more permissions, launch the JVM with: `java -Djava.security.manager -Djava.security.policy=/path/to/other.policy`.

To replace the default policy file with your own, launch the JVM with: `java -Djava.security.manager -Djava.security.policy==/path/to/other.policy`.

Sample policy:

```text
grant codeBase "file:target/spring-petclinic-1.4.2.jar" {
  permission java.lang.RuntimePermission "getProtectionDomain";
  permission java.util.PropertyPermission "java.protocol.handler.pkgs", "read,write";
  permission java.io.FilePermission "${java.io.tmpdir}/-", "read,write,delete";
  permission java.io.FilePermission "src/main/webapp/WEB-INF/classes/org/springframework/boot/autoconfigure/web/*", "read";
  permission java.net.SocketPermission "localhost:8080", "listen,resolve";
};
```

