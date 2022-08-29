# git-signing-demo
a repo to demonstrate signing git commits with GPG and SSH keys

# GPG

## On your local machine

1. Install GPG, if you haven't already:

### WSL2 / Ubuntu
```console
$ sudo apt install git gpg
```
### macOS
You will need to [install homebrew](https://brew.sh/) if you haven't already.
```console
$ brew install git gpg
```

### Windows
Download and install git from [the official Git website](https://git-scm.com/).
Default settings should be OK for most. This should install git, the git bash shell
and GPG.

Optional: if you are not comfortable using nano or vim as your text editor, please
consider [installing Notepad++](https://notepad-plus-plus.org/downloads/) and
selecting it as your preferred editor during Git installation (you can download
and install Notepad++ while the Git installer is open, since it doesn't use MSI
and will allow you to run two installers concurrently).

Also note that for the purpose of this exercise, we will assume that you will be using 
the git bash shell which will come with the Git for Windows install package. If you
would like to use PowerShell or the Windows CMD shell, during installation, you will
need to select the option "Use Git and optional Unix tools from the Command Prompt".
Please note, however, that this will override some Windows commands with their UNIX
counterparts (e.g. find), and as such is only recommended for advanced users.

2. Create a GPG and/or SSH key, if you haven't already.

```console
$ gpg --quick-generate-key <your email address>

$ gpg --list-secret-keys --keyid-format long
/Users/alex/.gnupg/pubring.kbx
------------------------------
sec   ed25519/DEADBEEFDEADBEEF 1970-01-01 [SC]
              ^^^^^^^^^^^^^^^^ This is the part you need
      Key fingerprint = DEAD BEEF DEAD BEEF DEAD  BEEF DEAD BEEF DEAD BEEF
uid                 [ultimate] Alex Tremblay (Main Key) <alex.tremblay@utoronto.ca>
ssb   cv25519/AACA86644BAF7C57 1970-01-01 [E]

$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/tchakhma/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/tchakhma/.ssh/id_rsa
Your public key has been saved in /c/Users/tchakhma/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:QVjaPJKpjT8L7D5F8NcJebL9RmVEyWwZb3fOhFH88og tchakhma@AMD64
The key's randomart image is:
+---[RSA 3072]----+
|       oo.   ==*.|
|    . .B+ .   O+.|
|     o= =B . +. B|
|     +o.oo+ . .=+|
|    o...S  o . +o|
|   . ..     E . .|
|    o.o    .     |
|   ... o         |
|   .o..          |
+----[SHA256]-----+

```

3. Configure git to use your GPG or SSH key

```console
# Tell git which GPG key to use. 
$ git config --global user.signingkey DEADBEEFDEADBEEF

# For SSH keys, use the path to the secret key, e.g. ~/.ssh/id_rsa
$ git config --global user.signingkey ~/.ssh/id_rsa
# You also need to tell git that your key is an SSH (not GPG) key:
$ git config --global gpg.format ssh

# Tell git to always auto-sign commits
$ git config --global commit.gpgsign true

# Pro tip: to edit your git settings all at once in a text editor:
$ git config --global --edit
```

Here's what a sample git config file looks like (~/.gitconfig):
```gitconfig
[user]
	email = your.email@example.com
	name = Your Name
	# GPG key signing
	#signingkey = 0xDEADBEEFDEADBEEF
	# SSH key signing
	signingkey = ~/.ssh/id_rsa

[gpg]
	# SSH commit signing; comment out for GPG signing
	format = ssh
	
[commit]
	# Comment out if you don't want to sign every commit by default
	# You can invoke git commit with the -S parameter to sign manually
	# when this flag is unset or is set to false.
	gpgsign = true
```

4. Print out your GPG/SSH public key so you can copy /paste it into Github / Gitlab
```console
$ gpg --armor --export DEADBEEFDEADBEEF
# Be careful not to paste/print the SSH secret key! Remember, you will need to add
# SSH keys twice: once for authentication (so you can push and pull using SSH) and
# signing (so GitHub knows to use your SSH key for commit signature verification).
$ cat ~/.ssh/id_rsa.pub 
```

## On Github / Gitlab

### Github:

1. Click on your user profile in top-right corner
2. Go to Settings > SSH and GPG keys
3. Click on "New GPG key" or "New SSH key" (note that for SSH keys, you will need to
   add them twice; once for authentication, and once for signing).
4. Paste the public key you printed out previously into the text box
5. (Optionally) enable vigilant mode

### Gitlab:

1. Click on your user profile in top-right corner
2. Go to Preferences > GPG keys
3. Paste the public key you printed out previously into the text box

Note: Gitlab does not yet support using SSH keys for commit signing. To keep track of
this feature request, see [Gitlab issue 343879](https://gitlab.com/gitlab-org/gitlab/-/issues/343879).