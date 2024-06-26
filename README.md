# Catnet 🐺-😺

Catnet is a [custom Bitcoin signet](https://en.bitcoin.it/wiki/Signet#Custom_Signet) with OP_CAT enabled, used to test implementation of [Bitcoin Circle STARK Verifier](https://github.com/Bitcoin-Wildlife-Sanctuary/bitcoin-circle-stark).

Catnet signet magic is: `af34136e`.

## Resources

- [Catnet block explorer](https://catnet-mempool.btcwild.life/)
- [Catnet faucet](https://catnet-faucet.btcwild.life/)

## Joining the Catnet Custom Signet

Catnet is a custom Signet network that provides an environment for testing Bitcoin applications with the OP_CAT functionality enabled. This guide will walk you through setting up a local node to connect to the Catnet.

Notably, we will use Catnet to test the implementation of a [Bitcoin Circle STARK Verifier](https://github.com/Bitcoin-Wildlife-Sanctuary/bitcoin-circle-stark).

Catnet is built based on [Bitcoin Inquisition fork](https://github.com/bitcoin-inquisition/bitcoin), [v27.0](https://github.com/bitcoin-inquisition/bitcoin/releases/tag/v27.0-inq). Bitcoin Inquisistion includes activation of BIP 118 (ANYPREVOUT), BIP 119 (CHECKTEMPLATEVERIFY), and BIN-24-1 (BIP 347, OP_CAT). It also includes a new 'evalscript' subcommand for bitcoin-util that can be used to test script opcode behaviour.

### Prerequisites

- Basic command-line interface skills
- Administrative permissions on your machine
- An internet connection

### Step 1: Download and Install Bitcoin Core

You have two options to download and install Bitcoin Core:

- **Download the pre-built binaries**: You can download the pre-built binaries from the Bitcoin Inquisition release and use them directly.
- **Build from source**: You can build Bitcoin Inquisition from source code ([this release](https://github.com/bitcoin-inquisition/bitcoin/releases/tag/v27.0-inq)).

First, you need to download the appropriate Bitcoin Core binaries for your system. Below are links for commonly used systems:

- **Linux (aarch64)**: [Download v27.0 for aarch64](https://github.com/bitcoin-inquisition/bitcoin/releases/download/v27.0-inq/bitcoin-27.0-inq-aarch64-linux-gnu.tar.gz)
- **macOS (arm64)**: [Download v27.0 for arm64](https://github.com/bitcoin-inquisition/bitcoin/releases/download/v27.0-inq/bitcoin-27.0-inq-arm64-apple-darwin.tar.gz)

You can find links for other systems [here](https://github.com/bitcoin-inquisition/bitcoin/releases/tag/v27.0-inq).

After downloading the tar.gz file for your system, extract it using the following command:

```bash
tar -xzf bitcoin-27.0-inq-<platform>.tar.gz
```

Navigate to the extracted directory:

```bash
cd bitcoin-27.0-inq/bin
```

### Step 2: Configuration

Before starting your node, you need to create a configuration file to properly join the Catnet Signet.

1. **Create a new directory for your Bitcoin data:**

    ```bash
    mkdir -p ~/.bitcoin/catnet
    ```

2. **Create the `bitcoin.conf` file:**

    ```bash
    nano ~/.bitcoin/catnet/bitcoin.conf
    ```

3. **Add the following configuration to the file:**

    ```ini
    # General settings
    signet=1
    txindex=1
    server=1
    daemon=1
    deprecatedrpc=create_bdb

    # Signet settings
    [signet]
    # Custom signet challenge
    signetchallenge=5121027be9dab7dfc2d1b9aac03f883b9a229fc9c298770dec626b2acbf39e9b6e0e0c51ae
    # Add the seed node
    addnode=catnet.btcwild.life

    # RPC settings
    rpcbind=127.0.0.1
    rpcallowip=127.0.0.0/8
    rpcport=38332
    rpcuser=XXX
    rpcpassword=XXX
    ```

    Save and close the file. Replace `rpcuser` and `rpcpassword` with your desired credentials.

### Step 3: Start Your Node

Using `bitcoind`:
Run the following command in the terminal from the `bin` directory of your Bitcoin Core installation:

```bash
./bitcoind -datadir=~/.bitcoin/catnet
```

This command will start your Bitcoin node and connect it to the Catnet Signet.

OR

Using `bitcoin-qt`: Run the following command in the terminal from the `bin` directory of your Bitcoin Core installation:

```bash
./bitcoin-qt -datadir=~/.bitcoin/catnet
```

Screenshot example:

![bitcoin-qt](./docs/resources/img/bitcoin-qt-example.png)

### Step 4: Verifying the Connection

After your node starts, you can verify it's properly connecting to the network by checking the peer information:

```bash
./bitcoin-cli -rpcport=38332 -rpcuser=XXX -rpcpassword=XXX getpeerinfo
```

You should see the Catnet node `35.192.139.170` listed among the peers.

### Step 5: Create a Wallet

You can create a new wallet to interact with the Catnet Signet using the following command:

```bash
./bitcoin-cli -rpcport=38332 -rpcuser=XXX -rpcpassword=XXX -named createwallet wallet_name="test" descriptors=false
```

### Step 6: Generate a New Address

You can generate a new address to receive funds on the Catnet Signet:

```bash
./bitcoin-cli -rpcport=38332 -rpcuser=XXX -rpcpassword=XXX getnewaddress
```

### Step 7: Claim some coins from the faucet

You can claim some coins from the faucet by visiting the [Catnet faucet](https://catnet-faucet.btcwild.life/) and submitting your address and the captcha.

Screenshot example:

![faucet](./docs/resources/img/catnet-faucet-example.png)

### Conclusion

You are now connected to the Catnet custom Signet! This environment allows you to test applications with Bitcoin’s OP_CAT enabled without risking real assets or impacting the main Bitcoin network.

Please ensure that your firewall settings allow connections on port 38333 to enable peer-to-peer network interactions.
