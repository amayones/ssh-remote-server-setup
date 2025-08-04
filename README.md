# SSH Remote Server Setup

Setup a basic remote Linux server and configure it to allow SSH access using two separate SSH key pairs.

## Project URL

[https://roadmap.sh/projects/ssh-remote-server-setup](https://roadmap.sh/projects/ssh-remote-server-setup)

## Features

* Create and manage **two SSH key pairs** for secure server access
* Configure `~/.ssh/authorized_keys` and proper file permissions
* Support login via **Termius** and CLI (`ssh -i`)
* Alias setup using `~/.ssh/config` for ease of access
* **Fail2Ban (optional)** installation for brute-force protection

## Usage

### Setup:

1. Launch an EC2 instance on AWS (choose **Amazon Linux** or **Ubuntu**) **without selecting a key pair**
2. Set up a Security Group rule to allow **SSH (port 22)** from your IP
3. Locally, generate two SSH keys:

   ```bash
   ssh-keygen -t ed25519 -f ~/.ssh/key1 -C "key1"
   ssh-keygen -t ed25519 -f ~/.ssh/key2 -C "key2"
   ```
4. In the AWS Console, use **EC2 Instance Connect** (browser shell) to log into the new instance
5. On the server, create `~/.ssh/authorized_keys` and paste in the contents of `key1.pub` and `key2.pub` (one per line)

### Login:

```bash
# Using terminal:
ssh -i ~/.ssh/key1 ec2-user@<EC2‑IP>
ssh -i ~/.ssh/key2 ec2-user@<EC2‑IP>

# From Termius:
- Create hosts using IP <EC2‑IP>
- Set user to ec2-user
- Upload and select key1 or key2
```

## Requirements

* Remote Linux server on AWS EC2 (Amazon Linux or Ubuntu)
* Two local SSH key pairs (`key1`, `key2`)
* Inbound SSH (port 22) allowed in EC2 Security Group
* Client tool: Terminal CLI or Termius
* EC2 Instance Connect enabled for initial key setup

## SSH Config (Optional)

To simplify login, add this to your local `~/.ssh/config`:

```ini
Host ssh-key1
    HostName <EC2‑IP>
    User ec2-user
    IdentityFile ~/.ssh/key1

Host ssh-key2
    HostName <EC2‑IP>
    User ec2-user
    IdentityFile ~/.ssh/key2
```

Then connect easily by:

```bash
ssh ssh-key1
ssh ssh-key2
```

## Stretch Goal: Fail2Ban (Optional)

Add protection against brute‑force attacks:

**On Amazon Linux:**

```bash
sudo yum update -y
sudo yum install epel-release -y
sudo yum install fail2ban -y
sudo systemctl enable --now fail2ban
```

**On Ubuntu:**

```bash
sudo apt update
sudo apt install fail2ban -y
sudo systemctl enable --now fail2ban
```

## Installation

1. Launch EC2 instance (Amazon Linux or Ubuntu) without key pair
2. Use Instance Connect to access the server and add public keys to `~/.ssh/authorized_keys`
3. Set correct permissions:

   ```bash
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```
4. Log in via both SSH keys (`key1` and `key2`) using CLI or Termius
5. (Optional) Configure `~/.ssh/config` aliases for streamlined access

## Important Note for Submission

* ✅ Only submit this `README.md`
* ❌ Do **not** push any private key to a public repository or share it anywhere

## Verification

* Successful login with both SSH keys
* Both CLI (`ssh -i`) and Termius methods work correctly
* Alias commands (`ssh ssh-key1`, `ssh ssh-key2`) function as expected
* (Optional) Fail2Ban is installed and running, if included

---

After completing this setup, you will have successfully fulfilled the roadmap.sh **SSH Remote Server Setup** project requirements and have a basic understanding of SSH-based remote access on a Linux server.
