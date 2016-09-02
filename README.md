# git-ssh-add-key-with-permissions
add existing ssh key to git/github and fix public key permissions...

assuptions:
- you have `backup of previous ~/.ssh` folder
- you have installed git` and ssh-client
- your unix user ang group is: `mak`
- your git user is: `daggerok`

```fish
# 1: copy your key from backup
mkdir ~/.ssh
cp -Rf ./backup/id_{dsa,rsa}* ~/.ssh/

# 2: create ssh config within keys configuration
cat ~/.ssh/config
...
Host github.com
  HostName github.com
  User daggerok
  IdentityFile ~/.ssh/id_dsa      # or: IdentityFile ~/.ssh/id_rsa
  PubkeyAcceptedKeyTypes +ssh-dss # or: KexAlgorithms +diffie-hellman-group1-sha1

# 3: setup permissions
chmod 700 ~/.ssh
chown -R mak:mak
chmod +r ~/.ssh/id_*.pub
chmod 600 ~/.ssh/id_{rsa,dsa}
chmod 600 ~/.ssh/authorized_keys

# 4: add key to ssh
ssh-add ~/.ssh/id_{rsa,dsa}

# 5: verify
ls -lah ~/.ssh/
total 100K
drwx------  2 mak mak 4,0K Sep  2 12:07 ./
drwx------ 45 mak mak  12K Sep  2 12:07 ../
-rw-------  1 mak mak  757 Aug 31 21:50 authorized_keys
-rw-------  1 mak mak  924 Sep  2 12:07 config
-rw-------  1 mak mak  672 Aug 31 21:48 id_dsa
-rw-r--r--  1 mak mak  589 Aug 31 21:48 id_dsa.pub
-rw-------  1 mak mak 3,2K Aug 31 21:47 id_rsa
-rw-r--r--  1 mak mak  757 Aug 31 21:47 id_rsa.pub
```
