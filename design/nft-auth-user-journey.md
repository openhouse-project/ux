## NFT Auth User Journey

This document describes a minimum viable integration between:

a) `jitsi-meet`'s base functions of "granting a user access to a room" and "booting a user from a room"
b) Openhouse's [`ParticipantAccessToken.sol`](https://github.com/openhouse-project/contracts/blob/main/ParticipantAccessToken.sol) contract based on ERC-721.

This is to enable users to gain access to a `jitsi-meet` chat room by proving they own the appropriate access token for that room.

The feasibility of this approach needs to be validated before finalising a proposed demo flow.

## Assumptions - Moderator Multisig

This approach assumes that there exists a "Moderator Multisig", which is a decentrally controlled entity.

This entity can be assigned a `BURNER_ROLE`, for taking a "decentralised decision" to burn access tokens, and to boot a user from a chat.

This will be known as the "Moderator Multisig", and is representated onchain by an `address`.

### Main Success Scenario 1

1. User `0xDeF...987` loads domain.com/0xAbC...123
- `0xAbC...123` is contract address of an already-deployed [`ParticipantAccessToken.sol`](https://github.com/openhouse-project/contracts/blob/main/ParticipantAccessToken.sol) contract

2. User connects wallet, and signs whatever is required to validate that the user owns `0xdef...987`

3. Auth Server calls the `balanceOf(0xDeF...987)` function on the `0xAbC...123` contract.
- If this returns `0`, tell user they don't have access.
  - Perhaps tells them how to get access token - e.g. link to buy an access token on Opensea.
- If this returns `>=1`, do what is required to allow `jitsi-meet` to let the user in.

4. Auth Server continues to monitor `balanceOf(0xDeF...987)` on `0xAbC...123`.
- If this ever returns `0`, do what is required to boot the user from the chat.

### Main Success Scenario 2

1. User `0xDeF...987` loads domain.com and connects wallet, and signs whatever is required

2. User starts new meeting, provides a `meetingName` or uses an auto-generated name
This triggers the following transactions:
a) deploy new [`ParticipantAccessToken.sol`](https://github.com/openhouse-project/contracts/blob/main/ParticipantAccessToken.sol) containing `meetingName`
  - sets MINTER_ROLE to themself
  - sets BURNER_ROLE to the "Moderator Multisig".
  - returns address of new contract `0xAcE...456`
b) mints one token for themself `0xDeF...987`

3. Auth Server calls the `balanceOf(0xDeF...987)` function on the new `0xAcE...456` contract.
- This ought to return `>=1`, and if so, do what is required to allow `jitsi-meet` to let the user in.

4. Auth Server polls the `balanceOf(0xDeF...987)` function on the `0xAbC...123` contract.
- If this ever returns `0`, do what is necessary to boot the user from the chat.

### Additional Scenario 1

This scenario describes a potential approach for inviting a user.

1. User is alone in the chat.

2. User clicks "Invite more people"

3. App allows user to input address of person to invite `0xAfB...579`

4. App allows user to tick a box to say whether to allow this user to also invite other users.

5. User inputs address, ticks box and clicks "add to chat".
This triggers the following transactions:
a) call `safeMint(0xAfB...579)` to mint access token for invited user
b) call `grantRole(MINTER_ROLE,0xAfB...579)` where `MINTER_ROLE` is retrieved by calling the read `MINTER_ROLE` function which returns `bytes32`

### Additional Scenario 2

At step 2 of Main Success Scenario 2, during the process of starting a new meeting, the app can allow the user to mint multiple NFTs and put them for sale e.g. on Opensea.

It would require a user who starts a meeting to define:

1. how many NFTs they want to mint - `n`
2. what price to charge per NFT - `p`

This option would require a "batch minting" function to be added to the contract. Broadly speaking, this function would mint `n` NFTs.

An additional function would likely need to be called to put `n-1` for sale for price `p`.

If there are tokens available for sale on Opensea, then at step 3 of Main Success Scenario 1, a user with balance of 0 can be given the option of "buying a ticket" to the chat room, and paying the price `p` to make the purchase, and gain access to the room.
