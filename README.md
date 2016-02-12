# JMBinary

Binary distributions of Joinmarket.

## GUI app JoinmarketQt (for sendpayment only at the moment)

The source is from the Joinmarket [`gui branch`](https://github.com/Joinmarket-Org/joinmarket/tree/gui), 
currently based off [release 0.1.2](https://github.com/joinmarket-org/joinmarket/releases).

**LATEST VERSION of JoinMarketQt is version 2**. This includes some minor updates, the most important of which is adding sweep transactions.

Bugs/nits/intended improvements list [here](#todo-list)

Executables can be found under:

* windows/joinmarket-qt.exe
* ubuntu64/joinmarket-qt
* debian32/joinmarket-qt

No other files / setup should be needed. You might need to `chmod 755` on Linux.

For TAILS: use debian32 version, and run the executable with `torify joinmarket-qt`. You can set the irc onion host in the settings tab.

The github commit of these executables has been signed. There are also .asc gpg signatures in each subdirectory.

My key (Adam Gibson, @AdamISZ on github, waxwing on reddit, btctalk, @waxwing__ on twitter): 

    4668 9728 A9F6 4B39 1FA8  71B7 B3AE 09F1 E9A3 197A

The gpg signature files for each binary should verify against the above key.

**You can also use the [python source version](https://github.com/Joinmarket-Org/joinmarket/tree/gui)** ; just run `python joinmarket-qt.py`. However, this of course requires the same installation/dependencies as the console app.

##Walkthrough

Double click the binary to run it (or, if on TAILS, do `torify joinmarket-qt`).
You will be presented with this start screen:

![](/images/jm_start.png)

Note "CURRENT WALLET: NONE" means no joinmarket wallet is loaded yet. Assuming you haven't
yet created one, do Wallet->Generate (otherwise, do Wallet-->Load):

![](/images/jm_seedphrase.png)

This list of 12 words allows you to recreate the wallet in future. **WRITE IT DOWN and do not 
lose it under any circumstances**! (This seedphrase has the same function as for all other
HD wallets, like Electrum).

Next, enter a password for encrypting the wallet:

![](/images/jm_newpassword.png)

Losing this password means you won't be able to access this wallet file and will cause
inconvenience, but you'll still be able to recover the coins with the above seedphrase.

Next, give a name for your wallet file:

![](/images/jm_newwalletfile.png)

The wallet file will be saved with this name under the directory `wallets`, which is 
created in the same location as where you have downloaded the application/binary.

Joinmarket wallet file names are .json by default; this isn't strictly necessary, but better to 
stick to that convention.

The wallet will now automatically load from the blockchain (by default, using blockr's
website API; see below for altering this). It will take a little time (2-10 seconds typically), during which you'll
see "Reading wallet from blockchain...":

![](/images/jm_loadingwallet.png)

Since you just created it, it will have no coins initially:

![](/images/jm_newwalletsynced.png)

Start out by putting some coins into the first address in the "EXTERNAL" section 
of mixdepth 0. Once you have coins in the wallet it will show something like this 
(after a few transactions):

![](/images/jm_walletloadedcoins.png)

This is a good time to explain "EXTERNAL" and "INTERNAL"; external is for paying in. 
Use the 'new' addresses to ensure no address reuse, when you want to pay into the wallet.
When paying out, Joinmarket will take coins from *both* from the internal and external. 
For more info about how this works, read the wiki linked at the top of this page.

Next (and you could have done this at the start), take a look at the "Settings" tab.

![](/images/jm_settings.png)

If you don't have Bitcoin Core, you'll need to use `blockr` as the first option (
blockchain_source); if you do, you can use `bitcoin-rpc`. In that case, you'll need 
to set up Bitcoin Core correctly, see the 
[wiki article](https://github.com/JoinMarket-Org/joinmarket/wiki/Running-JoinMarket-with-Bitcoin-Core-full-node) on that.

The application is compatible with Testnet, if you check that box. Note however that 
there isn't currently an active testnet trading pit, although this changes from time 
to time.

If you're using TAILS, the other thing you can change is the value of `host` in messaging,
to the onion for the IRC server.

Another possible setting that people might want to change is `tx_fees` under `Messaging`. 
Hover over the setting for the tooltip explanation; in brief, leave it as default unless 
it's really important to you to get fast confirmation, in which case change it from 3 to 1.

The tooltips give brief explanations of the other settings.

Next, we'll send a test transaction:

![](/images/jm_sendStart.png)

The top item is unselected by default; it will send change outputs as donations only 
if they are smaller than some small number you set. If you prefer to make a donation 
in the normal way, you can find the address in the "About" window.

Paste the address you want to send to under `Recipient address`

The number of counterparties can be anything from 2 up to 20; realistically, numbers 
greater than about 8 can *sometimes* get a bit problematic, for a number of reasons, e.g. 
the fact that the transaction fee gets large, there might not be enough counterparties 
with sensible fees, messaging delays etc. Having said that, joins with 10-15 counterparties 
*can* be done, and have been. But for ordinary working, I'd recommend a figure between 
3 and 6 as fine.

Mixdepth: this is not completely obvious, and is **important**: this is *where you are 
spending the coins from*. If you have 2 btc in mixdepth 0 and 1 btc in mixdepth 3, and 
you want to send 1.4 coins you must type `0` in this box; Joinmarket only spends coins 
from one mixdepth at one time, to aid privacy (again, see the wiki for details).

Amount in BTC is the amount the recipient address will actually receive; so don't add 
any transaction fees or coinjoin fees here.

Once the settings are done, click Start. **NOTE** you haven't committed to the transaction 
yet at this point. After a little delay (it can be changed using the setting `order_wait_time`) 
while you're connecting to IRC and gathering orders, you'll eventually see a prompt like this:

![](/images/jm_checktransaction.png)

You'll see that 3 (or whatever number you chose) counterparties have been selected. The 
selection is partly random and partly favouring fees as low as possible. The percentage 
fee is shown: in this case, it's 0.546% of 0.01, i.e. 5460 satoshis. Yes, Joinmarket is 
usually very cheap :) For small amounts like this 0.01btc example, the coinjoin fee is 
actually much lower than the bitcoin transaction fee. For larger amounts you'll usually 
find the percentage fee is lower (less than 0.1% is not uncommon).

If you don't like the fee, click No and the transaction is just aborted. If you click Yes, 
you are now committed to the transaction. (Sorry but the "Abort" button is non functional 
right now, this may be fixed later). Assuming all goes well, you will see this:

![](/images/jm_transactionsuccess.png)

The coinjoin transaction has now been broadcast to the blockchain. (This example is real, 
you can look it up if you like: 0b09fdea15059b2cc08cf0db23b9579413d20e43e8385feb8a2415d7b856db6d)

Once the transaction is finished, the Send Payment tab will look something like this:

![](/images/tx_completedsend.png)

You can refer to the previous transactions you've done in the Tx History tab:

![](/images/jm_txhist.png)

Enjoy doing coinjoins :)

##TODO list

1. Add an icon for the GUI
2. Export of private keys (and *possibly* import)
3. (Under discussion) change to a reusable donation address instead of a static one? 







