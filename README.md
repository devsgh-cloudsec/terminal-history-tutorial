# Automatically Save Terminal Command History with Timestamps on Ubuntu, Zsh, Docker, and Remote Servers

Keeping a detailed, timestamped history of your terminal commands can help track your workflow, audit activities, or maintain logs. This tutorial shows how to **automatically save command history with timestamps to a file** on different environments:

* GNOME Terminal (Bash on Ubuntu)
* Zsh Shell
* Docker Containers
* Remote Servers via SSH

---

## GNOME Terminal (Bash on Ubuntu)

### Step 1: Open and Edit Your `.bashrc`

```bash
nano ~/.bashrc
```

*Opens the Bash configuration file for editing.*

---

### Step 2: Enable Timestamps in Command History

Add this line at the bottom:

```bash
HISTTIMEFORMAT="%F %T "
```

*Adds date and time format to history entries (e.g., 2025-07-19 15:24:00).*

---

### Step 3: Automatically Save History on Terminal Exit

Add the following block below the previous line:

```bash
function save_history_on_exit() {
    history -a
    history -r
    history >> ~/Documents/terminal_history_with_time.txt
}
trap save_history_on_exit EXIT
```

*Appends your full history with timestamps to a file in the Documents folder every time you close the terminal.*

---

### Step 4: Save and Apply Changes

* Press `Ctrl + O` to save
* Press `Enter` to confirm
* Press `Ctrl + X` to exit the editor

Then reload the configuration:

```bash
source ~/.bashrc
```

*Applies the new settings immediately.*

---

## Zsh Shell

### Step 1: Open and Edit Your `.zshrc`

```bash
nano ~/.zshrc
```

*Opens the Zsh configuration file.*

---

### Step 2: Enable Timestamps

Add this line:

```bash
export HISTTIMEFORMAT="%F %T "
```

*Enables date and time stamps in the Zsh history.*

---

### Step 3: Automatically Save History on Terminal Exit

Add this block:

```zsh
function save_zsh_history_on_exit() {
    fc -A
    history >> ~/Documents/zsh_terminal_history.txt
}
TRAPEXIT() {
    save_zsh_history_on_exit
}
```

*Ensures your Zsh history is saved with timestamps in the Documents folder when the terminal session ends.*

---

### Step 4: Apply the Changes

```bash
source ~/.zshrc
```

*Reloads your Zsh configuration.*

---

## Docker Containers

### Step 1: Run a Docker Container with a Mounted Volume

```bash
docker run -it -v $(pwd)/logs:/logs ubuntu /bin/bash
```

*Starts an Ubuntu container with a folder mounted from the host machine for saving logs.*

---

### Step 2: Configure Bash History Inside the Container

Run inside the container:

```bash
echo 'HISTTIMEFORMAT="%F %T "' >> ~/.bashrc
echo 'function save_history_on_exit() { history -a; history >> /logs/container_history.txt; }; trap save_history_on_exit EXIT' >> ~/.bashrc
source ~/.bashrc
```

*Sets up timestamped history and configures automatic saving to the mounted logs folder when the container session ends.*

---

## Remote Servers via SSH

### Step 1: Connect to Your Remote Server

```bash
ssh user@remote-ip
```

*Starts an SSH session with your remote machine.*

---

### Step 2: Edit the Shell Configuration on the Remote Server

For Bash, open `.bashrc`:

```bash
nano ~/.bashrc
```

Add:

```bash
HISTTIMEFORMAT="%F %T "
function save_history_on_exit() {
    history -a
    history >> ~/remote_history.txt
}
trap save_history_on_exit EXIT
```

*Enables timestamps and sets up automatic saving of the history to a file when you exit the SSH session.*

---

### Step 3: Apply Changes on Remote

```bash
source ~/.bashrc
```

*Reloads your shell configuration on the remote server.*

---

## Summary Table

| Environment           | Config File                | Key Commands Added                            | Saved History Location                       |
| --------------------- | -------------------------- | --------------------------------------------- | -------------------------------------------- |
| GNOME Terminal (Bash) | `~/.bashrc`                | `HISTTIMEFORMAT`, `save_history_on_exit` func | `~/Documents/terminal_history_with_time.txt` |
| Zsh Shell             | `~/.zshrc`                 | `HISTTIMEFORMAT`, `save_zsh_history_on_exit`  | `~/Documents/zsh_terminal_history.txt`       |
| Docker Container      | `~/.bashrc` (in container) | `HISTTIMEFORMAT`, `save_history_on_exit` func | Mounted folder `/logs/container_history.txt` |
| Remote Server (SSH)   | `~/.bashrc` or `~/.zshrc`  | Same as above                                 | `~/remote_history.txt`                       |

---
