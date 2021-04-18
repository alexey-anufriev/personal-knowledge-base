# Security

## Common

A **denial of service** attack is about overloading the system with too much data or too many requests to process legitimate incoming requests.

## Security Manager

To add another policy file in addition to the default JRE’s, thus adding more permissions, launch the JVM with: `java -Djava.security.manager -Djava.security.policy=/path/to/other.policy`.

To replace the default policy file with your own, launch the JVM with: `java -Djava.security.manager -Djava.security.policy==/path/to/other.policy`.

