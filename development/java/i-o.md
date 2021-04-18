# I/O

## Common

`StandardCopyOption.ATOMIC_MOVE` - performs the move as an atomic file operation. If the file system does not support an atomic move, an exception is thrown. With an `ATOMIC_MOVE` you can move a file into a directory and be guaranteed that any process watching the directory accesses a complete file.

`System.console()` may be `null`. But `System.in` - not.

## Serialization

When data is deserialized, none of variable initializers, instance initializers, or constructors is called. The class can have static initializers, but they are not called as part of deserialization.

All `transient` and `static` fields do not get serialized.

The `serialVersionUID` is used to verify that the serialized and deserialized objects have the same attributes and thus are compatible with deserialization.

We can override the default serialization behavior inside our Java class by providing the implementation of `writeObject` and `readObject` methods.

And we can call `ObjectOutputStream.defaultWriteObject` and `ObjectInputStream.defaultReadObject` from `writeObject` and `readObject` methods to get the default serialization and deserialization logic.

The Java Serialization process can be further customized and enhanced using the `Externalizable` interface.

`writeReplace` method allows the developer to provide a replacement object that will be serialized instead of the original one. And the `readResolve` method is used during deserialization process to replace the de-serialized object by another one of our choices. The method `readResolve` is called after `readObject` has returned \(conversely `writeReplace` is called before `writeObject` and probably on a different object\).

If we want to perform certain validations on some of our fields, we can do that by implementing `ObjectInputValidation` interface and overriding the `validateObject` method from it. The method `validateObject` will automatically get called when we register this validation by calling `ObjectInputStream.registerValidation(this, 0)` from `readObject` method. It is very useful to verify that stream has not been tampered with, or that the data makes sense before handing it back to your application.

```java
@Override
public void validateObject() {
    System.out.println("Validating");
}

// Custom serialization logic
private void writeObject(ObjectOutputStream oos) throws IOException {
    System.out.println("Custom serialization logic invoked.");
    oos.defaultWriteObject(); // Calling the default serialization logic
}

// Replacing serializing object
private Object writeReplace() throws ObjectStreamException {
    System.out.println("Replacing serialising object by this.");
    return this;
}

// Custom de-serialization logic
private void readObject(ObjectInputStream ois) throws IOException, ClassNotFoundException {
    System.out.println("Custom deserialization logic invoked.");

    ois.registerValidation(this, 0); // Registering validations

    ois.defaultReadObject(); // Calling the default deserialization logic.
}

// Replacing de-serializing object with this,
private Object readResolve() throws ObjectStreamException {
    System.out.println("Replacing de-serializing object by this.");
    return this;
}
```

In the following way the list of fields to serialization can be customized:

```java
class List implements Serializable {
    List next;

    private static final ObjectStreamField[] serialPersistentFields
                 = {new ObjectStreamField("next", List.class)};
}
```

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

`relativize` build a relative path:

```java
Path p1 = Path.of("/p");
Path p2 = Path.of("/p/sp/ssp");

System.out.println(p1.relativize(p2)); // sp/ssp
```

`relativize` can operate over two relative or absolute paths. In case of mix there will be an exception: `IllegalArgumentException: 'other' is different type of Path`.

`subpath` / `toAbsolutePath`:

```java
Path p = Path.of("/p/sp/ssp");
System.out.println(p.subpath(1 /* start inclusive */, 2 /* end, exclusive */)
        .getName(0).toAbsolutePath()); // <current-dir>/sp
```

## `Files`

Reading the complete file:

```java
Files.readAllLines(Path.of("some-file")) // List<String>
        .stream()
        .forEach(System.out::println);
        
Files.lines(Path.of("some-file")) // => Stream<String>
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

Walking over the FS:

```java
Files.walk(Path.of("."), /* maxDepth, def is Integer.MAX_VALUE */ 2) // Stream<Path>
        .forEach(System.out::println);
        
Files.list(Path.of(".")) // Stream<Path>
        .forEach(System.out::println);
```

Searching:

```java
Files.find(
        Path.of("."),
        /* maxDepth, no def value */ 1,
        /* matcher */ (path, attributes) -> true
).forEach(System.out::println);
```

## `InputStream`

`void mark(int readlimit)` - marks the current position in this input stream. A subsequent call to the reset method repositions this stream at the last marked position so that subsequent reads re-read the same bytes. The `readlimit` arguments tells this input stream to allow that many bytes to be read before the mark position gets invalidated, but this is just a recommendation, a stream may be able to reset even after a bigger number of reads.

```java
byte[] bytes = new byte[] {1,2,3,4,5,6,7,8,9,10};
InputStream is = new BufferedInputStream(new ByteArrayInputStream(bytes));

is.read(new byte[2]);
is.mark(2); // 2 may be ignored
is.read();
is.read();
is.read();
is.read();
is.reset();
System.out.println(is.read()); // 3

is.skip(3);
System.out.println(is.read()); // 7
```

## `ObjectInputStream`

`readObject()` may return `null` when `writeObject(null)` was executed. Correct stop signal for the reader would be to keep executing `readObject()` until an `EOFException` is thrown.

`ObjectInputStream.GetField readFields()` reads the persistent fields from the stream and makes them available by name

## try-with-resources

On the last line exception will happen as the input stream has been already closed, but an exception will not be printed as the stream for errors has been also closed:

```java
BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
PrintStream err = System.err;

try (reader; err) {
    System.out.println(reader.readLine());
}

System.out.println(reader.readLine());
```

