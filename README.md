# 💎 Emerald Academy

## 🚩 Challenge 4: Voting

🎫 Deploy a Voting contract to learn the basics of voting on the Flow blockchain and Cadence. You'll use:
- The local Flow emulator to deploy smart contracts. 
- The local Flow dev wallet to log into test accounts.
- A template Next.js app with sample scripts and transactions to interact with your contract.

🌟 The final deliverable is a DApp that spins up an open DAO that lets community members create proposals and vote within it.

💬 Meet other builders working on this challenge and get help in the [Emerald City Discord](https://discord.gg/emeraldcity)!

---

# 📦 Checkpoint 0: Install

Required: 
* [Git](https://git-scm.com/downloads)
* [Node](https://nodejs.org/dist/latest-v16.x/)  (🧨 Use Node v16 or a previous version as v17 may cause errors 🧨). You know you have installed it if you type `node -v` in your terminal and it prints a version.
* [Flow CLI](https://docs.onflow.org/flow-cli/install/)  (🧨 Make sure to install the correct link for your system 🧨). You know you have installed it if you type `flow version` in your terminal and it prints a version.

```sh
git clone https://github.com/emerald-dao/4-voting.git
```

> in a terminal window, 📱 install the dependencies start your frontend:

```sh
cd 4-voting
npm install
npm run dev
```

> in a second terminal window, start your 👷‍ local emulator:

```bash
cd 4-voting
flow emulator start -v
```

*Note: the `-v` flag means to print transaction and script output to your local emulator*

> in a third terminal window, 💾 deploy your contract and 💸 start your local wallet:

```bash
cd 4-voting
flow project deploy
flow dev-wallet
```

> You can `flow project deploy --update` to deploy a new contract any time.

📱 Open http://localhost:3000 to see the app

---

# 👛 Checkpoint 1: Wallets

We'll be using **the local Flow dev wallet**.

> Click the "Log In" button and notice a window appears with different accounts to select, each with their own Flow Token balance. Select the first account to log in to it.

---

# 📘 Checkpoint 2: Create a Proposal

After logging in, you will be brought to the main dashboard of your DAO. You can see there are no active proposals, so let's make one!

> Press the "Submit Proposal" button on the right side

<img src="https://i.imgur.com/HMfsBPQ.png" alt="filling in proposal fields" width="400" />

To create a proposal, you must fill in:
- The name of the proposal
- The image of the proposal
- A start & end date
- A description of the proposal

> Click "Submit Proposal" to run a transaction that will create the new proposal on-chain

<img src="https://i.imgur.com/Fg8Qmuz.png" alt="run submit proposal tx" width="400" />

A popup will appear to create your new proposal. This is a transaction that will change data on the blockchain. Specifically, it is creating a new `Proposal` resource and storing it in your DAOs collection of proposals. 

> For more on Cadence & Resources, you can look at the contract code in `/flow/cadence/Vote.cdc/` or check out our <a href="https://github.com/emerald-dao/beginner-cadence-course">Beginner Cadence Course</a>

After clicking "Approve", you will be taken back to the main dashboard. You should now see a vote in play:

<img src="https://i.imgur.com/cWsfwBF.png" alt="a vote is now in play" width="400" />

---

# ✏️ Checkpoint 3: Voting

Now that a proposal has been created, lets click on it and see what it looks like:

<img src="" alt="active proposal" width="400" />

You should see:
- Name
- Description
- Image
- Vote counts
- Who voted for what option
- Start & end date
- Who submitted the proposal

You can click to vote a certain way by clicking one of either "For", "Against", or "Abstain".

> Click one of the voting options and see the transaction popup appear:

<img src="" alt="tx to vote" width="400" />

If you click approve, you should see the vote tally change, and your address get added to the list of voters below. Remember, you can't vote again!

---

# ✏️ Checkpoint 4: Vote with a Different Account

Let's try to submit a vote from another account!

> At the top, click "Logout" and log in with a different account than before.

> Click on the same proposal and vote once again.

You should now see there are two votes casted under the proposal. Congradulations, you have a voting system properly working on your local emulator!

# 💾 Checkpoint 5: Deploy it to testnet!

📔 Ready to deploy to a public testnet?!?

> 🔐 Generate a **deployer address** by typing `flow keys generate --network=testnet` into a terminal. Make sure to save your public key and private key somewhere, you will need them soon.

<img src="https://i.imgur.com/jU9sRiL.png" alt="generate key pair" />

> 👛 Create your **deployer account** by going to https://testnet-faucet.onflow.org/, pasting in your public key from above, and clicking `CREATE ACCOUNT`: 

<img src="https://i.imgur.com/OitvEoO.png" alt="configure testnet account on the website" width="400" />

> After it finishes, click `COPY ADDRESS` and make sure to save that address somewhere. You will need it!

> ⛽️ Add your new testnet account to your `flow.json` by modifying the following lines of code. Paste your address you copied above to where it says "YOUR GENERATED ADDRESS", and paste your private key where it says "YOUR PRIVATE KEY".

```json
"accounts": {
  "emulator-account": {
    "address": "f8d6e0586b0a20c7",
    "key": "5112883de06b9576af62b9aafa7ead685fb7fb46c495039b1a83649d61bff97c"
  },
  "testnet-account": {
    "address": "YOUR GENERATED ADDRESS",
    "key": {
      "type": "hex",
      "index": 0,
      "signatureAlgorithm": "ECDSA_P256",
      "hashAlgorithm": "SHA3_256",
      "privateKey": "YOUR PRIVATE KEY"
    }
  }
},
"deployments": {
  "emulator": {
    "emulator-account": [
			"Vote"
		]
  },
  "testnet": {
    "testnet-account": [
      "Vote"
    ]
  }
}
```

> 🚀 Deploy your Vote smart contract:

```sh
flow project deploy --network=testnet
```

<img src="https://i.imgur.com/M7CBp6A.png" alt="deploy contract to testnet" />

> Lastly, configure your .env file to point to Flow TestNet so we can interact with your new contract.

In your .env file, change the following:
1. `NEXT_PUBLIC_CONTRACT_ADDRESS` to your generated testnet address
2. `NEXT_PUBLIC_STANDARD_ADDRESS` to `0x9a0766d93b6608b7`
3. `NEXT_PUBLIC_ACCESS_NODE` to `https://rest-testnet.onflow.org`
4. `NEXT_PUBLIC_WALLET` to `https://fcl-discovery.onflow.org/testnet/authn` 

You can now terminate all your terminals since we no longer need to run our own local blockchain or wallet. Everything lives on testnet!

> Run `npm run dev` to start your application in a terminal.

> On http://localhost:3000/, click "connect" and log in to your Blocto or Lilico wallet, making sure to copy the address you log in with.

> In a terminal, run `npm run mint [THE ADDRESS YOU COPIED ABOVE] [AMOUNT OF TOKENS]`

> In your terminal, you should see a printed "Transaction Id". If you go to [Testnet Flowscan](https://testnet.flowscan.org/) and paste in that Transaction Id, you should see information about that minting transaction.

Click "Get NFTs", and you should see all your NFTs in your account!

---

# 📝 Make Edits!

🔏 You can also check out your smart contract `ExampleToken.cdc` in `flow/cadence/ExampleToken.cdc`.

💼 Take a quick look at how your contract get deployed in `flow.json`.

📝 If you want to make frontend edits, open `index.js` in `pages/index.js`.

# ⚔️ Side Quests

> 🏃 More challenges coming soon...

> 💬 Meet other builders working on this challenge and get help in the [💎 Emerald City Discord](https://discord.gg/emeraldcity)!

> 👉 Problems, questions, comments on the stack? Post them to the [💎 Emerald City Discord](https://discord.gg/emeraldcity).
