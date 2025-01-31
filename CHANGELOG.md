Version 1.1.8.2
https://github.com/dianaw353/dotfiles/releases/tag/V1.1.8.2
-----------------------------------------------------------
**Changes**
- AGS: Make favorites configurable.
- Gaming: Add an option to set system-wide default for OpenJDK version.
  - This is very useful as you might have to use Java 21 if you compile Android apps.
- Hyprland: Make keybound apps (Ex. default terminal app) configurable.
- Pre-tasks: Install `adw-gtk-theme` and add a variable to set the default GTK3 theme between GTK3 and GTK4 Adwaita.
- System: Add option to set USE_MPERFORMANCE=true|false in /etc/environment for applicable AUR packages like `linux-nitrous`.
- Vars: Add Eye of GNOME as the image viewer in core packages.
- Vars: Turn `system.ags.version` to a list variable and rename it to `system.ags.versions`.
- Move `gnome-themes-extra` from vars to pre-tasks.
- Move /etc/environment template from AGS to Hyprland.

**Script**
- Fix the in-place upgrade method in rolling/development releases.

**Fixes**
- AGS: Fix the icon substitude for OpenJDK Java 23.
- Hyprland: Fix the `col.active_border` property in window configs.
- Drivers: Install DKMS version of the proprietary NVIDIA driver instead of regular one.
  - To support custom kernels etc.
- VMware: Install kernel headers using AUR helper instead of Pacman itself.
  - Really useful if you have a custom kernel installed from the AUR.

Version 1.1.8.1
https://github.com/dianaw353/dotfiles/releases/tag/V1.1.8.1
-----------------------------------------------------------
**Changes**
- AGS: Install AGSv2 config on both AGS versions.
- AGS: Install `nwg-look` for GTK2 and GTK3 theme configuration.
- AGS: Don't use the template for keybinds widget data for both AGS versions in AGSv2.
- AGS: Remove AGSv2 uninstall step from AGSv1 build task.
- Core apps: Create configuration directory for Vesktop to link `electron-flags.conf` in there properly.
- Hyprland: Put HyprPM stuff in a separate role. This change brings the following changes along:
  - Check if Hyprland is running or not before actually running HyprPM. This should buy me enough time to make an implementation to have it run on autostart instead of completely skipping it.
  - Remove `system.hyprland.plugins.enabled` variable. The rest of the variables inside `system.hyprland.plugins` stay as is.
    - **If your modification used to rely on this variable's presence in one way or another, you should either restore the variable downstream or check if `hyprpm` is in `default_roles` instead.**
- Gaming: Disable IPv6 kernel support if Lutris is going to be installed.
  - This was done to boost launch time for Lutris from more than 1 minute to mere seconds. This is a known issue and this workaround is advised to do so.

**Script**
- Add an option for developer branch.
- Various optimizations and improvements to handle git-based upgrades.
  - Rolling and developer branches will always use Git to upgrade.
  - Stable will try to upgrade with the use of `patch -p1` and the patch hunks provided by the GitHub APIs if eligible.

**Fixes**
- AGS: Check if AGSv1 is already installed before proceeding with a rebuild.
- AGS: Make AGSv1 compilation actually work by passing `--noconfirm`.
- AGS: Migrate to Ansible-native way of handling local packages.
- AGS: Remove the separate AGSv1 build dependencies installation step.
- Vars: Add `gnome-themes-extra` to core packages to install Adwaita theme as known to be the default for GTK.
- Hyprland: Add touchpad toggle keybinding into the keybindings template too.
- Hyprland: Fix the command for reloading AGSv2 directly.
- Hyprland: Fix the request name for recorder in AGSv1.
- Hyprland: Install the main Hyprland package last to avoid conflicts with -git versions of the dependencies.
- Hyprland: Install -git packages first to avoid conflicts with non-git versions of the same packages later on.
- Hyprland: Make touchpad toggle script work again for Synaptics touchpads using Microsoft Precision Touchpad standard on the latest version of Hyprland.
  - If your touchpad only advertises itself as a `2-synaptics-touchpad` and no other device name ending in `-touchpad`, you should open an issue with your laptop's output for `hyprctl devices`.
- Hyprland: Port over the check for AGS version check in switch-ags to all scripts related to AGS.
- Hyprland: Run desired AGS version if reload is triggered but there's no AGS instance running.
  - AGSv1 will run if system.ags.version == v1 or both.
  - AGSv2 will run if system.ags.version == v2.
- HyprPM: Port the changes in package build process from the AGS role to Hyprland rebuild.
- VMware: Fix misleading name for the package installation process. (KVM packages -> VMware Workstation)

Version 1.1.8
https://github.com/dianaw353/dotfiles/releases/tag/V1.1.8
---------------------------------------------------------
**Changes**
- Install `nvidia-prime` to use `prime-run` if proprietary NVIDIA driver is being installed.

**Script**
- Link Electron launch flag configuration to non-default locations too.
  - This configuration was introduced along with i18n support to have Electron run under native Wayland.
  - This is equivalent to running the executable with `--ozone-platform-hint=auto --enable-wayland-ime`.
    - The former enables Electron to use native Wayland if the session is Wayland.
    - The latter enables 3rd party IMEs (I.e. those that aren't native to the shell) to be used.
  - The following apps are explicitly configured so far;
    - Google Chrome and Chromium browsers that read `chrome-flags.conf`.
    - Apps reading `electron12-flags.conf` instead of `electron-flags.conf`.
    - Spotify
    - Vesktop (Desktop Vencord client using newer versions of Electron)
  - Please open an issue if you come across an app that doesn't get possessed with this change (the cursor should be Adwaita and fcitx5 shouldn't work on such an app).
    - Steam Client is **EXCLUDED**! Open an issue about Steam Client **only if you can find a way to seep these flags through yourself!** A PR would be much appreciated if you can do that instead.

**Fixes**
- Add an `im_quirks.sh` `profile.d` script for applications to pick up fcitx5 as best as physically possible.
  - This includes, but isn't limited to,
    - Apps running under XWayland that respect `XMODIFIERS` variable
    - GTK apps
    - Qt apps (Tested and confirmed with WhatSie)
- Add build dependencies for agsv1
- Ags version switcher fix

Version 1.1.7
https://github.com/dianaw353/dotfiles/releases/tag/V1.1.7
---------------------------------------------------------
## This version is the last version to properly support AGSv1! The next versions will only be for maintenance until AGSv2 support is complete and no new features or changes will be made unless absolutely necessary!

**Changes**
- Initial support for AGSv2 (DEVELOPMENT PURPOSES ONLY!!!)
  - Change need_v1 to version and support both versions to coexist.
  - Configure Hyprland keybindings to run respective scripts for AGS invocations.
  - Write scripts to run across both versions of AGS.
  - Add the ability to switch AGS versions using a keyboard shortcut when system.ags.version is set to "both".

**Script**
- Add version check for the main script.
  - Also performs a true upgrade depending on your installation type if you already had dotfiles before!
  - If you're crazy enough, you may even try to swap channels between rolling and stable.
    - **No support will be provided for any attempts about channel swaps!**

**Fixes**
- Add support for touchpad toggle key on laptops using Microsoft Precision Touchpad standard - Credits to the r/hyprland Reddit community. (#49)
<details>
 <summary>Important notes for this change</summary>

- The implementation is pretty much barebones and needs to be improved.
- The support was tested only for one laptop and needs more laptops to be tested this.
  - Any recent laptop with a touchpad that requires no specific drivers on Windows should have support for this standard.
  - This support only aims laptops with touchpad **toggle** key. If your laptop has specific touchpad on and off keys, which is very unlikely, one of them should return the same keycombo.
  - If your laptop has no toggle key but a gesture (E.g. Double tap on top left corner of the touchpad), it should already work OOTB and this change has nothing to do with that.
-  When reporting a related issue, please do the following and send outputs;
  - The output from `sudo showkey`.
    - Run the command, invoke touchpad lock (such as Fn+Esc on Casper Excalibur G770 series laptops) and wait for 10 seconds for the command to exit.
  - The notification posted if any. Possible values are;
    - Touchpad enabled.
    - Touchpad disabled.
    - Touchpad could not be enabled.
    - Touchpad could not be disabled.
  - The output from `hyprctl devices`, more specifically the `mice` section.
  - If `hyprctl devices | grep touchpad` returns a device name, try the following commands and include the results in your report;
    - `hyprctl keyword device\[<the output from the previous command>\]:enabled false` - Should disable the touchpad.
    - `hyprctl keyword device\[<the output from the previous command>\]:enabled false` - Should enable the touchpad.
- The script posts NORMAL notifications and will clutter your notification panel over time. If you can create an OSD widget for this, contributions are welcome.

</details>

Version 1.1.6
https://github.com/dianaw353/dotfiles/releases/tag/V1.1.6
---------------------------------------------------------
**Changes**
- New package: `batsignal`
  - Also preconfigured for a sane configuration on just about any laptop.

**Fixes**
- Fix how local rebuilds independent from AUR are handled.
- Fix the way AGS versions are treated during version check.
- Stick to AGSv1.
  - We have to do this as an interim until we port the current AGS config to v2.
- Various improvements for PiP windows
  - Improve the way PiP windows are detected
    - This one aims to make it work on Google Chrome and Chromium forks
  - Make PiP windows opaque
  - Remove WM borders from PiP windows
- Fix some keybind symbols in the keybind guide.
- Fix the fullscreen keybind to involve Shift

Version 1.1.5
https://github.com/dianaw353/dotfiles/releases/tag/V1.1.5
---------------------------------------------------------
**Changes**
- Hyprland: Involve SUPER key into the shortcut for screen recording
- Hyprland: Rebind "Reload AGS" to `SUPER+Shift+R`
- Enable support for XDG autostart entries using `dex`
  - This replaces explicitly launching fcitx5 as it already provides an XDG autostart entry.
  - See #52 for a deeper lore into this.

**Script**
- Make `hyprland` role work standalone too.
- Install Steam Native Runtime for compatibility.
- Optionally rebuild Hyprland if;
  - Hyprpm fails,
  - The relevant toggle is enabled, and
  - A URL to a PKGBUILD is defined.
    - See #53 as to why this is a thing.
- Allow installing other plugins using Git repo URLs.

**Fixes**
- Configure Electron apps for proper Wayland support
- AGS: Fix the "Settings" button under WiFi menu in the QS
- ZSH: Fix return code "1" upon launch
- Add missing keycombo guide for logging out (exiting Hyprland)

Version 1.1.4
https://github.com/dianaw353/dotfiles/releases/tag/V1.1.4
---------------------------------------------------------
**Script**
- Add new AUR package: `hyprsunset-git`
- Add keybind to toggle Hyprsunset's night light: Ctrl+Super+N
- Remove `hyprshade`
- Add opt-in role for VMware Workstation
- Ship and add support for fcitx5
  - Also includes fcitx5 IMEs for the following languages;
    - Chinese
    - Japanese
    - Korean
    - toki pona
- Add supplementary font support for Japanese and toki pona
- Add `lib32-gnutls` as a compatibility layer dependency

**Fixes**
- Fix autocompletion for `ls`
- Add missing keycombo guide for SUPER key passthrough

Version 1.1.3
https://github.com/dianaw353/dotfiles/releases/tag/V1.1.3
---------------------------------------------------------
**Changes**
- Add a ZSH alias to remove `pacman`'s `db.lck`
- Enable experimental D-Bus interfaces for Bluetooth

**Script**
- Add new AUR package: `bluetooth-autoconnect`
- Add Noto fonts
- Relocate where NetworkManager is enabled and started
- Restart NetworkManager and Bluetooth services

**Fixes**
- Update so it works with hyprland V0.45

Version 1.1.2
https://github.com/dianaw353/dotfiles/releases/tag/V1.1.2
--------------------------------------------------------
**Changes**
- Switch to wezterm terminal instead of kitty

Version 1.1.1
https://github.com/dianaw353/dotfiles/releases/tag/V1.1.1
--------------------------------------------------------
**Script**
- Simplify dependencies installation on ags/hyprland

**Fixes**
- Autostart hyprland plugins and polkit 
- Switch hibronate to suspend for better system compatibility
- Keybind fixes
- Settings page header fix

Version 1.1
https://github.com/dianaw353/dotfiles/releases/tag/V1.1
--------------------------------------------------------
**Script**
- Hyprland & ags refactoring
- Speed up moveing files to correct location
- Change laptop workarounds
- Rename gpu_drivers to drivers
- Add installation for cpu ucode
- Make sure NetworkManager is enabled
- Gaming role now has compatibillity layer dependencies, utilities, and launchers
- Add android-tools to user_packages
- Update Prism Launcher dependency
- Replace Firefox with Zed Browser
- Replace Alacritty with Kitty
- Replace Blueman with Bluetuith 
- Copy 'usr/share/pipewire' folder to '/etc/pipewire' to fix screenshare
- Pipewire loads by default
- Change core/custom packages so they are not dependent on arch only features

**Ags**
- Added config

**Fastfetch**
- Cleaner look

**Hyprland**
- Updated some keybindings
- Add hypr-dynamic-cursor plugin for shake to find cursor
- Autostart polkit-gnome

**Hypridle**
- Screen no longer locks when in fullscreen
- Pause all players when screen locks

**Hyprlock**
- Get correct pfp and wallpaper

Version 1.0.7
https://github.com/dianaw353/dotfiles/releases/tag/V1.0.7
--------------------------------------------------------
**Script**
- More Refactoring

**Hyprland**
- Add polkit-gnome
- New lock screen keybind

**Hypridle**
- Shorten time to dim, screen/keyboard backlight, lock screen

**Fixes**
- Make sure screen locks when laptop lid is closed

Version 1.0.6
https://github.com/dianaw353/dotfiles/releases/tag/V1.0.6
--------------------------------------------------------
**Script**
- Add gaming module, option to install minecraft
- Change masa drivers to only install for amd/intel
- Add 32bit drivers
- Added AMD close source drivers
- Added blocks to each roles

**Fixes**
- gpu_drivers & package_cleanup runs correctly

Version 1.0.5
https://github.com/dianaw353/dotfiles/releases/tag/V1.0.5
--------------------------------------------------------
**Script**
- Reorganise some flags
- GTK window buttons configuration (no buttons default)
- Command to disable oh-my-posh update warnings
- Add systemd-timesync module
- Move many apps to core_apps
- Add custom_apps
- Update installation method
- Remove sunroof & spotube
- Add custom_apps
- Enable systemd-timesync module
- Simplify group_vars/all.yml
- Move system to pre_tasks

**Fixes**
- Add dejavu-nerd font, github-cli
- Remove sunroof
- Remove is_nvidia flag
- dotfiles script now download dotfiles

Version 1.0.4
https://github.com/dianaw353/dotfiles/releases/tag/V1.0.4
--------------------------------------------------------
**Script Changes**
- WIP/ALPHA: Add variable for driver preference for Nvidia GPUs
- Make terms for package managers more generic
- Merge audio/bluetooth into system role

**Hyprland**
- Add hypridle & hyprlock

**Fixes**
- Install openssh as a preliminary dependency
- Install some missing packages for a barebones Arch install fresh out of the wiki replication
- Separate new Nvidia cards from old, unsupported ones
- Install Nouveau dependencies if opted out of proprietary Nvidia drivers

Version 1.0.3
https://github.com/dianaw353/dotfiles/releases/tag/V1.0.3
--------------------------------------------------------
**Fixes**
- Add playerctl
- Move avatar to pfp and mountain to wallpaper in Pictures
- Premission improvements

Version 1.0.2
https://github.com/dianaw353/dotfiles/releases/tag/V1.0.2
--------------------------------------------------------
**Script Changes**
  - Install KVM
  - Set up KVM w/ 3d acceleration support
  - Detect if user is using VM
  - Add is_vm for additional configuration
    - Change hyprland env's depending if is_vm is set to true
  - Add is_nvidia for additional configuration
    - Change hyprland env's depending if is_nvidia is set to true
  - Greetd gets images from assets

**Hyprland Changes**
- QT_QPA_PLATFORMTHEME,qt5ct added 
- Add gamemode script
- Change missioncenter windowrules to use class instead of title and preferences to be shown in the center of the screen
- Autostart hyprpaper and add hyprpaper config

**Fixes**
- Hyprland scripts are now executable
- Ansible host fixes 
- Rename gpu_drivers.yml to main.yml
- Rebase main.yml
- Add xdg-desktop-portal-gtk to system compatibility packages
- Move Greetd config to correct location 
- Install all ansible requirements
- Fix cursor install to install correct package
- Replace swww with hyprpaper
- Add assets folder
  
Version 1.0.1
https://github.com/dianaw353/dotfiles/releases/tag/V1.0.1
--------------------------------------------------------
- Script: Add sunroof and spotube as new applications
- Script: Add 4 new flags
  - Hyprland: Add autostart template for the following to work
    - Icons: Add new icons section
      - Add flag for cursor package (AUR ONLY ATM) 
      - Add flag for cursor theme
      - Add flag to change cursor size
  - Pacman: Add for chaotic_aur
    - Update pacman conf to also have ILoveCandy and VerbosePkgLists
- Script: rate-mirrors added
  - Add rate-mirrors package
  - Update pacman & chaotic aur mirrors list
- Script: Refactor framework workaround to laptop workarounds
- Update pacman conf to also have ILoveCandy and VerbosePkgLists
- Refactor aur_helper
- Add script to backup dotfiles
- Add Hyprshot
- Add keybindings for hyprshot
- Add keybindings to set active windodw to fullscreen
- Add libadwaita as a dependencie

Version 1.0
https://github.com/dianaw353/dotfiles/releases/tag/V1.0
--------------------------------------------------------
- Inital Release
