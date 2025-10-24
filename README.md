# Bip157 Playground
A playground for learning about and testing bip157 filters

# What is this?
It's a website where you can learn what bip157 filters are and play with them.

# Why did you make it?
I've been trying to learn this stuff on my own and thought it might help others if I share what I've learned.

# How can I try it?
Just click here: https://supertestnet.github.io/Bip157-Playground

# What are bip157 filters?
To answer that, it will help if I tell you about some of my previous work. Sometimes I make bitcoin wallets, and when I do, I typically make two types: L1 wallets and L2 wallets. L2 wallets use experimental layer two software like the lightning network, and L1 wallets typically "just" use the regular bitcoin blockchain to transact. I've also made some hybrids of the two.

For my L1 wallets, I typically have code in the wallet that generates one or more addresses for the user, and somehow I need the wallet to check each address's balance. Normally, I do this by querying something called an "electrum server." Electrum is a wallet for desktop and android that uses a client/server model, where the client (the wallet itself) asks a server (an electrum server) for data about the blockchain. My wallets typically ask the server for data like, "how many sats are in address A? address B? address C?" etc.

But that is bad for user privacy. Electrum servers can easily log your ip address (which is often associated with your real-world identity) and the bitcoin addresses your wallet asked about, and then sell that info to chain analysis companies, who may resell it to other corporate surveillance firms or even law enforcement.

Bip157 and Bip158 are two specifications that were introduced into bitcoin in 2017 to help fix this. Bip157 offers a privacy-protecting way to run a bitcoin wallet, and bip158 offers a way for bitcoin nodes to help bitcoin wallets do that. A bip157 "wallet" does *not* query a server its addresses. Instead, it uses a two-step process to check the balance of its addresses.

The first step is to ask bitcoin nodes for a bit of data called a Compact Block Filter about each bitcoin block created after their wallet was generated -- the "start height." This Compact Block Filter -- also called a CBF filter or a Bip158 filter or a Bip157 filter -- is not a full block, it's only a list of addresses that sent or received money in a block, and bitcoin nodes pass the list through a compression algorithm before giving it to the user, that way it's relatively small -- much smaller than a full bitcoin block.

Once the user has downloaded CBF filters for each block created after their start height, the second step is, they scan through each filter to see if their address is in the list -- i.e. to see if their address sent or received money in that block. If they detect a match, they request the full block from bitcoin nodes so they can see *how much* money their address sent or received in that block.

This process benefits bip157 wallets in two ways: unlike a full node, they don't have to download blocks that aren't relevant to their own wallet, which saves time and bandwidth; and unlike an electrum wallet, they don't have to share their addresses with a server and ask them how much money they contain. The tradeoff is, downloading and scanning CBF filters takes some time, and the user's wallet has to download and scan at least one filter every time a new bitcoin block gets mined. If the user's wallet is off for a few weeks, it takes about a minute to "catch up," depending on how fast the user's device is and how good their internet connection is.

This project is a bip157 playground. You can toy around with buttons that make bip157 do things -- namely, look up the history of an address in a more privacy-preserving manner than other methods -- and you can take a look at my codebase to see how it works.
