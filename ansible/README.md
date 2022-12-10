---
description: Ansible Ad-Hoc command and playbook options
---

# Ansible

## Ad-Hoc Commands

Syntax: **`ansible -i [inventory] [server group] -m [module] -u [user]`**

| Options | Description                                                                                                                   |
| :-----: | ----------------------------------------------------------------------------------------------------------------------------- |
|    -m   | Specify a module Eg:`-m command -a ping`                                                                                      |
|    -a   | Specify arguments for a module or use command module by default                                                               |
|    -f   | Specify number of `forks` ansible uses. 1 will send command to one server at a time in order hosts are arranged in inventory. |
|    -b   | Becomes a different user `root` by default. `--become` is used to supplement `sudo` when not operating with Linux systems.    |
|    -K   | Will ask the password for root user when used with `-b`. Alternative:`--ask-become-pass`                                      |
|    -u   | Specify a username Eg: `-u mhs`                                                                                               |
| --limit | Limits the host, command will not execute on the ip marked with limit. Also, supports regex for ip. Eg: `"ip1,ip2"`           |

{% hint style="info" %}
If module is not specified with -m **ansible defaults to the command module** for ad-hoc commands

* **Command module doesn't support `|` & redirection**, use `-m shell` module

Eg: `ansible all -m shell -a "command | grep text"`
{% endhint %}

### Polling & Background jobs <a href="#backgroundtasks" id="backgroundtasks"></a>

**`-p 0`** Specify a polling time in seconds. Ansible will run the command in the background and check in the specified time if that command has executed.`0 = polling disabled`

* Eg: **`ansible all -B 1800 -P 60 -a "/usr/bin/long_running_operation --do-stuff"`**
* This job runs for 30 minutes with polls every minute

`-B` Specify time limit for the job to run in background.

* Eg: **`ansible all -B 3600 -P 0 -a "/usr/bin/long_running_operation --do-stuff"`**
* This job runs for 3600 seconds without polling

> **Note:** A result file is printed in output which gives a job ID. Check the result of the job using **`ansible all -m async_status -a "jid=jobid"`**

* Job ID is unique to each server.

## Playbook Options <a href="#playbookoptions" id="playbookoptions"></a>

**Syntax: `ansible-playbook -i [inventory] [Options]`**

|         Options        | Description                                                                                                    |
| :--------------------: | -------------------------------------------------------------------------------------------------------------- |
|     --syntax-check     | perform a syntax check on the playbook, but do not execute it                                                  |
|        --version       | show program’s version number, config file location, module search path, module location & executable location |
|       -C, --check      | don’t make any changes; instead, try to predict some of the changes that may occur                             |
|       -D, --diff       | when changing (small) files and templates, show the differences in those files; works great with –check        |
|      -b, --become      | run operations with become (does not imply password prompting)                                                 |
|  -K, --ask-become-pass | ask for privilege escalation password                                                                          |
|     -T , --timeout     | override the connection timeout in seconds (default=10)                                                        |
|    -e, --extra-vars    | set additional variables as key=value or YAML/JSON, if filename prepend with @                                 |
|      -f , --forks      | specify number of parallel processes to use (default=5)                                                        |
|      --flush-cache     | Clear the fact cache for every host in inventory                                                               |
|    --force-handlers    | Run handlers even if task fails                                                                                |
|      --list-hosts      | List matching hosts                                                                                            |
|      --list-tasks      | List all tasks that would be executed                                                                          |
|      --private-key     | Use this file to authenticate the connection                                                                   |
|         --step         | One step at a time, confirm each task before running                                                           |
|      -v, --verbose     | verbose mode (-vvv for more, -vvvv to enable connection debugging)                                             |
|    --ask-vault-pass    | Ask for vault password                                                                                         |
|      --become-user     | Run operations as this user (default=root)                                                                     |
|     --become-method    | privilege escalation method to use (default=sudo), use _ansible-doc -t become -l_ to list valid choices.       |
| --become-password-file | Pass a password file for the playbook                                                                          |
|       --vault-id       | the vault identity to use                                                                                      |
|    --vault-pass-file   | vault password file                                                                                            |
