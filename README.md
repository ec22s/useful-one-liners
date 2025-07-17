# useful-one-liners
Some one-liners as note to self.

### Environment
```shell
$SHELL --version
# GNU bash, version 5.2.26(1)-release (x86_64-apple-darwin22.6.0)
```

<br>

### For cloud storage: download all files in a virtual local directory (e.g. My dirve in Google Drive).
```sh
find {TAGET_DIRECTORY} -type f -exec wc {} \;
```

### For cloud storage: Exclude a file or directory from being synced to cloud. 
```sh
# exclude
xattr -w 'com.apple.fileprovider.ignore#P' 1 {TARGET_FILE or DIRECTORY}
```
```sh
# reinclude
xattr -d 'com.apple.fileprovider.ignore#P' {TARGET_FILE or DIRECTORY}
```
```sh
# Ex. exclude all node_module directories from sync
find {TAGET_DIRECTORY} -type d | grep "/node_modules" | grep -v "/node_modules/" | xargs -I% xattr -w 'com.apple.fileprovider.ignore#P' 1 "%"
```

<br>

### Format number with thousand separator.

```sh
echo 5555 | LC_ALL="en_US" printf "%'.f\n" $(cat)
```

```sh
# applying for remote file
(curl --head https://demo-bucket.protomaps.com/v4.pmtiles 2>/dev/null) | grep length | awk -F ': ' '{print $2}' | LC_ALL="en_US" printf "%'.f\n" $(cat) 2>/dev/null
```

<br>

### Create text file from encoded hex codes.
```sh
echo -n "879F0a879F879F879F879F" | xxd -p -r > test.txt
# hexdump test.txt # check
# Ex. '俱' as '0x879F' in encoding SHIFT_JIS_2004 (JIS X 0123)
# 0a is a linebreak.
```

### Get sequence with zero-paddding.
```sh
seq -w 1 100
```

### Remove all .DS_Store files in the current and sub directories.
```sh
# fast but unavailable for paths including a single quote (')
find . -name ".DS_Store" | xargs rm
```
```sh
# slow but available for paths above
find . -name ".DS_Store" -exec rm {} \;

# Ref. https://detail.chiebukuro.yahoo.co.jp/qa/question_detail/q12198239097
```

### Replace file names with sequential numbers.
```sh
# Ref. https://marbles.hatenablog.com/entry/2018/09/08/222745
# Ex. list files in order of date and rename to 2-digits with zero padding numbers.
ls -tr * | awk '{ printf "mv \"%s\" %02d\n", $0, NR}' | sh
```

### Get file permission in number format.
```sh
stat -s {TARGET_FILE or DIRECTORY} | cut -d " " -f3
# st_mode=...
```

### Export each line from command as envs.
```sh
export $({COMMAND} | xargs -L 1)
```

### Count lines in multiple files.
```sh
ls {TARGET_FILES} | xargs -L 1 wc -l
```

### Check listening ports.
- https://qiita.com/yokozawa/items/dbcb3b31f9308e4dcefc
- https://zenn.dev/kazu_o/scraps/b287f569e3fac5
```sh
lsof -i -P | grep "LISTEN" # add 'sudo' if needed

## close listening process
# kill -9 {PID}
```

<br>

## macOS

### Disable "not ejected safely" notification.
- https://www.reddit.com/r/mac/comments/vsn1t6/how_to_disable_not_ejected_safely_notification_on/
- https://x.com/Daihuku0015/status/1856352162298900757
```sh
sudo defaults write /Library/Preferences/SystemConfiguration/com.apple.DiskArbitration.diskarbitrationd.plist DADisableEjectNotification -bool YES && sudo pkill diskarbitrationd

# type in login password
# restart Mac
```

### Disable shadow of screenshots.
```sh
defaults write com.apple.screencapture disable-shadow -bool true
killall SystemUIServer
```

<br>

## CSV

### Get unique values in one column.
```sh
cut -d, -f {COLUMN_INDEX} {CSV_FILE_NAME} | awk '!a[$0]++{print}'
```
### Add random values to each row.
```sh
cat {INPUT_CSV} | xargs -I{} bash -c 'echo "{}",$RANDOM,$RANDOM' > {OUTPUT_CSV}
```

### Extract N columns from the beginning.
```sh
# https://nvnote.com/awk-delete-column/
# Ex. from the second to last columns
awk -F "," 'OFS="," {for(i=2;i<NF;i++){printf("%s%s",$i,OFS)}print $NF}' {INPUT_CSV} > {OUTPUT_CSV}
```

<br>

## FFmpeg

### Version 
```sh
ffmpeg -version
# ffmpeg version 7.1 Copyright (c) 2000-2024 the FFmpeg developers
# built with Apple clang version 16.0.0 (clang-1600.0.26.4)
```

### Output all keyframes as PNG from a movie.
```sh
ffmpeg -skip_frame nokey -i {INPUT_MOVIE_FILE} -vf framestep=1 -fps_mode passthrough [OUTPUT_DIRECTORY]%05d.png

# outputs 00001.png, 00002.png, ...
```

<br>

## ImageMagick

### Resize an image by percent value.
```sh
magick {INPUT_FILE} -resize {PERCENT_VALUE}% {OUTPUT_FILE}
```

### Crop multiple files.
```sh
# Ex. width=1920, height=1080, left=100 and top=50
ls *.png | xargs -I% magick % -crop 1920x1080+100+50 output/%
```

### Convert svg to png with size specified.
```sh
# Ex. png_width=1024, svg_width=36
magick -density `echo "scale=1; 96*1024/36 + 0.5" | bc | xargs printf "%.0f"` {INPUT.svg} {OUTPUT}.png
```

<br>

## JavaScript

### Count frequencies of array elements.
```javascript
// Ex.
[1, 2, 2, 3, 3, 3].reduce((acc, v) => { acc[v] = (acc[v] || 0) + 1; return acc }, {})
```

### Display comma separated number.
```javascript
// Ref. https://stackoverflow.com/questions/50961083
const ex1 = Number(12345678.999).toFixed(0).replace(/(\d)(?=(\d{3})+$)/g, '$1,');
// "12,345,679"
const ex2 = Number(12345678.999).toFixed(2).replace(/(\d)(?=(\d{3})+\.)/g, '$1,');
// "12,345,679.00" 
```

<br>

## Node.js

### Display MD5 hash value.
```sh
node -v
# v23.8.0

node -e "const { createHash: hash } = require('node:crypto'); console.log(hash('md5').update('STRING_TO_BE_HASHED').digest('hex'));"
```

### List all packages installed grobally with each version.
- https://diwao.com/2019/09/npm-list-global.html
```sh
npm list -g --depth=0
```

### To be continued.
