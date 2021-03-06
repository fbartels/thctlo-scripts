# Kopano
Scripts for Kopano.

`get-kopano-community.sh`: This script pull the community files for your OS and setup a repo so you can use apt-get to install.

Currently tested on Debian 10 but should work for Debian 8-9 and Ubuntu 16/18 .04 (LTS editions) also.<br>
This eliminates the use of `dpkg -i *.deb on kopano-community files.`

It sets up a local file repo, which is easy to adapt for a webserver repo, examples are provided in the files.<br>
It also adds the z-push repo and libreoffice-online repo for you.<br>
I've also added an autobackup function, so you can revert to a previous version if needed.<br>

For the quick and unpatient, keep the defaults and run:<br>
```
wget -O - https://raw.githubusercontent.com/thctlo/Kopano/master/get-kopano-community.sh | bash
apt install kopano-server-packages
```

And too see the new versions, you can use the following command:
```
apt-cache policy kopano-server-packages kopano-webapp z-push-kopano libreoffice-online
```

Note, when you are upgrading and you might see packages are "kept back" and this is why.<br>
<br>
Kopano is fast moving at the moment, sometimes new packages are added or older removed,<br>
when you just run apt update, in these cases you must use `apt dist-upgrade`.<br>
So make sure you always check for "kept back" packages.<br>
<br>
But there are also packages which might not be removed when upgrading and to make this all work,<br>
you might want to at these options --autoremove --purge, so you can run : `apt dist-upgrade --autoremove --purge`<br>
This removed obsolete files and installes the kept back packages in one go.<br>

The script and the default settings in it, will do following for you:<br>
- create a folder `/home/kopano` , you can adjust the path in the script if you like.<br>
  ! Do note, if you change it after you have run it, you need to adjust the /etc/apt/sources.list.d/*.list files also.<br>
- create a subfolder `apt`, this is the folder where the "$ARCH"/*.deb files will be placed.<br>
- create a subfolder `tmp-extract`, this is used to download the tar.gz file and exact it in there.<br>
  This is done due to different depts of subfolders in the tar.gz files, which made extacting and placing bit harder.<br>
  So we dump all in a temp folder and move it when ready to $ARCH.<br>
- create a subfolder `backups`, when you run the script, the folder apt/$ARCH is moved to backups.<br>
  If the kopano packages had a bad release, you can revert back to a previous version.<br>
- pulls the files from the Kopano community site.<br>
- makes a backup of the previous version to `/home/kopano/backups/OS-ARCH-Date`<br>
- cleanup leftovers in `apt` and `tmp-extract`.<br>
- add z-push repo ( `/etc/apt/sources.list.d/kopano-z-push.list` )<br>
- add libreoffice repo  ( `/etc/apt/sources.list.d/kopano-libreoffice-online.list` )<br>
- setup the local-file repo ( `/etc/apt/sources.list.d/kopano-community.list` )<br>
the repo example file:<br>
  - File setup for Kopano Community: `deb [trusted=yes] file:/home/kopano/apt/ amd64/`<br>
  - Webserver setup for Kopano Community: `deb [trusted=yes] http://localhost/apt amd64/`<br>
  To enable the webserver, install a webserver ( apache/nginx )<br>
  Now symlink `/home/kopano/apt/` to `/var/www/html/apt`<br>
  And dont forget to change localhost to you hostname of ip of you server.<br>


## Donations
If you like my work, support me a bit, even with 1 $ you are helping me.<br>
I dont ask for hunderds, a (few) buck(s) is/are a great gift also.<br>
- [Donate via Paypal](https://www.paypal.me/LouisVanBelle) (my paypal email is louis at van-belle .nl)<br>
- Donate via Bitcoin: 3BMEXFUrncjVKByryNU1fcVLBLKE8i9TpX<br>

## Thanks
@Christian Knittl-Frank for fixes so far. (https://github.com/lcnittl)
