---
icon: rectangle-terminal
description: scheduler but a hard one
---

# cron tabs (schedule)

Users may want to schedule a certain action or task to take place after the system has booted. Take, for example, running commands, backing up files, or launching your favourite programs on, such as Spotify or Google Chrome.

to edit cron tabs enter `crontab -e`, and this file opens up:

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

A crontab is simply a special file with formatting that is recognised by the `cron` process to execute each line step-by-step. Crontabs require 6 specific values:

<table><thead><tr><th width="361">Value</th><th>Description</th></tr></thead><tbody><tr><td>MIN</td><td>What minute to execute at</td></tr><tr><td>HOUR</td><td>What hour to execute at</td></tr><tr><td>DOM</td><td>What day of the month to execute at</td></tr><tr><td>MON</td><td>What month of the year to execute at</td></tr><tr><td>DOW</td><td>What day of the week to execute at</td></tr><tr><td>USER</td><td>What user the command will run as</td></tr><tr><td>CMD</td><td>The actual command that will be executed.</td></tr></tbody></table>

as shown in the image the format for scheduling a command is:

`[MIN] [HOUR] [DOM] [MON] [DOW] [CMD]`

e.g. `0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/` is executed every 12 hours.

#### `m h dom mon dow user command`

`17 * 1 * * * root cd / && run-parts --report /etc/cron.hourly`

[Crontab Generator](https://crontab-generator.org/) and [Cron Guru](https://crontab.guru/) can help you create it.

