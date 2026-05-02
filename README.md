> [!WARNING]
> This theme requires **SDDM v0.21.0 or newer**. Make sure your distro provides the correct version before installing.

[`Customization guide`](#Customizing)

# Dependencies

- SDDM ≥ 0.21;
- QT ≥ 6.5;
- qt6-svg;
- qt6-virtualkeyboard
- qt6-multimedia

# Installation
[`Install script`](#Install-script) [`NixOS flake`](#NixOS-flake) [`Manual`](#Manual-Installation)

## Install script
Just clone the repo and run the script:

```bash
git clone -b main --depth=1 https://github.com/kleqing/SilentSDDM && cd SilentSDDM && ./install.sh
```

> [!IMPORTANT]
> Make sure to test the theme before rebooting by running `./test.sh`, otherwise you might end up with a broken login screen.

## NixOS flake
For NixOS with flakes enabled, first include this flake into your flake inputs:
```nix
inputs = {
   silentSDDM = {
      url = "github:uiriansan/SilentSDDM";
      inputs.nixpkgs.follows = "nixpkgs";
   };
};
```

Next, import the default nixosModule and set the enable option
```nix
{
  inputs,
  ...
}: {
    imports = [inputs.silentSDDM.nixosModules.default];
    programs.silentSDDM = {
        enable = true;
        theme = "rei";
        # settings = { ... }; see example in module
    };
}
```

That's it! SilentSDDM should now be installed and configured.
You may now run the `test-sddm-silent` executable for testing.
For further configuration read the [module](./nix/module.nix) option descriptions and examples.

> [!NOTE]
> Since the module adds extra dependencies to SDDM, 
> you may need to restart for the theme to work correctly.

### Local development and testing under nix
First git clone the repository and cd into the resulting directory
```bash
git clone https://github.com/kleqing/SilentSDDM.git
cd SilentSDDM/
```

Now you may make changes to the contents and test them out using the
following

```bash
nix run .#test
```
## Manual Installation

### 1. Install dependencies:

#### Arch Linux

```bash
sudo pacman -S --needed sddm qt6-svg qt6-virtualkeyboard qt6-multimedia-ffmpeg
```

#### Void Linux

```bash
sudo xbps-install sddm qt6-svg qt6-virtualkeyboard qt6-multimedia
```

#### Fedora

```bash
sudo dnf install sddm qt6-qtsvg qt6-qtvirtualkeyboard qt6-qtmultimedia
```

#### OpenSUSE

```bash
sudo zypper install sddm-qt6 libQt6Svg6 qt6-virtualkeyboard qt6-virtualkeyboard-imports qt6-multimedia qt6-multimedia-imports
```

### 2. Clone this repo:
```bash
git clone -b main --depth=1 https://github.com/kleqing/SilentSDDM
cd SilentSDDM/
```

### 3. Test the theme to make sure you have all dependencies:
```bash
./test.sh
```

### 4. Copy the theme to `/usr/share/sddm/themes/`:
```bash
cd SilentSDDM/
sudo mkdir -p /usr/share/sddm/themes/windows
sudo cp -rf . /usr/share/sddm/themes/windows/
```

### 5. Install the fonts:
```bash
sudo cp -r /usr/share/sddm/themes/windows/fonts/* /usr/share/fonts/
```

### 6. Replace the current theme and set the environment variables in `/etc/sddm.conf`:
```bash
sudoedit /etc/sddm.conf

    # Make sure these options are correct:
    [General]
    InputMethod=qtvirtualkeyboard
    GreeterEnvironment=QML2_IMPORT_PATH=/usr/share/sddm/themes/windows/components/,QT_IM_MODULE=qtvirtualkeyboard

    [Theme]
    Current=windows
```

# Customizing

The preset configs are located in `./configs/`. To change the active config, edit `./metadata.desktop` and replace the value of `ConfigFile=`:

```bash
ConfigFile=configs/<your_preferred_config>.conf
```

> [!NOTE]
> Changes to the login screen will only take effect when made in `/usr/share/sddm/themes/windows/`. If you've changed things in the cloned directory, copy them with `sudo cp -rf SilentSDDM/. /usr/share/sddm/themes/windows/`

<br/>

You can also create your own config file. There's a guide with the list of available options (there are more than 200 of them xD) in the [wiki](https://github.com/uiriansan/SilentSDDM/wiki/Customizing).

> [!IMPORTANT]
> Don't forget to test the theme after every change by running `./test.sh`, otherwise you might end up with a broken login screen.

There are some extra tips on how to customize the theme on the [snippets page](https://github.com/uiriansan/SilentSDDM/wiki/Snippets).

# Acknowledgements

- [Keyitdev/sddm-astronaut-theme](https://github.com/Keyitdev/sddm-astronaut-theme): inspiration and code reference;
- [Match-Yang/sddm-deepin](https://github.com/Match-Yang/sddm-deepin): inspiration and code reference;
- [qt/qtvirtualkeyboard](https://github.com/qt/qtvirtualkeyboard): code reference;
- [Joyston Judah](https://www.pexels.com/photo/white-and-black-mountain-wallpaper-933054/): background;
- [DesktopHut](https://www.desktophut.com/blue-light-anime-girl-6794): background;
- [MoeWalls](https://moewalls.com/anime/ken-kaneki-tokyo-ghoul-re-3-live-wallpaper/): background;
- [MoeWalls](https://moewalls.com/anime/anime-girl-nissan-silvia-live-wallpaper/): background;
- [iconify.design](https://iconify.design/): icons
- [SlientSDDM](https://github.com/uiriansan/SilentSDDM): original author;
