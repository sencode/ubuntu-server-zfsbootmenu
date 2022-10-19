# Changes from upstream
Added BIOS boot support for systems without EFI - doesn't work yet, still working on this.
Based on https://github.com/prabirshrestha/simple-ubuntu-installer

# Ubuntu zfsbootmenu install script

This script creates an Ubuntu installation using the ZFS filesystem. The installation has integrated snapshot management using pyznap. Snapshots can be rolled back remotely at boot over ssh using zfsbootmenu. This is useful where there is no physical access to the machine.

Snapshots allow you to rollback your system to a previous state if there is a problem. The system automatically creates snapshots on a timer and also when the system is updated with apt. Snapshots are pruned over time to keep fewer older snapshots.

Supports:
- Ubuntu 22.04.
- Root filesystem on ZFS.
- Choose from: Ubuntu Server, Ubuntu Desktop, Kubuntu, Xubuntu, Budgie, and Ubuntu MATE.
- Single, mirror, raidz1, raidz2, and raidz3 topologies.
- Native ZFS encryption.
- Remote unlocking of encrypted pools at boot over SSH.
- Automated system snapshots taken on a timer and also on system updates. 
- Remote rollback of snapshots at boot for system recovery over SSH.
- Creation of a separate encrypted data pool (single/mirror/raidz).

## Usage
Boot the system with an Ubuntu live desktop iso (ZFS 2.0 support needed for native encryption, so use Ubuntu 21.04 or later). Start the terminal (Ctrl+Alt+T) and enter the following.

	git clone https://github.com/sencode/ubuntu-server-zfsbootmenu.git ~/ubuntu-server-zfsbootmenu
    cd ~/ubuntu-server-zfsbootmenu
    chmod +x ubuntu_server_encrypted_root_zfs.sh
	
Edit the variables in the ubuntu_server_encrypted_root_zfs.sh file to your preferences.

	nano ubuntu_server_encrypted_root_zfs.sh
	
Run the first part of the script.

	./ubuntu_server_encrypted_root_zfs.sh initial
	
Reboot after the initial installation completes and login to the new install. Username is root, password is as set in the script variables. Then run the second part of the script.

	./ubuntu_server_encrypted_root_zfs.sh postreboot

## Optional: Remote access during boot
The script includes an optional feature to provide remote access during boot. Remote access over ssh allows the system state to be rolled back to a previous snapshot without physical access to the system. This is helpful to return a system to a bootable state following a failed upgrade.

Run the following optional part of the script to enable remote access to zfsbootmenu during boot. Guidance on the use of zfsbootmenu can be found at its project website linked in the credits below.

	./ubuntu_server_encrypted_root_zfs.sh remoteaccess

## Optional: Create a zfs data pool
The script includes an optional feature to create an encrypted zfs data pool on a non-root drive. The data pool will be unlocked automatically after the root drive password is entered at boot.

	./ubuntu_server_encrypted_root_zfs.sh datapool

## FAQ
Additional guidance and notes can be found in the script.

## Reddit discussion thread
https://www.reddit.com/r/zfs/comments/mj4nfa/ubuntu_server_2104_native_encrypted_root_on_zfs/

## Credits
rlaager (https://openzfs.github.io/openzfs-docs/Getting%20Started/Ubuntu/Ubuntu%2022.04%20Root%20on%20ZFS.html)

ahesford E39M5S62/zdykstra (https://github.com/zbm-dev/zfsbootmenu)

cythoning (https://github.com/yboetz/pyznap)
