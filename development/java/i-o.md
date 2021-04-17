# I/O

## Common

`StandardCopyOption.ATOMIC_MOVE` - performs the move as an atomic file operation. If the file system does not support an atomic move, an exception is thrown. With an `ATOMIC_MOVE` you can move a file into a directory and be guaranteed that any process watching the directory accesses a complete file.

## Path

`resolve` returns either other Path if it is absolute, or concatenation:

```text
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

```text
Path p1 = Path.of("/p");
Path p2 = Path.of("/p/sp/ssp");

System.out.println(p1.relativize(p2)); // sp/ssp
```

