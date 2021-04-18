# Security

## Common

A **denial of service** attack is about overloading the system with too much data or too many requests to process legitimate incoming requests.

**Inclusion attacks** occur when multiple files or components are embedded within a single entity, such as a zip bomb or the billion laughs attack. Both can be thwarted with depth limits.

The vanilla **Billion Laughs attack** is illustrated in the XML file represented below.

```text
<?xml version="1.0"?>
<!DOCTYPE lolz [
<!ENTITY lol "lol">
<!ENTITY lol2 "&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;">
<!ENTITY lol3 "&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;">
<!ENTITY lol4 "&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;">
<!ENTITY lol5 "&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;">
<!ENTITY lol6 "&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;&lol5;">
<!ENTITY lol7 "&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;&lol6;">
<!ENTITY lol8 "&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;&lol7;">
<!ENTITY lol9 "&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;&lol8;">
]>
<lolz>&lol9;</lolz>
```

In this example, there are 10 different XML entities, `lol` – `lol9`. The first entity, `lol` is defined to be the string “lol”.  However, each of the other entities is defined to be 10 of another entity.  The document content section of this XML file contains a reference to only one instance of the entity `lol9`.  However, when this is being parsed by a DOM or SAX parser, when `lol9` is encountered, it is expanded into 10 `lol8`’s, each of which is expanded into 10 `lol7`’s, and so on and so forth.  By the time everything is expanded to the text `lol`, there are 100,000,000 instances of the string "lol".  If there was one more entity, or `lol` was defined as 10 strings of “lol”, there would be a Billion “lol”s, hence the name of the attack.  Needless to say, these many expansions consume an exponential amount of resources and time, causing the DoS.

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

