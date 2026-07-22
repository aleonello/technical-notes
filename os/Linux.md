# Linux

## Links

* [Linux man pages online](https://man7.org/linux/man-pages/)
* [tldr-pages](https://tldr.sh/)
* [explainshell](https://explainshell.com/)
* [GNU Coreutils Documentation](https://www.gnu.org/software/coreutils/manual/coreutils.html)
* [systemd Documentation](https://systemd.io/)
* [Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.html)

## Systemctl - View service logs

```sh
journalctl -u <service>
```

Useful flags:
* `--no-pager` — print the full log without pagination.
* `-e` — jump to the end of the log.
* `-f` — follow (stream) new log entries.
* `--help` — list all available options.
