# ethereum-snippets
This repo contains Solidity & Web3 code snippets to make the life of Ethereum Devs easier (hopefully)

I've done most of the eth development using [Truffle](https://github.com/ConsenSys/truffle), and so should you!

I've grown extremely frustrated, trying to follow the [Documentations & Tutorials](https://media.readthedocs.org/pdf/solidity/latest/solidity.pdf) of Solidity and web3, provided by the Ethereum Community.

I've only started exploring blockchain and ethereum development, so I'm still a beginner. From a noob's perspective, learning ethereum dev really sucks. This is mainly due to the lack of **good** resources out there. I get it, its still early stage, blockchain as a technology itself is not mainstream yet, but I expect some level of consistency in its docs from a platform trying to disrupt multiple Billion $ industries.

Ethereum's documentation and tutorials are exactly like its database - DISTRIBUTED! There is just too much chaos and things are not organized at 1 place, from where I could possible access all knowledge about ethereum development. To top it all, even the documentation has a lot of **functions that no longer seem to work in the way they are shown to**.

Take page 13 of the solidity & web3 doc (link provided above) for eg-:

```javascript
Coin.Sent().watch({}, '', function(error, result) {
  //definition
}
```

I must've pulled my hair out a gazillion times because I simply couldn't get this exact thing to work. Even the console errors are cryptic :/
After about 3 hours of frustration, I figured what **actually** worked was something like:

```javascript
MyCoinContract.Sent().watch(function(error, result) {
  //definition
}
```
However, as of this writing, you cannot apply watch () on an event using the Contract Name itself. You have to create a contract object and handle the event using that. Furthermore, the 1st argument for watch () must be the handler function.
So, the full snippet looks something like:

```javascript
let accounts = web3.eth.accounts; //list of accounts truffle provides (sample accounts), accounts [0] is by default the master
Coin.new ({from: accounts [0]})   //create a new Coin Contract
  .then ( (contract) => {
    contract.Sent ().watch ( (err, result) => {
      console.log ('the Sent() event was triggered', err, result);
    });
  })
  .catch ( (err) => {
    console.log ('uh we fucked up');
  });
```

[This guy](https://karl.tech/soliditys-biggest-bug-javascript/) pretty much sums up how I feel about Ethereum development right now.

##What this repo contains
Solidity and Web3 Code snippets & Tutorials for Noobs

##Why did I make this Repo
1. To give blockchain noobs a single place from where they can start developing Etherum DApps, without spending weeks researching what tools fit where (and avoid dealing with frustration the way I did).
2. To point out flaws in the code snippets in Solidity & web3 docs (because these docs don't seem to be up-to-date) and provide the actual syntax & mechanics.

Hopefully, this will help someone
ps- contributions, suggestions and swearing are all welcome =)

