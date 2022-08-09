# File System

## FS Watch

`inotify` or `incron` are good options for watching the file system.

## Find File

`find . -name some-file -exec ls {} \;` to find files recursively and execute an operation with it.

## Replace in Files from Subfolders

NOTE: _this is Mac version._

```
for d in */ ; do                                                                                                                                                                                                                                7531  11:43:26    develop ⎈
    dir_name=${d%/}
    sed -i '' "s/placeholder/$dir_name/g" ./$dir_name/config.yaml
done
```

