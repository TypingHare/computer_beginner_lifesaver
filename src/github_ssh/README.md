# How to Set up GitHub SSH

**SSH (Secure Shell)** is a cryptographic network protocol used to access and manage networked devices over an unsecured network securely. It provides a secure way to log into remote systems, then execute commands, transfer files, and manage servers.

By setting up SSH on GitHub, we will be free from the validation process every time we access the GitHub server, such as when pushing commits.

## Create SSH Key Pairs on a Personal Computer

Open the terminal and enter the **SSH directory** (`~/.ssh`. where `~` refers to the home directory):

```shell
$ mkdir -p ~/.ssh & cd ~/.ssh
```

Generate the SSH key pair (**ed25519** is one of the safest **asymmetric encryption** methods at the moment):

~~~shell
$ ssh-keygen -t ed25519 -C "comment"
~~~

Here, the last argument is a string of a one-sentence comment, which is particularly your email address. This command creates two files in the SSH directory:

* `id_ed25519`: This is your **private key** (or **secret key**). You should keep it secure on your computer and never share it with anyone.
* `id_ed25519.pub`: This is your **public key**. You can share it with others and add it to services like GitHub, GitLab, or remote servers.

## Adding SSH Keys to GitHub

Open [GitHub > Settings > SSH and GPG keys](https://github.com/settings/keys) and click the `New SSH key` button on the top-right corner. Copy and paste the contents of the public key to the `Key` text field and add a title, a descriptive name indicating the device having the private key, for it. Finally, click `Add SSH key` to confirm.

Later, when you clone a repository that you want to contribute to, use the **SSH URL** instead of the **HTTP URL**, for example:

```shell
# ✅ Recommended: SSH URL (allows read/write access)
git clone git@github.com:TypingHare/burrow.git

# ⚠️ Not Recommended: HTTPS URL (read-only access)
git clone https://github.com/TypingHare/burrow
```

