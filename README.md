# Glove80 Rev 1 — ZMK config with Realtek RTL8852AU Bluetooth fixes

This repository contains a personal ZMK firmware configuration for the MoErgo Glove80 (Revision 1), built from the official `moergo-sc/glove80-zmk-config` template.

## What issue this fixes

On Linux systems using a **Realtek RTL8852AU** Bluetooth adapter (USB ID `0bda:4852`, driver `btusb`, firmware `rtl8852au_fw.bin`), the Glove80 may exhibit:

- Constant Bluetooth connect/disconnect loops
- Repeated pairing failures
- A connection that appears to “blink” between connected and disconnected states

This behavior is consistent with a Bluetooth compatibility/stability problem between the Glove80’s ZMK BLE stack and the Realtek adapter. It is not necessarily a hardware fault in the keyboard.

Official Glove80 firmware v26.09 did not resolve the issue in this setup.

## What this config changes

The file `config/glove80.conf` adds Bluetooth stability workarounds:

```conf
CONFIG_ZMK_BLE_EXPERIMENTAL_CONN=y
CONFIG_BT_GATT_ENFORCE_SUBSCRIPTION=n
CONFIG_BT_CTLR_TX_PWR_PLUS_8=y
CONFIG_BT_MAX_CONN=3
CONFIG_BT_MAX_PAIRED=3
```
 
## Instructions
1. Log into, or sign up for, your personal GitHub account.
2. Create your own repository using this repository as a template ([instructions](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template)) and check it out on your local computer.
3. Edit the keymap file(s) to suit your needs
4. Commit and push your changes to your personal repo. Upon pushing it, GitHub Actions will start building a new version of your firmware with the updated keymap.

## Firmware Files
To locate your firmware files and reflash your Glove80...
1. log into GitHub and navigate to your personal config repository you just uploaded your keymap changes to.
2. Click "Actions" in the main navigation, and in the left navigation click the "Build" link.
3. Select the desired workflow run in the centre area of the page (based on date and time of the build you wish to use). You can also start a new build from this page by clicking the "Run workflow" button.
4. After clicking the desired workflow run, you should be presented with a section at the bottom of the page called "Artifacts". This section contains the results of your build, in a file called "glove80.uf2"
5. Download the glove80.uf2
6. Flash the firmware to Glove80 according to the user documentation on the official Glove80 Glove80 Support website (linked above)

### ⚙️ Step-by-Step Flashing Instructions

You will flash each half of the keyboard separately.

#### Flashing the Right Half

1. Power off the right half using its power switch.
2. Connect the USB-C cable from your computer to the right half.
3. Hold down the keys I + PgDn (on the default layout) and, while holding them, switch the power back on.
 - A slow pulsing red LED next to the power switch confirms it's in bootloader mode.
4. A USB Mass Storage device named GLV80RHBOOT should appear on your computer.
5. Copy the .uf2 firmware file onto this drive. The drive will automatically disappear once the copy is complete.

#### Flashing the Left Half

1. Disconnect the USB cable from the right half and connect it to the left half.
2. Power off the left half.
3. Hold down the keys Magic + E (on the default layout) and, while holding them, switch the power back on.
 - The same slow pulsing red LED confirms bootloader mode.
4. A USB Mass Storage device named GLV80LHBOOT should appear.
5. Copy the same .uf2 file onto this drive. It will also disappear when done.

### ⚠️ Important: After Flashing

After changing firmware versions, MoErgo generally recommends performing a "Configuration Factory Reset and re-pairing the left and right halves" procedure. This ensures the new firmware takes full effect. You can find instructions for this in the troubleshooting section of the MoErgo documentation.

Your keyboard is now ready to use.
