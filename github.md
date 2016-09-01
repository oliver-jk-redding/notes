#Notes on using Github

##Adding an ssh key to Github:

Copy the contents of the RSA key into clipboard:
```bash
$ pbcopy < ~/.ssh/id_rsa.pub
```
Go to github profile, click on profile pic in top right, click on settings, go to 'ssh keys', click add new key and paste in contents.

###Sources
* https://help.github.com/articles/error-permission-denied-publickey/
* https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/