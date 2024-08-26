# Linux

## Systemctl Services Log at the end

```sh
journalctl -u <service>
```

* appending `--no-pager` will print full log, so you wont have to scroll.
* Appending `-e` will start the log at the end removing the need to scroll, but without printing the entire log beforehand.
* appending `-f` will follow (print) updates to the log.
* appending `--help` will let you see all available options.