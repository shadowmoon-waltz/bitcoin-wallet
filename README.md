# BITCOIN WALLET SW

Fork (with minor to moderate changes) of [project of same name](https://github.com/bitcoin-wallet/bitcoin-wallet).
Changes will be described in this section; other sections are from original readme and may not reflect fork changes.

The license is unchanged (GPLv3 License). Note that I used and added to the repository the single file mjson library,
which is released under the Apache License 2.0.

Note that this fork is mainly for my own use, so support from me may be minimal. This fork is not affiliated with the upstream
project. I may still take issues and pull requests, time permitting. Please confirm issues or pull requests pertain specifically
to the fork and not the upstream project. If it pertains to the fork, report/pull request here. If it relates to upstream, it
should be reproduced/pull requested against the upstream project and reported/pull requested to that project. I added in features
I needed, and I did not do extensive testing. While I think it works and I don't want anyone to lose crypto, per the license,
neither of us are liable for anything that happens as a result of using this software, there is no warranty, and so on.

Main gradle build command: assembleProdRelease

Small changes: build system fixes due to my version of Gradle or some other build environment reason. Not building sample
Android 3rd party app integration example. uses my usual apk signing code. changes app name to avoid confusion with non-fork/upstream
bitcoin wallet. May be a bug or not, but dynamic fee data was being saved to a filename based on the static asset filename,
not the constant value defined for the dynamic fee data filename, and was fixed. ~~Add mempool.space block explorer. (upstream has
added independently)~~

The send coin button at the bottom of the main transaction screen takes up more space, because I use that more, and the scan a code
to send button that was in the center of the bar was removed, because I don't use that much (you can still choose the camera option
on the send coins screen). Shortened request coins to request, since I don't use it that much and it takes up less space.

The original bitcoin wallet's crash detection and suggestion to report, as well as the report an issue button on each transaction,
are removed (in line with the earlier warning on minimal support). Disables update check too.

~~The original reason for the fork was to save money by pulling in segwit changes from the bitcoinj then-post-release branch.
From what I understand, these changes calculate fees more accurately based on the virtual size rather than the actual message size (so
fees will be the same or lower depending if segwit is used). I forked the original repository and cherry picked changes in the master
branch, as those changes weren't in the then-latest release tag, which is what the upstream Bitcoin Wallet was compiled against. (upstream
has now upgraded to a bitcoinj release with these changes)~~

The send coins screen displays the message size in virtual bytes and the satoshis per virtual byte fees after adding in the intended
send amount (changes the dry run logic of the original code slightly). It also adds the fee amount in satoshis and notes the current
fee category to the existing message.

Added support for mempool.space minimum, low, medium, and high fee categories (in app they are labelled as MS Min, MS Low,
MS Medium, and MS High), which correspond to lowest fee to avoid being ignored indefinitely (seems to usually match existing
Economic fee category), confirmed within one hour, confirmed within 30 minutes, and confirmed as soon as possible, respectively.
Overall, these numbers seem to update faster and are often lower or the same (with the exception of MS Min) than the existing
Medium fee category. The original fee categories, Economic, Normal, and Priority, are still in this fork (and are also dynamic, but they
may update less). Note that the mempool.space values fluctuate often and are sometimes too low, so always double-check whether the values seem reasonable.

Added ability to preview the fee amount in the fee category menu options (they load in when opening send coins, so you might have to wait
a few seconds before they'll show up); it includes the satoshis per virtual byte cost and the approximate local currency amount based on
the average virtual byte transaction size if using segwit (140 virtual bytes).

Changed default send coins screen default fee category from normal to ms low, sweep wallet feature fee category from normal to ms low, and
raise fee feature fee category from priority to ms high.

The dynamic fee category used for fee raise has been changed to mempool.space high. Don't check if you have available funds for fee raise against static default tx fee when determining if the transaction fee raise option is possible for transactions in the transaction list. Like upstream, it will still check the fee against the dynamic fees when you actually click the option to try.

tl;dr changes: small build changes, small ui changes, mempool.space fee categories, enhanced fee preview alongside fee categories, and
changes to default fee category (usually reduces fees relative to upstream) ~~formerly reduces fee overpaying with segwit unlike previous upstream project in some common cases~~

Using release signing based on "gradle.properties" in your gradle config directory (which usually defaults to "~/.gradle").
Add the following lines to that file `
keystoreFile=C:\\somewhere\\key.jks
keystorePassword=<keystore password>
keystoreAlias=<key alias>
keystoreAliasPassword=<key password>
`

Since the upstream commits upgrading the target sdk version, I was having some issues sending payments. This may have been an issue with
something in my fork or network, so it may just be me, but worth noting.

---

# BITCOIN WALLET

Welcome to _Bitcoin Wallet_, a standalone Bitcoin payment app for your Android device!

This project contains several sub-projects:

 * __wallet__:
     The Android app itself. This is probably what you're searching for.
 * __market__:
     App description and promo material for the Google Play app store.


### PREREQUISITES FOR BUILDING

You'll need git, a Java 11 SDK and Gradle between 4.4 and 6.9.x for this. We'll assume Ubuntu 22.04 LTS (Jammy Jellyfish)
for the package installs, which comes with OpenJDK 11 and Gradle 4.4.1 out of the box.

    # first time only
    sudo apt install git gradle openjdk-11-jdk

Create a directory for the Android SDK (e.g. `android-sdk`) and point the `ANDROID_HOME` variable to it.

Download the [Android SDK Tools](https://developer.android.com/studio/index.html#command-tools)
and unpack it to `$ANDROID_HOME/`.

Finally, the last preparative step is acquiring the source code. Again in your workspace, use:

    # first time only
    git clone -b master https://github.com/bitcoin-wallet/bitcoin-wallet.git bitcoin-wallet
    cd bitcoin-wallet


### BUILDING

You can build all sub-projects in all flavors at once using Gradle:

    # each time
    gradle clean build

For details about building the wallet see the [specific README](wallet/README.md).
