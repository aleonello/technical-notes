# Git

## Links

* [Official Git Documentation](https://git-scm.com/doc)
* [Pro Git Book](https://git-scm.com/book/en/v2)
* [Git Cheat Sheet (GitHub)](https://education.github.com/git-cheat-sheet-education.pdf)
* [GitHub Flow](https://docs.github.com/en/get-started/using-github/github-flow)
* [Conventional Commits](https://www.conventionalcommits.org/)
* [gitignore.io — Generate .gitignore templates](https://www.toptal.com/developers/gitignore)

## List remote URLs in all subfolders

```sh
find . -type f -path "*git*" -iname "config" -exec grep "https" {} \;
```

## Git pull and fetch for each subdirectory

```sh
for i in $(find . -mindepth 1 -maxdepth 1 -type d); do
  cd "${i}" && echo "processing: ${i}"
  if [ -d .git ]; then git pull && git fetch -p; fi
  cd ..
done
```

## Store credentials

```sh
git config --global credential.helper store
```

## Add an existing project to a remote repository

```sh
git init
git add --all
git commit -m "Commit inicial"
git remote add origin <remote_repository_url>
git push -u origin master
```

## Clean local branches already merged / removed from remote

Prune tracking references and delete local branches that no longer exist on origin:

```sh
git fetch --prune
git branch -r | awk '{print $1}' | egrep -v -f /dev/fd/0 <(git branch -vv | grep origin) | awk '{print $1}' | xargs git branch -d
```

Use `-D` instead of `-d` to force-delete branches that have not been merged yet.
