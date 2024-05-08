# Git

## [Shell] List remote git address in subfolders

```shell
find . -type f -path "*git*" -iname "config" -exec grep "https" {} \;
```

## [Shell] Git Pull and Git Fetch for each directory in a path

```shell
for i in $(find . -mindepth 1 -maxdepth 1 -type d); do cd "${i}" &&  echo "processing: ${i}" && if [ -d .git ]; then git pull && git fetch -p; fi && cd ..; done
```
