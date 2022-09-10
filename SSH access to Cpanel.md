## Problem :x:

I don't know how to clone my github repository with ssh in cPanel.

## Solution :white_check_mark:
Folow these steps

### 1. Follow sep 2!! https://support.cpanel.net/hc/en-us/articles/4404218477207-How-to-Configure-SSH-Key-Authentication-for-Use-with-cPanel-Git-Version-Control

### 2. Add remote to you local repo

When repository is added, run following command on you local machine: 

```git add remote cpanel ssh://user@example.tld/path/to/repo```

You can find this information in cpanel: Git Version control -> "arrow down" on repository listed -> copy "clone URL".
