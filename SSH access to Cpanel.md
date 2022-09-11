## Problem :x:

I don't know how to clone my github repository with ssh in cPanel. 

I want to continously deploy my changes on my main branch. 

## Solution :white_check_mark:
This guide requiers you to already have SSH private and public keys set up to your github repository. 


## 1. Clone repository in cpanel from github. 

1.1 Upload your PRIVATE key to the ~/.ssh directory of your cPanel account. This can be done via the File Manager icon in cPanel, FTP, SFTP, or you can copy and paste it in via SSH if you know how.

NOTE: Keys that were generated with a passphrase are not compatible with cPanel Git Version Control. Generate a new set of keys without a passphrase if needed.

 1.2. Login to your cPanel server via SSH as your cPanel user, or use the Terminal icon in the cPanel account to access the command line.

 1.3 Run the following command to check the permissions and ownership of the .ssh directory
NOTE: Pay special attention to the example output. Notice that the Uid and Gid are the cPanel username, and the permissions are set to ```0700```.

```stat ~/.ssh | grep Access -m 1```

And you should get this response: 

```Access: (0700/drwx------) Uid: ( 1002/cpanelusername) Gid: ( 1008/cpanelusername)```


 1.4 If the permissions and ownership do not follow the example above, you may use the following commands to fix this:

```
chmod 0700 ~/.ssh
chown cpanelusername:cpanelusername
```


1.5 Run the following command to check the permissions and ownership of your /.ssh/my_key directory. Ensure the permissions are set to ```0600```, and the Uid and Gid ownership are set to the cPanel user:

```stat ~/.ssh/my_key | grep Access -m 1```

And you should get this response: 

```Access: (0600/-rw-------) Uid: ( 1002/cpanelusername) Gid: ( 1008/cpanelusername)```





#### Create a config file: 

```
touch ~/.ssh/config
```

Add this to your file: 

```
Host *
    IdentityFile ~/.ssh/my_key
```

#### Ensure that the permissions and ownership of ~/.ssh/config match the following: ```0600``` and owned by the cPanel user:

```stat ~/.ssh/config | grep Access -m 1```

And you sould get this response: 

```Access: (0600/-rw-------) Uid: ( 1002/cpanelusername) Gid: ( 1008/cpanelusername)```




## 2. Add a repository in cpanel git verision control

go to cpanel and enter Git Version Control.

Click Create to add a repository

<img width="677" alt="image" src="https://user-images.githubusercontent.com/47527390/189514336-fe4f7f32-9b28-4369-9cb4-371aee160c07.png">

 Add the path to your repo and give it a name. 
    

## 2. Add a .cpanel.yml file to your project

```
---
deployment: 
     tasks: 
          - export DEPLOYPATH=/home/<username>/<public_html>/
          - /bin/cp -R public/* $DEPLOYPATH
          
```

* Line 1 is the beginning of a YAML file.

* Lines 2 and 3 add deployment and tasks keys.

* Line 4 begins a list of BASH commands that run during deployment.

### 2. Add remote to you local repo

When repository is added, run following command on you local machine: 

```git add remote cpanel ssh://user@example.tld/path/to/repo```

Find the url in cpanel: Git Version control -> "arrow down" on repository listed -> copy "clone URL".

Check all your remotes by using ```git remote -v```

### 3. Now your deploy routine should look like this: 

```
gatsby build
$ git add .
$ git commit -m "Update build"
$ git push origin main
$ git push cpanel main
```

Sources: 

https://support.cpanel.net/hc/en-us/articles/4404218477207-How-to-Configure-SSH-Key-Authentication-for-Use-with-cPanel-Git-Version-Control

https://dev.to/cheerupemodev/continuous-deployment-of-a-gatsby-site-to-cpanel-with-git-version-control-5ha2
