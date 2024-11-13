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

### Get file permission in number format.
```shell
stat -s [TARGET_FILE or DIRECTORY] | cut -d " " -f3
# st_mode=...
```

### Export each line from command as envs.
```shell
export $([COMMAND] | xargs -L 1)

## Ex. export all Heroku config vars
## execute `heroku login` in advance.
# export $(heroku config -s | xargs -L 1)
```

### Count lines in multiple files.
```shell
ls [TARGET_FILES] | xargs -L 1 wc -l
```

### Disable "not ejected safely" notification.
- https://www.reddit.com/r/mac/comments/vsn1t6/how_to_disable_not_ejected_safely_notification_on/
- https://x.com/Daihuku0015/status/1856352162298900757
```shell
sudo defaults write /Library/Preferences/SystemConfiguration/com.apple.DiskArbitration.diskarbitrationd.plist DADisableEjectNotification -bool YES && sudo pkill diskarbitrationd

# type in login password
# restart Mac
```

### Check listening ports.
- https://qiita.com/yokozawa/items/dbcb3b31f9308e4dcefc
- https://zenn.dev/kazu_o/scraps/b287f569e3fac5
```shell
lsof -i -P | grep "LISTEN" # add 'sudo' if needed

## close listening process
# kill -9 [PID]
```

<br>

## Node.js

### List all packages installed grobally with each version.
- https://diwao.com/2019/09/npm-list-global.html
```shell
npm list -g --depth=0
```

### To be continued.
