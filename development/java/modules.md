---
description: Jigsaw
---

# Modules

## Common

`java --list-modules` gives an ability to list all modules in JDK.

## Service

Jigsaw allows to build pluggable modules in the following way:

![Service architecture](../../.gitbook/assets/image.png)

```text
module ServiceInterface {
    exports serviceinterface;
}

module Provider {
    requires ServiceInterface;
    provides serviceinterface.ServiceInterface with serviceprovider.Provider;
}

module Consumer {
    requires ServiceInterface;
    uses serviceinterface.ServiceInterface;
}

ServiceLoader<ServiceInterface> loader = ServiceLoader.load(ServiceInterface.class);
```

