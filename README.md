# voting-smart-contract
decentralised voting system on ethereum using solidity


Basic Principles of Voting

According to the Equal Justice Foundation, the 6 principles of voting are as follows. 

Secret Ballot: Your vote is secret. Nobody should be able to link your vote back to your race, gender, age and personal profile.
One man, one vote: Every voter votes once and the voting system must be able to reconcile the total number of votes to the total number of voters and those who did not vote. 
Voter eligibility: Only eligible voters are allowed to vote.
Transparency: The vote counting process is fixed, rules are well established, known to voters and withstands public scrutiny.
Votes accurately recorded and counted: Vote counting is consistent. Rules are cast in stone. Vote counts are audit-able.
Reliability: Voting system must be accurate and verifiable. Safeguards are in place to prevent frauds, accidents and security breeches.
Demonstration

Here's how I implement these principles in my ballot smart contract. If you wish to run along with the demonstration, create 4 accounts that represents the following participants in MetaMask:

The Chairman - manages the ballot process
Zoe - voter
Ellie - voter
Peter - voter 
Then copy and paste Ballot.sol to remix.ethereum.org.



In this example, the chairman sets up a proposal which he will like his group of voters to vote, for example "Should we re-elect Jack?".



In Remix, switch to the chairman's MetaMask wallet account to deploy the Ballot contract.



The chairman compiles a list of wallet addresses of eligible votes. The decision about who is eligible to vote is an offline process beyond the scope of Blockchain.



In Remix, switch to the Chairman's wallet address. Then add the wallet addresses and names of Zoe, Ellie and Peter using the addVoter function. 



At any time you can click on the respective buttons below to read the ballot contract's public variables which includes information about who the Chairman is, what the proposal is, the current state of the contract and the names of people in the voters register and whether or not they have voted. This ensures transparency. 



The chairman triggers voting to start. At this point, no new voters can be added and the proposal is cast in stone. The chairman sends the ballot contract address to eligible votes.

In Remix, make sure that you are still using the Chairman's wallet address. Press [startVote] to let voting begin (the contract will not allow anyone else other than the person who has created it to startVote).



Zoe attempts to vote. The contract first compares Zoe's MetaMask wallet address to her record in the voters' array. If Zoe's contract address is not found in the array, she's deemed an ineligible voter and her vote will not be accepted. If Zoe's wallet address is found in the voters' array, the contract checks the vote array to confirm if she has voted previously. If she has, the ballot contract refuses to accept her vote. If she hasn't, her vote is accepted, and her status is updated to say that she has voted. Once she has voted, the contract will no longer allow Zoe to vote again.



To vote yes, type "true" at "doVote" and click [transact]. 

Note that the state of the contract now says "1", which means that voting is now ongoing.



Now Ellie and Peter vote.



Change the wallet address to Ellie's and Peter's for them to cast their votes.





At any point, anyone can check voteRegister to see if there's someone in the register who hasn't voted.



Time's up. The chairman closes the ballot. Closing the ballot is a significant step because it stops allowing anyone who hasn't voted to vote. Counting of votes is not possible unless the ballot closes. The Ballot contract now counts all the votes it has received and records the total number of "yes" votes. Once this is done, everyone can view the result. The balloting process has ended and no one, not even the chairman is allowed to changes any properties of the ballot contract. 



To do so in Remix, switch back to the Chairman's wallet account and press [countVote]. The contract will not allow anyone other than the contract creator (which is the chairman) to execute this step.

Once counting is done, results are released and the contract's state becomes 2 which means that voting has ended.



How does the Ballot contract implement the principles of voting?

Secret Ballot

In the ballot contract, only wallet addresses and names are stored. Every voter is identified by their MetaMask wallet address. Other than the chairman who was the person who created the new instance of the ballot contract, no one else can tell who voted yes or no. For the technical folks among us, I meant to say that the votes array is declared private so no one can read the contents of the votes array.

OK, the truth is a little more complicated than this. On a Blockchain, even private variables are readable if you try hard enough, as explained here. For this to truly be private, users of the contract will need to encrypt the values before writing them to the smart contract. 

One man one vote

The voters array stores a list of voters who have voted. It ensures that no one can vote the second time. Once a voter votes, his status changes to "voted" and the ballot contract checks to ensure that he does not vote again.

Voter eligibility

Voter eligibility is determined by assembling a voters' array of wallet address before voting begins. You need to vote with your MetaMask Wallet whose address matches the one that the chairman registers before voting begins.

Wait a moment, doesn't that mean that someone who have stolen my MetaMask wallet will be able to vote on my behalf! That's correct, on a Blockchain, the only thing that separates you from an identity thief is your wallet's private key. 

But how does the chairman ensure that it's really me who's casting the vote? Facial recognition? That's definitely a possibility - build a ballot app to allow the wallet to be unlocked to vote only if you pass a fingerprint or facial recognition test will work.

Transparency

This is one of the things that Blockchain does extremely well. Every action taken and every record etched on the Blockchain is immutable. On a public Blockchain, collusion is close to impossible as described here.

Votes accurately recorded and counted

In the ballot smart contract, the ballot goes through several states, from the point it is created, open for voting to the point where ballot is closed and votes are counted. 

In each of these states, the contract dictates what the chairman and voters is allowed or not allowed to do. For example, the contract does not allow voting to start until the chairman kick-starts the voting process. It does not allow the chairman to stuff the voting box with new voters once voting begins.

Reliability

There is no single point of failure on a Blockchain as every node participating in the Blockchain comes together to keep it running. A smart contract, once deployed on the Blockchain is immutable. The business logic of the smart contract once deployed is cast in stone. There's no way the chairman can change the rules, say, from a one man one vote to one man two vote once the contract is deployed.

In fact, the source code to the smart contract can be published for everyone to inspect. Don't participate if you do not like how it is coded!
