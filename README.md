This is a personal Kickstart / Ansible automation for fresh re-install of Fedora Silverblue distro.

Feel free to take copy whole thing as you need, or take any inspiration from it... :)

# Steps to prepare the reinstall:
0. Populate all the variables inside `group_vars/` and `host_vars/` folders. You can also check the default values of the `defaults/main.yml` files in the `roles/` subfolder in case something does not suit your needs. Don't forget to commit your changes!
1. Backup your current Fedora Silverblue installation by running the backup playbook and then commiting all the changes:
```
$ ansible-playbook --ask-vault-pass backup-user-config.yml
$ git add .
$ git commit -m "Backup before next Fedora installation"
```
Also, make sure to copy the created `*.tar` archives from `/mnt/btrfs` folder to your external disk.
2. Run the update of the **kickstart file** like this:
```
$ ansible-playbook --ask-vault-pass prepare-kickstart-file.yml -e kickstart_host=<hostname>
```
3. Once the updated **kickstart file** has been updated on Github, you can start Fedora Silverblue installation. During the boot process, select the `Install Fedora` option and press the `e` button to update the boot command. Add `inst.ks=<kickstart_file_URL>` before the `quiet` command and press **CTRL+X** to start the installation.
4. Insert your *LUKS encryption password* when prompted by Anaconda installer. After the installation has finished, boot into the freshly installed Fedora Silverblue, and login via the `Ansible Setup` user.
5. Copy the archives of previously backed personal files back to the `/mnt/btrfs` folder. However, you'll have to mount it first:
```
$ sudo -i
$ mkdir -p /mnt/btrfs
$ chmod 0700 /mnt/btrfs
$ chown root:root /mnt/btrfs
$ mount /dev/mapper/luks-... /mnt/btrfs
$ cd /mnt/btrfs
$ cp -v /run/media/setup/<external-drive>/.../*.tar /mnt/btrfs
$ exit
```
6. Ensure that your Internet router has static DHCP entries for the machine you have just reinstalled. Update the IP addresses in the `hosts` if needed.
7. Run the configuration playbook for the freshly installed Fedora:
```
$ ansible-playbook --ask-vault-pass configure-silverblue.yml -e host=<hostname>
```
During the playbook run you will be asked several times to enter the LUKS encryption password during the necessary system reboots.
8. After all the steps has finished successfully, the `Ansible Setup` user will be automatically deleted, and you should be able to login into your new fresh Fedora Silverblue installation... Enjoy! ;)

# NOTE
You take the full responsibility for using any or all steps / guides provided in this repository, and I am not to be hold liable for any damage to your system or data you might incur to yourself by using these Ansible Playbooks. Always make sure to fully review the tasks in all of these playbooks / roles!
