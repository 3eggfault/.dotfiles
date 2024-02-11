# Dotfiles
These are my personal dotfiles and a list of tools I like to use. My main operating System of choice is [Endeavour OS](https://endeavouros.com/) with the **XFCE** Desktop Environment and the [Chicago 95 Theme](https://github.com/grassmunk/Chicago95).

## Wallpaper
The Wallpaper is an image made by the Artist `Ilya Kuvshinov` in case you are interested.

## Software and Tools 
Here is a list of Tools I like to use
|Tool|Description|
|---|-----|
|[LightDM GTK Greeter Settings](https://github.com/xubuntu/lightdm-gtk-greeter-settings)|Tool for configuring LightDM|
|Zsh|Shell|
|Chromium|Browser|
|Flameshot|Screenshot Tool|
|GitUI|Terminal based UI for Git|
|Feh|Lightweight Tool to view images|
|Plymouth|Allows you to have custom boot screens|
|[Oh-My-Zsh](https://ohmyz.sh/#install)|Framework for managing Zsh configurations|
|[Kitty](https://github.com/kovidgoyal/kitty)|Terminal|

In order to install these packages (assuming you are on Endeavour OS) run the following command:

```bash
sudo pacman -S zsh kitty chromium flameshot gitui plymouth
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

```bash
yay -S picom-ftlabs-git lightdm-gtk-settings
```

## Chicago95
The first thing to do is to clone the Chicago95 Git Repository:

```bash
git clone https://github.com/grassmunk/Chicago95.git 
```
Then to apply the Chicago theme we need to run the file `installer.py` which resides inside the cloned Git Repository:

```bash
python installer.py
```

### Plymouth
After installing plymouth you can simply run the following commands in order to set the `Chicago95 Plymouth Theme`. Please note that the `Chicago95` part of the path is the location of the locally cloned GitHub Repo of Chicago95:
```bash
sudo cp -r Chicago95/Plymouth/Chicago95 /usr/share/plymouth/themes/
sudo plymouth-set-default-theme -R Chicago95
```

### Customization
For customization I followed this guide [here](https://github.com/grassmunk/Chicago95/blob/master/INSTALL.md). I did however make some small changes:
* Icon Size for Panel Icons is set to `24px`.
* Set the Default Font to `Helvetica Regular 10px` in the Appearance Section
* Edited the `.zshrc` file to fix a slight error with a missing space for a commented line.
* Add a custom keyboard shortcut under `Keyboard` with the name `xfce4-popup-whiskermenu` which toggles the Menu upon pressing the Super Key
* In Chromium under `Appearance` choose `Use system title bar and borders` and make sure the Theme is set to `Classic`
* Setting the Background Color of the Login Screen to the iconic Win95 color, e.g `#008080`
* Change the Title font of the `Window Manager` to `Helvetica Bold 9px`
* Set the `greeter-session` in `/etc/lightdm/lightdm.conf` to `lightdm-gtk-greeter`.

## Symlinking dotfiles

> Note that this assumes that it is assumed that the Repo you cloned is in the location `$HOME/GitRepos/dotfiles/` if that is not the case you need to adapt this path.

I suggest symlinking the dotfiles. This has the benefit of having the files can reside inside another directory (for example a GitRepo) but the changes are still being applied. In order to Symlink the files execute the following command:

```bash
cp -rsf $HOME/GitRepos/dotfiles/.config/* $HOME/.config/
```

I also created a simple Script that does excatly that. You can simply make it executable with `chmod +x symlink.sh` and call it with `./symlink.sh`.

## Troubleshooting
Some things have not worked out of the box for me. So in case you might have a similar setup/problems I hope this might be useful to you.

### Function Keys not working on IQUNIX F96 
I have a IQUNIX F96 Keyboard and the Function Keys were not working properly. The reason seems to be that the FN Functionality gets
disabled. In order to enable it again create the file `/etc/modprobe.d/hid_apple.conf` with the following content:

```bash
options hid_apple fnmode=2
```
After that create the file `/etc/dracut.conf.d/hid_apple.conf` with the following content

```bash
install_items+=/etc/modprobe.d/hid_apple.conf
```
Lastly regenerate the configuration for dracut with the `sudo dracut --force` command.
