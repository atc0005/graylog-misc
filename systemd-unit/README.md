# Graylog systemd unit file

## Our situation

We run an internal Graylog appliance based off of the official 2.x OVA image.
We've run it a few years now for small internal testing purposes and are happy
with the results. We recently upgraded the OS from Ubuntu 14.04 to 16.04 and
Graylog no longer automatically started at boot. This was because Upstart was
used to handle starting/stopping all required daemons. Thankfully, the existing
shell script which handles the startup process still works when called
directly and the `graylog-ctl` script still safely shuts everything down.

While an upgrade path from the 2.x OVA-based Graylog installation to a 3.0
installation isn't officially supported, we are able to at least run an
updated OS until we have time to manually port the settings/data over to a
fresh installation of Graylog.

## Workaround

I borrowed from other unit files to write a simple systemd unit that calls the
existing tooling for starting/stopping services. It's essentially a wrapper
until we can upgrade to a fully supported configuration.

```ini
ExecStart=/opt/graylog/embedded/bin/runsvdir-start
ExecStop=/usr/bin/graylog-ctl stop
```

Other settings are the file are mostly standard items other than the
`SuccessExitStatus` parameter which is pulled directly from the `runsvdir`
documentation.

## References

- <https://packages.graylog2.org/appliances/ova>
- <http://docs.graylog.org/en/3.0/pages/installation/virtual_machine_appliances.html>
