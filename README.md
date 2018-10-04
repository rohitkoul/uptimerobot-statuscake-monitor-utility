# UptimeRobot Monitor Utility

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/da395757a07e45e9a57f8e23bd9aa614)](https://www.codacy.com/app/christronyxyocum/uptimerobot-monitor-utility?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=christronyxyocum/uptimerobot-monitor-utility&amp;utm_campaign=Badge_Grade) [![GitHub](https://img.shields.io/github/license/mashape/apistatus.svg)](https://github.com/christronyxyocum/uptimerobot-monitor-utility/blob/master/LICENSE.md) ![Github All Releases](https://img.shields.io/github/downloads/christronyxyocum/uptimerobot-monitor-utility/total.svg)

A bash script to work with [UptimeRobot](https://uptimerobot.com) monitors via the API. It checks to make sure that the API key that you provided is valid before performing any requested operations.

It is recommended that you install the JQ package as the script uses it to automatically format the JSON output into a much more human-readable and colorized output. If you do not install you will see errors about the `jq` command not being found and it may impact the functionality of the script.

```bash
tronyx@suladan:~$ sudo apt install jq
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following NEW packages will be installed:
  jq
0 upgraded, 1 newly installed, 0 to remove and 7 not upgraded.
Need to get 45.6 kB of archives.
After this operation, 90.1 kB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu bionic/universe amd64 jq amd64 1.5+dfsg-2 [45.6 kB]
Fetched 45.6 kB in 0s (123 kB/s)
Selecting previously unselected package jq.
(Reading database ... 107503 files and directories currently installed.)
Preparing to unpack .../jq_1.5+dfsg-2_amd64.deb ...
Unpacking jq (1.5+dfsg-2) ...
Setting up jq (1.5+dfsg-2) ...
Processing triggers for man-db (2.8.3-2) ...
```

## Setting it up

To get the script working you will need to clone the repo onto your preferred machine:

```python
tronyx@suladan:~$ git clone https://github.com/christronyxyocum/uptimerobot-monitor-utility.git
Cloning into 'uptimerobot-monitor-utility'...
remote: Enumerating objects: 108, done.
remote: Counting objects: 100% (108/108), done.
remote: Compressing objects: 100% (75/75), done.
remote: Total 262 (delta 60), reused 76 (delta 32), pack-reused 154
Receiving objects: 100% (262/262), 161.85 KiB | 6.74 MiB/s, done.
Resolving deltas: 100% (143/143), done.
```

Then make it executable:

```python
tronyx@suladan:~$ cd uptimerobot-monitor-utility
tronyx@suladan:~/uptimerobot-monitor-utility$ chmod a+x uptimerobot_monitor_utility.sh
```

Finally, open the script with your favorite text editor and add your UptimeRobot API key. If you forget this step the script will prompt you to enter your API key:

![API Key Prompt](/Images/api_key.png)

After entering your API key, the script will check whether or not it is valid and add it to the script for you.

If you are not running the script as the root user, which is recommended, you will need to use `sudo` as the script creates a directory in `/tmp`.

If you use the alert option, be sure to also enter in your Discord webhook URL.

## Usage

![Script Usage](/Images/usage.png)

## Examples
### List all monitors

```python
tronyx@suladan:~/uptimerobot-monitor-utility$ sudo ./uptimerobot_monitor_utility.sh -l
The following UptimeRobot monitors were found in your UptimeRobot account:
Plex (ID: 779783111) - Status: Up
Radarr (ID: 780859973) - Status: Down
Sonarr (ID: 780859962) - Status: Paused
Tautulli (ID: 780859975) - Status: Seems down
```

### Find paused monitors

```python
tronyx@suladan:~/uptimerobot-monitor-utility$ sudo ./uptimerobot_monitor_utility.sh -f
The following UptimeRobot monitors are currently paused:
Plex (ID: 779783111)
Radarr (ID: 780859973)
Sonarr (ID: 780859962)
Tautulli (ID: 780859975)

Would you like to unpause the paused monitors? ([y]es or [n]o):
```

### Info

```json
tronyx@suladan:~/uptimerobot-monitor-utility$ sudo ./uptimerobot_monitor_utility.sh -i 'plex'
{
  "stat": "ok",
  "pagination": {
    "offset": 0,
    "limit": 50,
    "total": 1
  },
  "monitors": [
    {
      "id": 779783111,
      "friendly_name": "Plex",
      "url": "https://plex.tv",
      "type": 1,
      "sub_type": "",
      "keyword_type": null,
      "keyword_value": "",
      "http_username": "",
      "http_password": "",
      "port": "",
      "interval": 300,
      "status": 2,
      "create_datetime": 1513815865
    }
  ]
}
```

### Pause all monitors

```json
tronyx@suladan:~/uptimerobot-monitor-utility$ sudo ./uptimerobot_monitor_utility.sh -p all
Pausing Plex:
{
  "stat": "ok",
  "monitor": {
    "id": 779783111
  }
}

Pausing Radarr:
{
  "stat": "ok",
  "monitor": {
    "id": 780859973
  }
}

Pausing Sonarr:
{
  "stat": "ok",
  "monitor": {
    "id": 780859962
  }
}

Pausing Tautulli:
{
  "stat": "ok",
  "monitor": {
    "id": 780859975
  }
}
```

### Pause specific monitors

```json
tronyx@suladan:~/uptimerobot-monitor-utility$ sudo ./uptimerobot_monitor_utility.sh -p 'Plex',780859973
Pausing Plex:
{
  "stat": "ok",
  "monitor": {
    "id": 779783111
  }
}

Pausing Radarr:
{
  "stat": "ok",
  "monitor": {
    "id": 780859973
  }
}
```

### Unpause all monitors

```json
tronyx@suladan:~/uptimerobot-monitor-utility$ sudo ./uptimerobot_monitor_utility.sh -u all
Unpausing Plex:
{
  "stat": "ok",
  "monitor": {
    "id": 779783111
  }
}

Unpausing Radarr:
{
  "stat": "ok",
  "monitor": {
    "id": 780859973
  }
}

Unpausing Sonarr:
{
  "stat": "ok",
  "monitor": {
    "id": 780859962
  }
}

Unpausing Tautulli:
{
  "stat": "ok",
  "monitor": {
    "id": 780859975
  }
}
```

### Unpause specific monitors

```json
tronyx@suladan:~/uptimerobot-monitor-utility$ sudo ./uptimerobot_monitor_utility.sh -u 'Plex',780859973
Unpausing Plex:
{
  "stat": "ok",
  "monitor": {
    "id": 779783111
  }
}

Unpausing Radarr:
{
  "stat": "ok",
  "monitor": {
    "id": 780859973
  }
}
```

### Create



### Reset



### Delete



### Discord alert for paused monitors

Using the `-a` option will check for any paused monitors and, if there are any, send an alert to the specified Discord webhook like below:

![Discord Notification](/Images/webhook.png)
