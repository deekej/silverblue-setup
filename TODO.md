# IDEAS for improvements

* Enable FSTRIM on LUKS partition in kickstart file / as the initial tasks in playbooks.
* Run the `fstrim -v --all` command after the FSTRIM has been enabled on LUKS encrypted partition.
* Enable TPM as the initial tasks in playbooks so that LUKS password is not needed to be entered during the boot process...
