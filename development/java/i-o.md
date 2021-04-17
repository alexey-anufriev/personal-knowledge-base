# I/O

## Common

`StandardCopyOption.ATOMIC_MOVE` - performs the move as an atomic file operation. If the file system does not support an atomic move, an exception is thrown. With an `ATOMIC_MOVE` you can move a file into a directory and be guaranteed that any process watching the directory accesses a complete file.

## `Path`

`resolve` returns either other Path if it is absolute, or concatenation:

```java
Path abs = Path.of("/abs");
Path abs2 = Path.of("/abs2");
Path rel = Path.of("./rel");
Path rel2 = Path.of("./rel2");

System.out.println(abs.resolve(abs2)); // /abs2
System.out.println(abs.resolve(rel));  // /abs/./rel
System.out.println(rel.resolve(abs));  // /abs
System.out.println(rel.resolve(rel2)); // ./rel/./rel2
```

relativize build a relative path:

```java
Path p1 = Path.of("/p");
Path p2 = Path.of("/p/sp/ssp");

System.out.println(p1.relativize(p2)); // sp/ssp
```

## `Files`

Reading the complete file:

```java
Files.readAllLines(Path.of("some-file")) // List<String>
        .stream()
        .forEach(System.out::println);
```

Get file attributes:

```java
BasicFileAttributes attr = 
        Files.readAttributes(Path.of("some-file"), BasicFileAttributes.class);
System.out.println(attr.size()); // file size
```

Modify file's dates:

```java
BasicFileAttributeView attributeView = 
        Files.getFileAttributeView(Path.of("some-file"), BasicFileAttributeView.class);
attributeView.setTimes(...); // access, creation, modification dates
```

