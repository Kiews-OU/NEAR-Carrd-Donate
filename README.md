Introduction
==========

If you have a project to fundraise, the Near Protocol is a very suitable platform for this task.

Near Protocol's ability to embed custom functions in the blockchain, Near Protocol, can not only easy transfer money between wallets but also, for example, make a rating of the best investors or philanthropists to stimulate competition between them,  display the amount needed to fundraise in real-time, and much more!

Now I will show how easy it is to implement these functions with Near.

I have created a project ["Let's help homeless pets"](https://near-donate.carrd.co/), with the next features:

1. `addDonate`
Make charitable contributions. But instead the base's "send" function of Near Protocol, I created a function - "addDonate", that can add to the transaction name of the philanthropist, date, and message with wishes.

2. `getDonateBalance`
Display the amount of raised funds in real-time.

3. `getNumberPhilanthropists`
Display the number of philanthropists in real time.

4. `getTopPhilanthropists`
The top list of philanthropists, sorted by amount.

How to do this and how it works to see on the [video/en](https://www.youtube.com/watch?v=GHJN7xU5reU),  [video/ua](https://www.youtube.com/watch?v=_uypyNrGXrQ)

And read step by step manual below.

Near Carrd Donate Module
==========

Near Donate Module for Carrd CMS is a charity fundraising project with a rating of philanthropists. It has some features. Login with Near protocol. Opportunity to donate a certain amount Near. Users can also see the latest donation transactions and the top 5 philanthropists.


Prepare Near contract
==========

To prepare Near contract, you shoud clone [repository](https://github.com/andersenbel/near-carrd-donate)

    git clone git@github.com:andersenbel/near-carrd-donate    

And modify settings in  `src/config.js`:

    const CONTRACT_NAME = process.env.CONTRACT_NAME || 'your-contact-name.testnet';

Next, you should to prepare envoriment accoording [Near Quick Start](#near-quick-start) part below, and deploy contract:

    yarn deploy:contract
    near deploy --accountId carrd-donate.testnet --wasmFile ./out/main.wasm

If all right, you can run the command:

    near view your-contract.testnet getTopPhilanthropists

And get it:

    View call: your-contract.testnet.getTopPhilanthropists()
    []

Carrd integration
==========

Near project can be integrated with [Carrd.co CMS](https://carrd.co/ ).
You shoud to add three embeded block: 
1. Near panel and input gorm for authorisied users `card/carrd-donate-panel.html`
2. Output donates lists area `card/carrd-donate-messages.html`
3. Javascript code for start `card/carrd-donate-invoke.html`


Also You shoud to modify carrd-donate-invoke.html for your settings:

    <link media="all" rel="stylesheet"
        href="https://cdn.jsdelivr.net/gh/andersenbel/near-carrd-donate/carrd/carrd-donate-1.css" />
    <script type="application/javascript"
        src="https://cdn.jsdelivr.net/gh/andersenbel/near-carrd-donate/carrd/carrd-donate-1.js"></script>
    <script>
        new add_near({
            networkId: "testnet",
            keyStore: false,
            nodeUrl: "https://rpc.testnet.near.org",
            walletUrl: "https://wallet.testnet.near.org",
            helperUrl: "https://helper.testnet.near.org",
            explorerUrl: "https://explorer.testnet.near.org",
        }, {
            con_name: "your-contract-name.testnet",
            app_name: "Example App",
            success_url: "https://your-site.com.carrd.co/#success",
            failure_url: "https://your-site.com.carrd.co/#failure",
        }, {
            "btn_signin": "near_protocol_signin",
            "btn_signout": "near_protocol_signout",
            "btn_donate": "buttons02",
            "input_amount": "form01-name",
            "input_sender_name": "near_protocol_input_your_name",
            "input_message_text": "near_protocol_input_message_text",
            "text_username": "near_protocol_username",
            "text_number_messages": "text10",
            "text_balance": "text06",
            "html_top_messages": "near_protocol_html_top_messages",
            "html_last_messages": "near_protocol_html_last_messages",
        })
    </script>    


Near Quick Start
===========
Based on near guest-book repsitory

To run this project locally:

1. Prerequisites: Make sure you have Node.js ≥ 12 installed (https://nodejs.org), then use it to install [yarn]: `npm install --global yarn` (or just `npm i -g yarn`)
2. Run the local development server: `yarn && yarn dev` (see `package.json` for a
   full list of `scripts` you can run with `yarn`)

Now you'll have a local development environment backed by the NEAR TestNet! Running `yarn dev` will tell you the URL you can visit in your browser to see the app.


Exploring The Code
==================

1. The backend code lives in the `/assembly` folder. This code gets deployed to
   the NEAR blockchain when you run `yarn deploy:contract`. This sort of
   code-that-runs-on-a-blockchain is called a "smart contract" – [learn more
   about NEAR smart contracts][smart contract docs].
2. The frontend code lives in the `/src` folder.
   [/src/index.html](/src/index.html) is a great place to start exploring. Note
   that it loads in `/src/index.js`, where you can learn how the frontend
   connects to the NEAR blockchain.
3. Tests: there are different kinds of tests for the frontend and backend. The
   backend code gets tested with the [asp] command for running the backend
   AssemblyScript tests, and [jest] for running frontend tests. You can run
   both of these at once with `yarn test`.

Both contract and client-side code will auto-reload as you change source files.


Deploy
======

Every smart contract in NEAR has its [own associated account][NEAR accounts]. When you run `yarn dev`, your smart contracts get deployed to the live NEAR TestNet with a throwaway account. When you're ready to make it permanent, here's how.


Step 0: Install near-cli
--------------------------

You need near-cli installed globally. Here's how:

    npm install --global near-cli

This will give you the `near` [CLI] tool. Ensure that it's installed with:

    near --version


Step 1: Create an account for the contract
------------------------------------------

Visit [NEAR Wallet] and make a new account. You'll be deploying these smart contracts to this new account.

Now authorize NEAR CLI for this new account, and follow the instructions it gives you:

    near login


Step 2: set contract name in code
---------------------------------

Modify the line in `src/config.js` that sets the account name of the contract. Set it to the account id you used above.

    const CONTRACT_NAME = process.env.CONTRACT_NAME || 'your-account-here!'


Step 3: change remote URL if you cloned this repo 
-------------------------

Unless you forked this repository you will need to change the remote URL to a repo that you have commit access to. This will allow auto deployment to GitHub Pages from the command line.

1) go to GitHub and create a new repository for this project
2) open your terminal and in the root of this project enter the following:

    $ `git remote set-url origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git`


Step 4: deploy!
---------------

One command:

    yarn deploy

As you can see in `package.json`, this does two things:

1. builds & deploys smart contracts to NEAR TestNet
2. builds & deploys frontend code to GitHub using [gh-pages]. This will only work if the project already has a repository set up on GitHub. Feel free to modify the `deploy` script in `package.json` to deploy elsewhere.




  [NEAR]: https://near.org/
  [yarn]: https://yarnpkg.com/
  [AssemblyScript]: https://www.assemblyscript.org/introduction.html
  [React]: https://reactjs.org
  [smart contract docs]: https://docs.near.org/docs/develop/contracts/overview
  [asp]: https://www.npmjs.com/package/@as-pect/cli
  [jest]: https://jestjs.io/
  [NEAR accounts]: https://docs.near.org/docs/concepts/account
  [NEAR Wallet]: https://wallet.near.org
  [near-cli]: https://github.com/near/near-cli
  [CLI]: https://www.w3schools.com/whatis/whatis_cli.asp
  [create-near-app]: https://github.com/near/create-near-app
  [carrd.co]: https://carrd.co/
