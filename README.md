# git-signing-demo
a repo to demonstrate signing git commits with GPG and SSH keys

# GPG

## On your local machine

1. Install GPG, if you haven't already:

```console
# WSL2 / Ubuntu
$ sudo apt install git gpg

# Macos
$ brew install git gpg
```

2. create a GPG key, if you haven't already

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
```

3. configure git to use your gpg key

```console
# Tell git which GPG key to use
$ git config --global user.signingkey DEADBEEFDEADBEEF

# Tell git to always auto-sign commits
$ git config --global commit.gpgsign true
```

4. Print out your GPG public key so you can copy /paste it into Github / Gitlab
```console
$ gpg --armor --export DEADBEEFDEADBEEF
```

## On Github / Gitlab

### Github:

1. click on your user profile in top-right corner
2. Go to Settings > SSH and GPG keys
3. Click on "New GPG key"
4. paste the public key you printed out previously into the text box
5. (Optionally) enable vigilant mode

### Gitlab:

1. click on your user profile in top-right corner
2. Go to Preferences > GPG keys
3. paste the public key you printed out previously into the text box