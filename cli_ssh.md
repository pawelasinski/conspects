## 1. Connecting to a Remote Server

### Basic Connection

```zsh
ssh <user_name>@<remote_host> -p <port>
```
- If the port is the default (22), you can omit the `-p <port>` option.

### Connection Using a Specific Key

```zsh
ssh -i ~/.ssh/id_rsa_example <user_name>@<remote_host>
```
- The `-i` option specifies which private key to use.

### Executing a Command on the Remote Server

To run a command on the server and exit immediately:
```zsh
ssh <user_name>@<remote_host> <command>
```

If the command requires an interactive terminal, add the `-t` option:
```zsh
ssh -t <user_name>@<remote_host> <command>
```

### Starting a Shell in a Specific Directory

If you need to change to a particular directory upon connection:
```zsh
ssh <user_name>@<remote_host> -t 'cd /path/to/particular/directory && exec $SHELL'
```
- It is recommended to use `&&` instead of `;` to ensure the directory change is successful.
- You can replace `exec $SHELL` with `exec bash` or simply `bash`.

### Permanently Setting the Working Directory

To avoid specifying the directory every time:
1. Open your shell profile (for example, `~/.bash_profile`, `~/.profile`, or `~/.bashrc`):
```zsh
vim ~/.bash_profile
```
2. Add the following line at the end of the file:
```sh
cd /path/to/particular/directory
```
3. Save the changes and run:
```zsh
source ~/.bash_profile
```
4. Now, when you connect:
```zsh
ssh <user_name>@<remote_host>
```
   you will automatically be in the specified directory.

---

## 2. SCP (Secure Copy Protocol)

### Copying Files from Local to Remote

```zsh
scp /path/to/local/file <user_name>@<remote_host>:/path/to/remote/directory
```

### Recursive Copying of Directories

```zsh
scp -r /path/to/local/directory/with/nested/one(s) <user_name>@<remote_host>:/path/to/remote/directory
```

### Copying Files from Remote to Local

```zsh
scp <user_name>@<remote_host>:/path/to/remote/file ~/Desktop
```

### Recursive Copying of Directories from Remote

```zsh
scp -r <user_name>@<remote_host>:/path/to/remote/directory/with/nested/one(s) ~/Desktop
```

---

## 3. Redirecting STDIN/STDOUT

### Redirecting Remote Command Output to a Local File

```zsh
ssh <user_name>@<remote_host> <command> > /path/to/local/file
```

### Sending Local Command Output to a Remote Server

```zsh
<command> | scp - <user_name>@<remote_host>:/path/to/remote/file
```

### Transferring Data Between Two Remote Servers

```zsh
<command> | ssh <user_name_1>@<remote_host_1> 'scp - <user_name_2>@<remote_host_2>:/path/to/remote/file'
```

---

## 4. Fingerprint and Connection Security

When connecting to a server for the first time, you might see a message like:
```
The authenticity of host '52.307.149.244 (52.307.149.244)' can't be established.
ECDSA key fingerprint is fd:fd:d4:f9:77:fe:73:84:e1:55:00:ad:d6:6d:22:fe.
Are you sure you want to continue connecting (yes/no)? yes
```
- Type `yes` to save the server's fingerprint.
- The fingerprint ensures that the server hasn't changed or been compromised on subsequent connections.

---

## 5. Generating SSH Keys

For passwordless access, create a pair of keys:  
- **Private key** — keep it secret.  
- **Public key** — can be shared with the server.

### Creating a Key Pair

```zsh
ssh-keygen
```
Parameters:
- `-t`: encryption type (e.g., rsa).
- `-b`: key length (e.g., 4096 bits).
- `-C`: comment (e.g., `<user_name>@<remote_host>`).
- `-f`: file name (e.g., id_rsa_<new_name>).

> **Note:** The last field (e.g., `<user_name>@<remote_host>`) is used for identification only and does not affect authentication.

### Updating the Passphrase

To change the key's passphrase:
```zsh
ssh-keygen -p
```
Simply press Enter if you wish to leave it empty.

### Removing a Known Server Key

If you need to remove an outdated key:
```zsh
ssh-keygen -R <remote_host>
```

The new server key can be found on the remote machine in `/etc/ssh/ssh_host_rsa_key.pub`.

---

## 6. Uploading the Public Key to the Server

### Automatic Method (if you know the password)

```zsh
ssh-copy-id <user_name>@<remote_host>
```

### Specifying a Particular Key

```zsh
ssh-copy-id -i ~/.ssh/id_rsa.pub <user_name>@<remote_host>
```

### Using a Non-Standard Port

```zsh
ssh-copy-id '<user_name>@<remote_host> -p <port>'
```

### Manual Method

Copy the contents of `~/.ssh/id_rsa.pub` and paste it into the `~/.ssh/authorized_keys` file on the server.  
For example, on macOS you can run:
```zsh
cat ~/.ssh/id_rsa.pub | pbcopy
```

---

## 7. Disabling Password Authentication

For improved security, it is recommended to allow access only by key.

1. Open the SSH server configuration file:
```zsh
vim /etc/ssh/sshd_config
```
2. Change the following parameters:
   - Change `PasswordAuthentication yes` to `no`
   - Change `PermitRootLogin yes` to `no`
   - Optionally, restrict access using:
```text
AllowUsers <user_name> [root]
```
3. Restart the SSH server:
```zsh
sudo service ssh restart
```

---

## 8. SSH Agent

The ssh-agent helps:
- Store keys in memory so you don’t have to enter the passphrase every time.
- Automatically select the correct key for a connection.

### Starting the ssh-agent

```zsh
eval '$(ssh-agent -s)'
```

### Adding a Key to the Agent

```zsh
ssh-add ~/.ssh/id_rsa
```
If no path is provided, the agent will try adding keys from:
- `~/.ssh/id_rsa`
- `~/.ssh/id_dsa`
- `~/.ssh/id_ecdsa`
- `~/.ssh/id_ed25519`
- `~/.ssh/identity`

### Listing Added Keys

```zsh
ssh-add -L
```

### Removing Keys

- Remove a specific key:
  ```zsh
  ssh-add -d ~/.ssh/id_rsa
  ```
- Remove all keys:
  ```zsh
  ssh-add -D
  ```

### Adding a Key with a Time Limit

To add a key for 1 hour (3600 seconds):
```zsh
ssh-add -t 3600 ~/.ssh/id_rsa_temp
```

> **Note:** The ssh-agent is tied to the current session.

---

## 9. Port Forwarding

### Agent Forwarding

Allows you to forward keys from one server to another:
```zsh
ssh -A <user_name_1>@<remote_host_1> ssh <user_name_2>@<remote_host_2>
```

### Reverse Port Forwarding

Redirects a port from the remote server to your local machine:
```zsh
ssh -R <remote_host>:80:localhost:8000 <user_name>@<remote_host>
```

> **Note:** Port 80 is privileged. If you don’t have root privileges, use a port above 1024.

### Local Port Forwarding

Redirects a local port to the remote server:
```zsh
ssh -L 8000:localhost:8000 <user_name>@<remote_host>
```

After connecting, open in your browser:
```
http://localhost:8000
```

### Dynamic Port Forwarding (SOCKS Proxy)

```zsh
ssh -D 1080 <user_name>@<remote_host>
```

#### Testing Network Throughput with iperf3

Installation:
- On macOS:
  ```zsh
  brew install iperf3
  ```
- On Linux:
  ```zsh
  sudo apt install iperf3
  ```

Start the server:
```zsh
iperf3 -s
```

Run the client:
```zsh
iperf3 -c localhost -p 1080
```
This command initiates a test, sending data from the client to the server and measuring throughput, latency, and potential packet loss.

### Checking Remote Server Availability

```zsh
ssh -T <user_name>@<remote_host>
```

---

## 10. SSH Aliases

To simplify connections, you can set up aliases in the configuration file.

1. Open the configuration file:
```zsh
vim ~/.ssh/config
```
2. Add the following configuration:
```text
Host <alias>
	Hostname <remote_host>
	User <user_name>
	Port <port>
	IdentityFile ~/.ssh/id_rsa_example
	IdentitiesOnly yes
```
3. Now you can connect using the alias:
```zsh
ssh <alias>
```