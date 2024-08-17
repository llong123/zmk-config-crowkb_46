# crowkb ZMK Config

## Instructions

### Brand new to ZMK?

1. You will need to [sign up for a GitHub account](https://github.com/signup), and [fork this repository](https://docs.github.com/en/get-started/quickstart/fork-a-repo#forking-a-repository) so you can edit it.
2. Navigate to the **Actions** tab and click the "I understand my workflows, go ahead and run them" button to enable builds.

   ![Actions tab with "I understand my workflows" button](https://i.imgur.com/B7cTAE6.png)
3. Edit [config/crowkb.conf](config/crowkb.conf) to add/enable/disable features. Edit [config/crowkb.keymap](config/crowkb.keymap) to change the keymap. If you do not have a nice!view, you will need to remove `nice_view_adapter nice_view` from the shield list in `build.yaml` in order to use the OLED.
4. After committing your changes, your firmware will begin compiling. Assuming there are no typos or other problems, it will eventually be [downloadable from the Actions tab](https://zmk.dev/docs/user-setup#installing-the-firmware).
5. [Flash](https://zmk.dev/docs/user-setup#flashing-uf2-files) the non-settings-reset firmware.

### Do you have a `zmk-config` repo already?

You can add this repository as a module by modifying your `config/west.yml` file.

```yaml
manifest:
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    # Add the base GitHub URL as a remote.
    - name: c-botz # You can name this whatever you like; just make sure the "remote" below matches.
      url-base: https://github.com/c-botz
  projects:
    - name: zmk
      remote: zmkfirmware
      revision: main
      import: app/west.yml
    # Add the name of the repository as a project.
    - name: zmk-config-crowkb_46
      remote: c-botz
      revision: master # This is the name of the branch you want to use.
  self:
    path: config
```

After making this change, add a copy of [the configuration file](config/crowkb.conf) and [the keymap](config/crowkb.keymap) to the `config` folder that is already in your repo.

## Common Questions/Problems

### There was an error and the build didn't finish.

#### There is probably an error in your keymap.

Carefully double-check the last changes you made. Each ZMK code is generally preceded by a reference categorizing what that code does. For example: the letter "Z" is categorized as a **k**ey **p**ress. To add the letter "Z" to a keymap, it must be written `&kp Z`. Don't forget to [read the ZMK docs](https://zmk.dev/docs/features/keymaps)!

[Clicking into the details of the action](https://docs.github.com/en/actions/quickstart#viewing-your-workflow-results) will show the error log. While very long, this log will often show the exact line number causing the problem in your keymap so it is worth reading closely.

[nickcoutsos's in-browser keymap editor](https://nickcoutsos.github.io/keymap-editor) aims to provide a GUI for remapping, which may be helpful.

### I edited the keymap/configuration and flashed, but nothing was updated.

#### Make sure you are editing the files in the [config](config) folder.

Changes made in the `boards/shields` folder will be overridden or ignored; your customizations belong in [config](config).

### My display doesn't work.

#### If you have an OLED, remove `nice_view_adapter nice_view` from your [build.yaml](build.yaml).

OLEDs are not the same as nice!views. If you're not sure what hardware you have, nice!views have *five* pins. If your display has four, it is probably an OLED.

#### Make sure external power is turned on.

The `VCC` pin on the display should measure at or above 3.3V. If you don't have a multimeter: assign `&ext_power EP_ON` in your keymap, press that key, and wait at least 60 seconds for the setting to be saved before resetting the keyboard or flashing new firmware.

### After disconnecting the keyboard from Bluetooth, it won't reconnect.

#### Forget the Bluetooth connection on both the keyboard and the host, then try re-pairing.

If you don't remember which profile on the keyboard was used, you may have to clear them all. [See ZMK's documentation for more details on profile management](https://zmk.dev/docs/behaviors/bluetooth#bluetooth-pairing-and-profiles).

As a last resort, try flashing the `settings_reset` firmware; this will force-clear everything.

## Further Troubleshooting Resources

- [ZMK's troubleshooting page](https://zmk.dev/docs/troubleshooting)
- Ask the ZMK experts in [ZMK's Discord](https://zmk.dev/community/discord/invite)
