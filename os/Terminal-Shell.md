# Terminal Shell

## Links

* [Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.html)
* [ShellCheck — shell script static analysis tool](https://www.shellcheck.net/)
* [Oh My Zsh](https://ohmyz.sh/)
* [fzf — command-line fuzzy finder](https://github.com/junegunn/fzf)
* [tmux GitHub](https://github.com/tmux/tmux)

#

## Terminal Multiplexer - TMux / BYOBU

## Tmux / Byobu Links

* [Tmux Cheat Sheet & Quick Reference](https://tmuxcheatsheet.com/)
* [Byobu Cheat Sheet](https://openclipart.org/pdf/250251/Byobu-Keybindings.pdf)
* [Byobu cheatsheet — most used hotkeys](https://medium.com/russian-it-stories/byobu-cheatsheet-%D0%BCost-used-hotkeys-5a8bbd8476fd)

## Git pull for all subdirectories (MINGW64 / Bash)

```sh
for i in $(ls -d */); do cd $i; echo $i; git pull; cd ..; done
```
