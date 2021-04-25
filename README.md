# myarch
My journey of installing arch and exploring it. Documenting so that I know what I did and know how to solve the problems I faced.


## Installing Arch
I installed arch by simply following [this](https://www.youtube.com/watch?v=UiYS8xWFXLY&t=6s) video mostly. 
The arch install part worked fine for most part, though I think he missed a `locale.gen` command, due to which I was facing issues with the terminal starting on gnome.

The major learning curve in the main arch (text) install was the partitioning. So while doing that, I ended up learning the following:- 
 - What are the necessary partitions required in a boot on an EFI system, and that EFI system partition is an important partition type which has different boot images from which we can load during boot. (I was also setting up refind during this same time so I think that I learnt a few solid things about booting.
 - GPT Partitioning table
 - How to use fdisk/cfdisk: Make partitioning tables, make partitions, change partition types.
 - Use of `lsblk`, `fdisk -l /dev/sda`
 - What the /dev and /boot folders are for
 - How and what to mount: `mount /dev/nvme0n1p1 /boot/efi`.

So basically, this was a good learning part.

My compilation of steps to install is arch is as follows:-
 - Boot into live Arch using a live usb
 - Create partitions (Minimum two /boot/efi and /, can also make a swap partition)
 - Mount partitions, mount to root partition to /mnt
 - generate fstab file
 - Install arch using pacstrap 
 - arch-chroot into the system
 - set the zone, hwclock, edit hosts file, make a user (add to wheel groupd) and add it, `sudo visudo` and give sudo priveledges to the wheel group (idk why wheel group especially, must be a good idea).
 - install efibootmgr and grub
 - grub install and make the grub config file 
 - reboot

And voila, arch should be installed, login using the user created last time, and that's it.

## Graphical interface 
Now even though arch is installed, well, without a graphical interface, it's not very fun. And even though it's kinda nice to look at the 200-300 MB RAM usage in neofetch, that fun wears off when you actually wanna do something. Hence, I wanted to install a graphical interface. 

Since I already love gnome, I wanted to try something else, also yeah, as people say, gnome is somewhat "bloated", but then again, if you compare it to the shit xfce4 is (I used it for a while and absolutely despised it), I would pick the "bloat" anyways. And I wonder why does gnome have relatively useless packages like Weather and Maps (seriously, am I too young to know who tf would use Maps on a PC lol); but it does not have gnome-tweaks so that we can actually enable dark mode. Ugh.

Anyways, will continue this later.


## Random erros I faced and how I solved them
### 1. GDM not loading
After installing gnome, when I rebooted into the system, gdm would not load, and I would just get a black screen with a blinking cursor. One way to login would be to go to tty2 and login from there using startx or something, however, if I would do a `Ctrl-Alt-F2` and then `Ctrl-Alt-F1` back to tty1, gdm would load, and everything seemed fine (except, everything was *not* fine. Somehow even though the default windowing system for gnome is Wayland, I could see in the About that it was using X11. And then I realized, this was a hardware problem, my amdgpu was not loading for some reason, and hence I had to edit the /etc/mkinitcpio.conf file and add my integrated graphics card (i915) instead of the already present (amdgpu) entry. 
This solution was found on the archwiki [here](https://wiki.archlinux.org/index.php/GDM#GDM_shows_black_screen_with_blinking_white_cursor).
After I did this and did a `sudo mkinitcpio -P`. 

Rebooted, and voila, got my gdm greeting me as it should, on time, as well as GNOME using Wayland by default (which meant that the three finger gestures worked yayy, gnome 40 would be pretty much the same to me without the three finger gestures lol, it's such a make or break feature)
