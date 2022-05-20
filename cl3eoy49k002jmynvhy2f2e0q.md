## A Peer-to-Peer Electronic Cash System

We all heard of Bitcoin and its underlying blockchain technology. In this blog I am trying to summaries  the white paper the paper that first introduced Bitcoin published by www.bitcoin.org. 

A purely peer-to-peer version of electronic cash would allow online payments to be sent directly from one party to another without going through a financial institution. 

An electronic payment system based on cryptographic proof instead of trust, allowing any two willing parties to transact directly with each other without the need for a trusted third party. Transactions that are computationally impractical to reverse would protect sellers from fraud, and routine escrow mechanisms could easily be implemented to protect buyers. The system is secure as long as honest nodes collectively control more CPU power than any cooperating group of attacker nodes.

### Transaction?

We define an electronic coin as a chain of digital signatures. Each owner transfers the coin to the next by digitally signing a hash of the previous transaction and the public key of the next owner and adding these to the end of the coin. A payee can verify the signatures to verify the chain of ownership.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653060746982/H8HZuP8Fu.png align="left")

### Time stamp server

We need a way for the payee to know that the previous owners did not sign any earlier transactions. For our purposes, the earliest transaction is the one that counts, so we don't care about later attempts to double-spend. The only way to confirm the absence of a transaction is to be aware of all transactions. Instead of using a central system that aware of all transactions like central bank we opt for a decentralized system means a network of computers who have the history of all transactions. The payee needs proof that at the time of each transaction, the majority of nodes agreed it was the first received.

A solution could be a Timestamp server. A timestamp server works by taking a hash of a block of items to be timestamped and widely publishing the hash, such as in a newspaper. The timestamp proves that the data must have existed at the time, obviously, in order to get into the hash. Each timestamp includes the previous timestamp in its hash, forming a chain, with each additional timestamp reinforcing the ones before it.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653060826182/Eu_AT9OxY.png align="left")

### Proof of work

The proof-of-work involves scanning for a value that when hashed, such as with SHA-256, the hash begins with a number of zero bits. The average work required is exponential in the number of zero bits required and can be verified by executing a single hash. For our timestamp network, we implement the proof-of-work by incrementing a nonce in the block until a value is found that gives the block's hash the required zero bits. Once the CPU effort has been expended to make it satisfy the proof-of-work, the block cannot be changed without redoing the work. As later blocks are chained after it, the work to change the block would include redoing all the blocks after it.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653060868053/AC4jPrJ5n.png align="left")

The proof-of-work also solves the problem of determining representation in majority decision making. If the majority were based on one-IP-address-one-vote, it could be subverted by anyone able to allocate many IPs. Proof-of-work is essentially one-CPU-one-vote.

### How this Timestamp generator network runs?

The steps to run the network are as follows:

1) New transactions are broadcast to all nodes.
2) Each node collects new transactions into a block.
3) Each node works on finding a difficult proof-of-work for its block.
4) When a node finds a proof-of-work, it broadcasts the block to all nodes.
5) Nodes accept the block only if all transactions in it are valid and not already spent.
6) Nodes express their acceptance of the block by working on creating the next block in the chain, using the hash of the accepted block as the previous hash.

Nodes always consider the longest chain to be the correct one and will keep working on extending it. If two nodes broadcast different versions of the next block simultaneously, some nodes may receive one or the other first. In that case, they work on the first one they received, but save the other branch in case it becomes longer. The tie will be broken when the next proof of- work is found and one branch becomes longer; the nodes that were working on the other branch will then switch to the longer one.

### Incentives

The first transaction in a block is a special transaction that starts a new coin owned by the creator of the block. This adds an incentive for nodes to support the network, and provides a way to initially distribute coins into circulation.
Like goldminers expend their resources to add new golds to circulation here nodes steadily adds new coins to network with the expense of CPU and electricity. These nodes are called digital miners.

The incentive may help encourage nodes to stay honest. If a greedy attacker is able to assemble more CPU power than all the honest nodes, he would have to choose between using it to defraud people by stealing back his payments, or using it to generate new coins. He ought to find it more profitable to play by the rules, such rules that favors him with more new coins than everyone else combined, than to undermine the system and the validity of his own wealth.

### Reclaiming Disk space

Once the latest transaction in a coin is buried under enough blocks, the spent transactions can be discarded to save disk space. We can archive this without breaking the block's hash, transactions are hashed in a Merkle Tree, with only the root included in the block's hash. Old blocks can then be compacted by stubbing off branches of the tree. The interior hashes do not need to be stored.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653060994110/4VUCzEXO7.png align="left")

### Privacy

The traditional financial system maintain privacy by restricting the public access to transactions but here we will do it by making available all transactions to public where the owner’s public keys will remain anonymous. The public can see that someone is sending an amount to someone else, but without information linking the transaction to anyone. This is similar to the level of information released by stock exchanges, where the time and size of individual trades, the "tape", is made public, but without telling who the parties were.

### Probability Calculation for an Attacker takeover the network

We consider the scenario of an attacker trying to generate an alternate chain faster than the honest chain. Even if this is accomplished, it does not throw the system open to random changes, such as creating value out of thin air or taking money that never belonged to the attacker. Nodes are not going to accept an invalid transaction as payment, and honest nodes will never accept a block containing them. An attacker can only try to change one of his own transactions to take back money he recently spent. 

The race between the honest chain and an attacker chain can be characterized as a Binomial Random Walk. The success event is the honest chain being extended by one block, increasing its lead by +1, and the failure event is the attacker's chain being extended by one block, reducing the gap by -1.

The probability of an attacker catching up from a given deficit is analogous to a Gambler's Ruin problem. Suppose a gambler with unlimited credit starts at a deficit and plays potentially an infinite number of trials to try to reach breakeven. We can calculate the probability he ever reaches breakeven, or that an attacker ever catches up with the honest chain, as follows:

p = probability an honest node finds the next block

q = probability the attacker finds the next block

qz = probability the attacker will ever catch up from z blocks behind



![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653061446100/wstPstteU.png align="left")


Given our assumption that p > q, the probability drops exponentially as the number of blocks the attacker has to catch up with increases. With the odds against him, if he doesn't make a lucky lunge forward early on, his chances become vanishingly small as he falls further behind.

### Conclusion

The paper basically proposed a system for electronic transactions without relying on trust and system can be built by creating a simple unstructured network of robust nodes. They do not need to be identified, since messages are not routed to any particular place and only need to be delivered on a best effort basis. Nodes can leave and re-join the network at will, accepting the proof-of-work chain as proof of what happened while they were gone. They vote with their CPU power, expressing their acceptance of valid blocks by working on extending them and rejecting invalid blocks by refusing to work on them.

Hope you liked the post. Happy Learning!






