## Problem :x:
I have two Github accounts: *user1* (personal) and *user2* (for work).
I want to use both accounts on same computer (without typing password everytime, when doing git push or pull).

## Solution :white_check_mark:
Use ssh keys and define host aliases in ssh config file (each alias for an account). AND use diffrent .gitconfig user credentials depending on what directory you're currently working in. 

## How? 🌟 


### 1. Change directory to root ~

Make sure your current directory is root. In root you should already have a .ssh folder by default. Since it's a dot folder you need to do ```ls -a``` in order to see it. If you don't have any, you can create one with ```mkdir .ssh```.


### 2. Change directory to .ssh and run these commands: 

```
$ ssh-keygen -t rsa -b 8192 -C "user1@email.com" -f "github-user1"
$ ssh-keygen -t rsa -b 8192 -C "user2@email.com" -f "github-user2"

-C flag adds a comment to help identify the key.
-f flag specifies the file name for the key pair.
```

Now you should have public and private keys for both your accounts in your ```~/.ssh/``` directory.


### 3. Add the SSH keys to your SSH-agent

```
$ ssh-add --apple-use-keychain ~/.ssh/github-user1
$ ssh-add --apple-use-keychain ~/.ssh/github-user2
```

To see all entries added, use ```ssh-add -l```


### 4. Add a config file to the ```~/.ssh/``` directory.

If you dont have one, run ```touch config```. Add these lines to your config file: 

```
Host github.com-user1
HostName github.com
User git
UseKeychain yes
IdentityFile ~/.ssh/github-user1

Host github.com-user2
HostName github.com
User git
UseKeychain yes
IdentityFile ~/.ssh/github-user2
```


### 5. Import all the public keys on the corresponding GitHub accounts.

Sign in to corresponding github account and add the public ssh key in Settings -> SSH and GPL Keys. Copy the correct keys by running 

```
$ pbcopy < ~/.ssh/github-user1.pub
$ pbcopy < ~/.ssh/github-user2.pub
```


### 6. Now we're ready to test out connections by running 
```
ssh -T git@github.com-user1
``` 
```
ssh -T git@github.com-user2
```

We should get a response similar to this: 
```
Hi user1! You've successfully authenticated, but GitHub does not provide shell access.
```


### 7. Change .gitconfig according to current directory.

if you want to be sure before committing, you can check current gitconfig by running ```git config --list```

Note that ```git config --list``` does display both
global and local gitconfig. You local can be found in ```.git/config```

Add this to your .gitconfig in root: 

```
[includeIf "gitdir:~/code/private/"]
  path = .gitconfig-private
[includeIf "gitdir:~/code/work"]
  path = .gitconfig-work
```

Now create the ```.gitconfig-private``` and ```.gitconfig-work``` files in root (```touch .gitconfig-private``` and ```touch .gitconfig-work```)

And add the following to respective file: 

```
[user]
 name = user1
 email = user1@email.com
```

And thats is! 🏁

Got stuck or got any feedback? Please leave a comment! ✍️

Considered this helpfull? Please star the repo 🌟



