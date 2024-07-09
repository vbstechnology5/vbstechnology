# Flashing USB's

**Identify the USB Drive:**

Open a terminal.

Use the following command to list connected storage devices:

* ```bash
  lsblk
  ```

Identify your USB drive, usually named `/dev/sdX` (be careful to select the correct drive).

**Unmount the USB Drive:**

Ensure the USB drive is unmounted (replace `/dev/sdX` with your USB drive):

* ```bash
  sudo umount /dev/sdX
  ```

**Flash Ubuntu Using "dd":**

Use the following command to write the Ubuntu ISO to the USB drive:

* ```bash
  sudo dd bs=4M if=path/to/ubuntu.iso of=/dev/sdX status=progress
  ```

Replace `path/to/ubuntu.iso` with the actual path to your Ubuntu ISO file, and `/dev/sdX` with the correct designation of your USB drive.

**Wait for Completion:**

This process may take some time. The `status=progress` option will show the progress of the operation.

**Sync and Eject the USB Drive:**

After the "dd" command completes, sync the data to the USB drive:

* ```bash
  sync
  ```

Safely eject the USB drive:

* ```bash
  sudo eject /dev/sdX
  ```

Now, your USB drive should be bootable with Ubuntu.
