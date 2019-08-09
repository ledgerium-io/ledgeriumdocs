
# Using Mist Ledgerium wallet

For the command line averse, there is now a GUI-based option for creating accounts: The “official” Mist Ledgerium wallet. The Mist Ledgerium wallet, and its parent Mist project, are being developed under the auspices of the Ledgerium Foundation, hence the “official” status. Versions of the wallet app are available for Linux, Mac OS X, and Windows.

!!! warning
    The Mist wallet is beta software. Please beware and use it at your own risk.

Creating an account using the GUI Mist Ledgerium wallet couldn’t be easier. In fact, your first account is created during the installation
of the app.

1.  [Download the latest version of the wallet app](https://github.com/Ledgerium/mist/releases) for your operating system. Opening the Wallet App will kick off syncing a
    full copy of the Ledgerium blockchain on your computer, since you will in effect be running a full geth node.
2.  Unzip the downloaded folder and run the Ledgerium-Wallet executable file.
3.  Wait for the blockchain to fully sync, then follow the instructions on the screen and your first account will be created.
4.  When you launch the Mist Ledgerium wallet for the first time, you will see the account you created during the installation process. By default it will be named MAIN ACCOUNT (COINASE).
5.  Creating additional accounts is easy; just click on ADD ACCOUNT in the app’s main screen and enter the required password.

 !!! info
    The Mist wallet is still in active development, so details of the steps outlined above may change with upgrades.

## **Creating a Multi-Signature Wallet in Mist**

The Mist Ledgerium wallet has an option to secure your wallet balance with a multisig wallet. The advantage of using a multisig wallet is that it requires authorization from more than one account to withdraw larger amounts from your balance. Before you can create a multisig wallet, you'll need to create more than one account.

It's very easy to create account files in Mist. In the 'Accounts' section click 'Add Account'. Pick a strong yet easy-to-remember password (remember there is no password recovery option), confirm it, and your account is created. Create at least 2 accounts. Secondary accounts can be created on separate computers running Mist if you prefer (and theoretically make your multisig more secure doing it this way). You only need the public keys (your deposit addresses) of your secondary accounts when creating the multisig wallet (copy/paste them, do not ever type them by hand). Your primary account will be needed to create the multisig wallet contract, so it must be on the computer you are creating the multisig wallet on.

Now that you have your accounts setup, be safe and back them up (if your computer crashes, you will lose your balance if you do not have a backup). Click 'Backup' in the top menu. Choose the 'keystore' folder, opposite-click on it / choose 'copy' (do NOT choose 'cut', that would be very bad). Navigate to your desktop, opposite-click in a blank area and choose 'paste'. You may want to rename this new copy of the 'keystore' folder to something like 'Ledgerium-keystore-backup-year-month-day' so you have quick recognition of it later. At this point you can then add the folder contents to a zip / rar file (and even password-protect the archive with another strong yet easy-to-remember password if backing up
online), copy it to a USB Drive, burn it to a CD / DVD, or upload it to online storage (Dropbox / Google Drive / etc).

You now should add approximately no less than 0.02 ETH to your primary account (the account you will initiate creation of a multisig wallet with). This is required for the transaction fee when you create the multisig wallet contract. An additional 1 ETH (or more) is also needed, because Mist currently requires this to assure wallet contract transactions have enough 'gas' to execute properly...so no less than about 1.02 ETH total for starters.

You will be entering the full addresses of all the accounts you are attaching to this multisig wallet, when you create it. I recommend copying / pasting each address into a plain text editor (notepad / kedit / etc), after going to each account's details page in Mist, and choosing 'copy address' from the right-side column of buttons. Never type an address by hand, or you run a very high risk of typos and could lose your balance sending transactions to the wrong address.

We are now ready to create the multisig wallet. Under 'Wallet Contracts', select 'Add Wallet Contract'. Give it a name, select the primary account owner, and choose 'Multisignature Wallet Contract'. You will see something like this appear:

"This is a joint account controlled by X owners. You can send up to X XLG per day. Any transaction over that daily limit requires the confirmation of X owners."

Set whatever amount of owners (accounts) you are attaching to this multisig wallet, whatever you want for a daily withdrawal limit (that only requires one account to withdraw that amount), and how many owners (accounts) are required to approve any withdrawal amount over the daily limit.

Now add the addresses of the accounts that you copied / pasted into your text editor earlier, confirm all your settings are correct, and click 'Create' at the bottom. You will then need to enter your password to send the transaction. In the 'Wallet Contracts' section it should show your new wallet, and say 'creating'.

When wallet creation is complete, you should see your contract address on the screen. Select the entire address, copy / paste it into a new text file in your text editor, and save the text file to your desktop as 'Ledgerium-Wallet-Address.txt', or whatever you want to name it.

Now all you need to do is backup the 'Ledgerium-Wallet-Address.txt' file the same way you backed up your account files, and then you are ready to load your new multisig wallet with ETH using this address.

If you are restoring from backup, simply copy the files inside the 'Ledgerium-keystore-backup' folder over into the 'keystore' folder mentioned in the first section of this walkthrough. FYI, you may need to create the 'keystore' folder if it's a brand new install of Mist on a machine it was never installed on before (the first time you create
an account is when this folder is created). As for restoring a multisig wallet, instead of choosing 'Multisignature Wallet Contract' like we did before when creating it, we merely choose 'Import Wallet' instead.

Troubleshooting:
-   Mist won't sync. One solution that works well is syncing your PC hardware clock with an NTP server so the time is exactly correct...then reboot.
-   Mist starts after syncing, but is a blank white screen. Chances are you are running the "xorg" video drivers on a Linux-based OS (Ubuntu, Linux Mint, etc). Try installing the manufacturer's video driver instead.
-   "Wrong password" notice. This seems to be a false notice on occasion on current Mist versions. Restart Mist and the problem should go away (if you indeed entered the correct password).