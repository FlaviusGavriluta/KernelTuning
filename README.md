# Kernel Tuning

## Story

Before you put your Linux server in the Wild Wild Web you want to make sure that SSH will always be on after system reboots and you also want to tighten the security by configuring the Linux kernel firewall. How awesome would it be if you could also disable the ping response to make it stealthier... 

## What are you going to learn?

- Identify parameters available to `sysctl`.
- Manipulating settings using the `sysctl` utility.
- Preserving `sysctl` changes between reboots.
- How to list and manage services using `systemctl`.
- What are cron jobs and how to use them.
- What is Linux kernel firewall and how to create chains using `iptables`

## Tasks

1. Manage kernel parameters using `sysctl`. Disable ICMP ping for IPv4. Make the change persistent and test it after a system reboot.
    - ICMP ping for IPv4 is disabled. The change persists after system reboot.
    - Pinging the IPv4 address is not possible.

2. Manage systemd services with `systemctl`.
    - Active/all/inactive systemctl units are listed. The `SSH` service is identified.
    - `SSH` service is enabled (will start on boot).
    - A new service like nginx is installed. The new service is started and enabled using `systemctl` commands.

3. Customize the welcome message (motd)
    - Executable permissions are disabled on all scripts in the `/etc/update-motd.d` directory.
    - A new custom welcome message is added.     

4. Create a cron job that will execute once a day at 8:00 AM. It should fetch the weather forecast from wttr.in/London (or choose your location) and write it to the `/etc/motd` file. This way it will be displayed everytime you log in.
    - A new cron job scheduled to run every day at 8:00 AM is created. The user sees the weather forecast as a `motd` message after log in.

5. Harden your iptables input chain by allowing only SSH.
    - The `INPUT` iptables chain is modified to accept connections only on the SSH port and drop all other input traffic. The change persists after system reboot.

## General requirements

None

## Hints

- Please use a VirtualBox VM for the tasks.
- Explore the different options available for `sysctl` using the man page. List all the paramaters.
- After you list the parameters, explore the parameters starting with `net.ipv4.` to find the one for ignoring ICMP echo requests.
- To list only the sysctl parameters that have ipv4 in their names use the command `sysctl -a -r ipv4`.
- To set a parameter permanently, you need to save the settings to `/etc/sysctl.conf` or in a configuration file in the `/etc/sysctl.d` directory.
- If you change the file `/etc/sysctl.conf` or any file in `/etc/sysctl.d` directory you can load the changes immediately with the command `sysctl -p` or you can reboot the OS and it will load them upon booting.
- Try pinging localhost. It will still be pingable, because it has an IPv6 address associated to it in the `/etc/hosts` file. You won't be able to ping your IPv4 localhost (127.0.0.1) address though.
- Please note that while `ping` is often being used to check reachability of a host on the network, it is sometimes the case that ICMP echo requests are filtered by firewalls or the kernel parameter to ignore them is set so if a host doesn't respond to ICMP echo requests it doesn't mean it is not reachable. 
- Use the command `curl wttr.in/London > /etc/motd` to save the weather forecast to the file `/etc/motd` in your cron job.
- You can change the default text editor used by various programs, such as crontab. To change it you can add the `export EDITOR='program'` to your `.bashrc` file, where 'program' can be your favourite cli editor or if you work in a Desktop environment it can be your favourite GUI editor (for example `code -w` for VS Code).
- Here you can check your crontab parameters if it really is that you want: [crontab guru](https://crontab.guru/).
- To see the current date/time use the commands `date`  or `timedatectl`, to set the correct time zone use `sudo dpkg-reconfigure tzdata`.
- Make sure you add the `ACCEPT` rule for `SSH` before you add the `DROP` rule to avoid locking yourself out of the server. Check the tutorial from the background materials for reference.

## Background materials

- <i class="far fa-exclamation"></i> [Linux Ping Command Tutorial With Examples](https://phoenixnap.com/kb/linux-ping-command-examples)
- <i class="far fa-exclamation"></i> [sysctl](https://wiki.archlinux.org/index.php/sysctl)
- <i class="far fa-exclamation"></i> [How To Use Systemctl to Manage Systemd Services and Units](https://digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units)
- <i class="far fa-exclamation"></i> [cron](https://en.wikipedia.org/wiki/Cron)
- <i class="far fa-exclamation"></i> [Understanding Crontab in Linux With Examples](https://linuxhandbook.com/crontab/)
- <i class="far fa-video"></i> [Linux/Mac Tutorial: Cron Jobs - How to Schedule Commands with crontab](https://www.youtube.com/watch?v=QZJ1drMQz1A)
- <i class="far fa-exclamation"></i> [iptables](https://en.wikipedia.org/wiki/Iptables)
- <i class="far fa-exclamation"></i> [Iptables Tutorial – Securing Ubuntu VPS with Linux Firewall](https://www.hostinger.com/tutorials/iptables-tutorial)
- <i class="far fa-exclamation"></i> [How to change welcome message (motd) on Ubuntu 18.04 server](https://linuxconfig.org/how-to-change-welcome-message-motd-on-ubuntu-18-04-server)

