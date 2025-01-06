# 1p-ssh
Linux CLI SSH tool for your keys stored in 1Password

This script is made to work with the 1Password CLI, which somehow forces you to use the UI, in particular if you want to use the 1Password SSH Agent.
This script helps you using your SSH keys stored in 1Password, without the need to use the UI.
You can list, extract a key, or also add a key or all of them, in your OpenSSH agent.

Also, as 1Password restricts the import of some old keys in the "SSH Key" category, I made it possible to use the "Secure Note" one.
For this specific use case, you need to have "[SSH" in the item title (i.e. "yourmailaddress@domain.tld [SSH Ed25519]"), in addition of these fields:
* public_key (required)
* fingerprint
* private_key (required)
* key_type

When you list the keys, it will take all your keys in the "SSH Key" category, and only the ones matching "[SSH" in the "Secure Note" category.

## Installation
First, as dependencies, you'll need to have these tools installed:
* 1Password CLI (https://developer.1password.com/docs/cli/get-started/)
* openssh-client
* jq
* This script (actually, you can put it in /usr/local/bin. Then, you'll be able to call it with 1p-ssh)

## Usage
Usage: 1p-ssh *action* [--id *ID*] (--all) [--field *field*]

Actions:
* list: Lists all available SSH keys in your 1Password vaults
* add: Adds a unique (with --id *ID*) or all (with --all) your SSH key(s) in your SSH agent
* extract: Extracts a field from a specific key (with --id *ID* --field *field*). The field must be one of:
  * public_key
  * fingerprint
  * private_key
  * key_type
* lock | logout | signout: Kills the SSH agent and signs out of 1Password

### Important
If your 1Password session is opened in your shell before you use the script, everything will be easier to use. It's the same story with your ssh-agent.
Otherwise, the script will open your 1Password session and launch your SSH agent, but in that case, your required environment variables won't be set once the script is done.
For this specific purpose, you will have a $HOME/.op-session file filled with the needed variables. All you'll have to do is sourcing it, as in the following example:

```
1p-ssh add --all
source $HOME/.op-session
ssh-add -l
```
