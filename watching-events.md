##Watching Events using the web3 API

Events help clients keep track of a particular occurance - the closing of an Auction, for example.
So let's explore how create events, emit them and establish a mechanism that notifies us upon firing of the particular event

We'll start with a very basic ```Auction``` Contract. The end result will load an HTML document which notifies that the call to close the auction will be made after 3 seconds. When the auction is closed, an event is triggered, notifying web3 of the closing, which in turn changes the text to "Auction closed".

Below are the 3 truffle files (the entire truffle project is available under the "project" directory):

**index.html**
```html
<!DOCTYPE html>
<html>
<head>
  <title>MetaCoin - Default Truffle App</title>
  <link href='https://fonts.googleapis.com/css?family=Open+Sans:400,700' rel='stylesheet' type='text/css'>
  <link href="./app.css" rel='stylesheet' type='text/css'>
  <script src="./app.js"></script>
</head>
<body>
  <div id = "status">Closing Auction in 3 seconds...</div>
</body>
</html>
```

**Auction.sol**
```
contract Auction {
  event AuctionClosed (uint highestBid);  //declare eventto be triggered when Auction closes
  address public creator;

  function Auction () { //Auction Constructor to register the creator of the contract
    creator = msg.sender;
  }

  function closeAuction (uint someRandomBid) {
    if (msg.sender == creator) {  //make sure that auction is being ended by the creator themselves
      AuctionClosed (someRandomBid);  //trigger the event to notify the listeneres that the auction has ended
      return;
    }
    throw;
  }
}
```

**app.js**
```javascript
window.onload = function () {
  let accounts = web3.eth.accounts; //create local variable for easy access
  let maxBid = Math.ceil (Math.random () * 1000); //the maximum bid placed by the end of the auction
  let status = document.getElementById ('status');

  Auction.new ({from: accounts [0]}) //create new contract object
    .then ( (contract) => {
      contract.AuctionClosed ().watch ( (err, response) => {  //set up listener for the AuctionClosed Event
        //once the event has been detected, take actions as desired
        status.innerHTML = 'The auction has ended! Highest Bid is ' + response.args.highestBid;
      });

      setTimeout ( () => {  //simulate an auction for 3 seconds, after which the creator closes the auction
        contract.closeAuction (maxBid, {from: accounts [0]});
      }, 3000);
    })
    .catch ( (err) => {
      status.innerHTML = 'Some error occured. I guess shit happens =(';
    });
};
```

The main thing in our Solidity contract is the declaration and triggering of the AuctionClosed event.
The main thing in app.js is the right way to use the web3 API to listen for events and act upon them as and when they occur.

Now, fire up 2 terminals. In the first one, launch the testrpc utility to simulate the ethereum blockchain:
```bash
testrpc
```

In the second terminal, navigate to the root directory of this project, then:
```bash
truffle compile
truffle serve
```

Then open up your browser and launch http://localhost:8080/ (if you haven't messed around with any of the default settings), wait for 3 seconds and BAM! You will see the highest bid (randomly generated, duh).

NOTE: There is no gas involved in this because testrpc is basically a simulation.

That's all folks. Would love to hear your feedback on this =)

If you liked it, share it with your friends so they too can learn something new.
If you hated it, share it with your friends so they too suffer through this piece of crap tut!
