---
description: Jigsaw
---

# Modules

## Common

`java --list-modules` gives an ability to list all modules in JDK.

For `javac` `-d` option specifies the directory and `-p` option specifies the module path.

The `jdeps` command lists information about dependencies within a module. The `-s` option provides a summary of output rather than verbose output.

The rules for determining the name of the automatic module include removing the extension, removing numbers, and changing special characters to periods \(.\).

## Migration

A top‐down migration starts by moving all the modules to the module path as automatic modules. Then, the migration changes each module from an automatic module to a named module.

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
```

It is logical to combine the service locator and service provider interface because neither has a direct reference to the service provider.

```text
ServiceInterface svc = ServiceLoader.load(SomeService.class).findFirst().get();

// or

ServiceLoader.load(ServiceInterface.class).stream()
        .map(ServiceLoader.Provider::get)
        .collect(Collectors.toList());
```

