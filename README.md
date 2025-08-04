# Linux SSH Setup with AWS + Termius

A step-by-step guide to set up a remote Linux server on AWS and configure it to allow SSH connections using two different SSH key pairs.

## Project URL

https://roadmap.sh/projects/ssh-remote-server-setup

## Features

* **Multi-key SSH Access** – Login using two separate SSH key pairs
* **Secure Configuration** – Proper key permissions and user access
* **Terminal & Termius Compatible** – SSH from both CLI and GUI
* **Alias Support** – Custom SSH aliases with `~/.ssh/config`
* **Fail2Ban (Optional)** – Brute force protection

## Usage

### Setup:

1. Launch an EC2 Ubuntu instance with **no key pair**
2. Enable port **22 (SSH)** in the Security Group
3. Generate two SSH key pairs on your local machine:

   ```bash
   ssh-keygen -t ed25519 -f ~/.ssh/key1
   ssh-keygen -t ed25519 -f ~/.ssh/key2
   ```
4. Use **EC2 Instance Connect** to SSH into the server
5. Add both public keys (`key1.pub` and `key2.pub`) into:

   ```bash
   ~/.ssh/authorized_keys
   ```

### Login:

```bash
ssh -i ~/.ssh/key1 ubuntu@<EC2-IP>
ssh -i ~/.ssh/key2 ubuntu@<EC2-IP>
```

## Requirements

* AWS EC2 instance (Ubuntu, Free Tier)
* Two local SSH key pairs (`key1`, `key2`)
* EC2 Security Group allowing SSH (port 22)
* Terminal or Termius client
* EC2 Instance Connect (browser-based)

## SSH Config (Optional)

Add this to `~/.ssh/config` for easier login:

```ini
Host aws-key1
    HostName <EC2-IP>
    User ubuntu
    IdentityFile ~/.ssh/key1

Host aws-key2
    HostName <EC2-IP>
    User ubuntu
    IdentityFile ~/.ssh/key2
```

Now you can SSH using:

```bash
ssh aws-key1
ssh aws-key2
```

## Stretch Goal: Fail2Ban

(Optional) Protect the server from SSH brute force attacks:

```bash
sudo apt update
sudo apt install fail2ban -y
sudo systemctl enable --now fail2ban
```

## Installation

1. Launch EC2 instance (Ubuntu)

2. Add both SSH keys to `~/.ssh/authorized_keys` on the instance

3. Adjust permissions:

   ```bash
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```

4. SSH into server using both key pairs to confirm setup

5. (Optional) Set up `.ssh/config` file for aliases

## Verification

✅ Login with both keys successfully
✅ Termius configuration works
✅ Fail2Ban is running (if installed)
✅ No private key pushed or exposed
