# Introduction

This document describes two alternative approaches to how to _begin_ creating an onchain access-control layer, to govern `jitsi-meet` server's video-chatroom access-control logic.

The approaches can be broadly defined as:

- Token Based: using ERC-721 framework (or ERC-20)
- Multisig Based: using Gnosis Safe

A brief comparison of the options is included below, however current preference is to proceed with the Token Based approach.

## Token Based Approach

### Summary

This approach requires a user's wallet contains NFTs which infer priveleges such as being allowed to enter a chat room.

### User Journey 1

- User loads website.com

- App shows "Connect Wallet" view

- User connects wallet

- App shows page with
  - List of rooms a user has access to (which could be zero)
    - (i.e. their NFTs, or perhaps those which are somehow "associated" with Openhouse... whatever that might mean)
  - Create new room (mints a new NFT)

- User selects Create new room

- App displays interface to allow a user to:
  - provide a name (optional)
  - provide an image (optional)
  - enter room (button)
  
- User enters information and clicks "enter room"

- App mints an NFT for that user, and requests to create a room with `0x` address of the NFT contract.

- Platform validates whether the user owns an NFT based on the `0x` token contract
  - If so, grant access to the user
  - If not, deny access to the user

### User Journey 2

- User loads website.com/0xd9546d7b514a33eff8785f97bf0b047326aa7d3d (for example)
  - where 0xd9546d7b514a33eff8785f97bf0b047326aa7d3d is the contract address for an NFT (MoMint v2)

- App shows "Connect Wallet" view

- User connects wallet

- App requests to create and join a room with the `0x` address of the NFT provided in the URL

- Platform validates whether the user owns an NFT based on the `0x` token contract
  - If so, grant access to the user
  - If not, deny access to the user
  
## Multisig Based Approach

### Summary

This approach requires that to gain access to a room, a users must be part of a multi-sig wallet.

It creates a 1:1 mapping between a "room" (where people can talk), and a Gnosis Safe Multisig wallet.

A user can create a Gnosis Safe multi-sig wallet e.g. at https://rinkeby.gnosis-safe.io/

The name of the room is indexed based on the address of the wallet, and all members of the wallet are allowed access to the room.

For the purposes of this proposed initial approach, it is assumed that users are already members of a Gnosis Multisig.

### User Journey 1

- User loads website.com

- App shows "Connect Wallet" view

- User connects wallet

- App shows page with
  - List of the user's rooms (empty at start)
  - Add existing room
  - Create new room (links to dApp to create new Gnosis Safe)

- User selects "Add existing room"

- App displays interface to allow a user to:
  - provide 0x address of the multisig (required)
  - enter room (button)
  - provide name (not required, only stored in browser cache)
  
- User enters information and clicks "enter room"

- Platform validates that the connected wallet is a member of multisig
  - If so, grant access to the user
  - If not, deny access to the user

### User Journey 2

- User loads website.com/0xaf7a13F96EA111C456eC2c885D34A08F93C0B9f5 (for example)
  - Where 0xaf7a13F96EA111C456eC2c885D34A08F93C0B9f5 is the address of an existing multisig

- App shows "Connect Wallet" view

- User connects wallet

- Platform validates that the connected wallet is a member of multisig referenced in the original URL.
  - If so, lets them in
  - If not, tells them they're not allowed in.

# Comparison (very rough notes)

NFT approach appears to stand out as the preferable option.
- Based on an ERC standard (perhaps Gnosis Multisig is also?)
- Open to being governed onchain, e.g. plug in a DAO / Multisig, by giving it mint/burn access to the NFT :)
- Easy to resolve NFTs (=chat rooms) from a user's wallet (resolving multisigs from wallets isn't so easy, bleugh!)
- Nicely brandable, and imagery / naming can be used to brand the chat room.
- In the mode right now
