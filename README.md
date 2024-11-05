# useful-one-liners
Some one-liners as note to self.

### Environment
```shell
$SHELL --version
# GNU bash, version 5.2.26(1)-release (x86_64-apple-darwin22.6.0)
```

### Exclude a file or directory from being synced to cloud storage. 
```shell
# exclude
xattr -w 'com.apple.fileprovider.ignore#P' 1 [TARGET_FILE or DIRECTORY]

# reinclude
xattr -d 'com.apple.fileprovider.ignore#P' [TARGET_FILE or DIRECTORY]
```

### Get file permissions in number format.
```shell
stat -s [TARGET_FILE or DIRECTORY] | cut -d " " -f3
# st_mode=...
```

### To be continued.
