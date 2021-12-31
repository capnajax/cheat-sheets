# Configuring NTP service on API Connect OVAs post-install

It's best to set up NTP services at the time you're installing API Connect using (this procedure)[https://www.ibm.com/docs/en/api-connect/10.0.1.x?topic=options-configuring-use-external-ntp-server], but once it's already installed, we can still reconfigure the virtual machines by configuring timesyncd on each machine by hand.

## Review the current status of timesyncd (the local NTP service)

```shell
$> sudo systemctl status systemd-timesyncd.service
```

## Edit the NTP config file

```shell
$> sudo vi /etc/systemd/timesyncd.conf
```

Uncomment and change the line that starts with `#NTP=` to `NTP=ntp1.local ntp2.local ntp3.local` (of course, use your own servers, and separate them with spaces, not commas.

Example: 

```
[Time]
NTP=10.11.0.9 10.11.0.8
#FallbackNTP=ntp.ubuntu.com
#RootDistanceMaxSec=5
#PollIntervalMinSec=32
#PollIntervalMaxSec=2048
```

## Restart timesyncd

```shell
$> sudo systemctl restart systemd-timesyncd.service
```

## Review the status

```shell
$> sudo systemctl restart systemd-timesyncd.service

● systemd-timesyncd.service - Network Time Synchronization
     Loaded: loaded (/lib/systemd/system/systemd-timesyncd.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2021-12-31 04:46:38 UTC; 13min ago
       Docs: man:systemd-timesyncd.service(8)
   Main PID: 2822182 (systemd-timesyn)
     Status: "Initial synchronization to time server 10.11.0.9:123 (10.11.0.9)."
      Tasks: 2 (limit: 38499)
     Memory: 1.6M
     CGroup: /system.slice/systemd-timesyncd.service
             └─2822182 /lib/systemd/systemd-timesyncd
```

You should see your new time servers in the `Status:` line. If the Status line says `Idle` or shows `ntp.ubuntu.com`, you probably mistyped the server hostnames in step 2.
