# How to resync Geth

A common reason for Geth to fail can be an unexpected shutdown of a validator machine. Geth uses RAM for temporary memory and during a graceful shutdown some important information will be written to disk. However, during an unexpected shutdown, there isn't time to write to disk (e.g. due to a sudden loss of power) so important data is lost. This loss of data leads to a corruption of the `chaindata` folder, requiring a resync.

The good news is, the required resync can be made much faster than a full resync simply by keeping the `ancient` folder. The ancient folder contains files that are not corrupted during an unexpected shutdown.

## File Locations

- Standard location of the `chaindata` folder.

  ```bash
  /var/lib/goethereum/geth/chaindata/
  ```

- Standard location of the `ancient` folder.

  ```bash
  /var/lib/goethereum/geth/chaindata/ancient
  ```

## Steps

1. Stop Geth.

1. Move the `ancient` folder.

   ```bash
   sudo mv /var/lib/goethereum/geth/chaindata/ancient /var/lib/goethereum/geth/
   ```

1. Delete everything remaining in the `chaindata` directory.

   ```bash
   sudo rm -rf /var/lib/goethereum/geth/chaindata/*
   ```

1. Move the ancient folder back to the now empty chaindata directory.

   ```bash
   sudo mv /var/lib/goethereum/geth/ancient /var/lib/goethereum/geth/chaindata
   ```

1. Change the ownership of the `chaindata` directory to the Geth user.

   ```bash
   sudo chown -R goeth:goeth /var/lib/goethereum/geth/chaindata
   sudo chmod 755 /var/lib/goethereum/geth/chaindata
   sudo chmod 755 /var/lib/goethereum/geth/chaindata/ancient
   ```

1. Start Geth.

Congratulations! You've successfully started a Geth resync 🥳
