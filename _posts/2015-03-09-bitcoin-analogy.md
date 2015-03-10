---
layout: post
title: Bitcoin explanation via an email analogy
---

Even though Bitcoins are already old news many still don't quite understand how
they work.

Bitcoins are often referred to as decentralised. In practice it means that
no one can seize, create or destroy them. No one can reverse a transaction
or freeze an account. How is this achieved?

Bitcoins are made of a collection of wallets, also called addresses, and a long
record of all transactions, also called a chain. In order to determine how much
money each wallet has surplus and deficit is traced through the history for
each wallet.

We can even emulate Bitcoins with very little cryptography needed.

Let us create a ledger where the records will be kept. Just a Google document
will do.  Any kind of transaction can be added to the ledger by anyone, even
an invalid one such as transferring a million from an empty wallet or
transferring money from someone else's wallet. The act of adding a record is
called broadcasting. Example of such ledger:

~~~ text
...
25. Transfer from Wallet 4567 to Wallet 8345 10 Bitcoins.
26. Transfer from Wallet 4511 to Wallet 2222 11 Bitcoins.
27. Transfer from Wallet 1234 to Wallet 3333 Graham's number of Bitcoins.

~~~

If everyone was honest and played by the rules our currency would be
complete. The ledger is the chain. But transaction 27 tries to transfer
[Graham number][graham]. I suspect it is a prank. So transaction 27 should be
ignored, it should not be in the chain. Firstly, create a rule that each
transaction should be signed to indicate ownership of a wallet. This is
achieved through public-key cryptography where each wallet has a public/private
key pair.  Public key is the address of the wallet. Anyone in possession of the
private key can generate valid signatures for transactions. Now the ledger
should looks like this:

~~~ text
...
25. Transfer from Wallet 4567 to Wallet 8345 10 Bitcoins. Signature 9901.
26. Transfer from Wallet 4511 to Wallet 2222 11 Bitcoins. Signature 7363.
27. Transfer from Wallet 1234 to Wallet 3333 Graham's number of Bitcoins. Signature: 8122.

~~~

However there is another problem: double-spending or spending non-existent
money. Everyone can now verify that transaction 27 is coming from the
legitimate owner of Wallet 1234 but it is certain that Wallet 1234 didn't
have Graham's number of Bitcoins at the point of the broadcast.

The most interesting part of the Bitcoin architecture is this mechanism which
prevents double spending. Using an email analogy I will try to illustrate this
major concept.

We, designers of our toy Bitcoin system, are going to add a long list of
emails to the ledger before any transaction has taken place. Using some
computer program we will register tons of emails but never make a record of
passwords to any of them. We are also going to need a publicly accessible
email account e.g. chain@gmail.com. Anyone should be granted permission to read
emails sent to chain@gmail.com.

In order to assist us in creating the chain a member of the public cracks a
password to any email from the list. He circulates a carefully constructed
message from the cracked email account in the following format (to
chain@gmail.com):

~~~
I recognise transactions 26, 25 and all transactions referred to in the
previous message sent from email2@gmail.com as valid. By the rules, my wallet XXXX
should now be awarded with 1 bitcoin.

~~~

Such message is called a block and the member of the public is referred to
as a miner. Remember that it is quite difficult to crack email accounts and by
circulating the message the miner demonstrates that he has done it. This is
called a proof of work. Since he is doing public a service of verifying
transactions the rules are that his work should be rewarded by 1 bitcoin.
This 1 bitcoin just springs into existence and this is why this process is
called mining.

By construction each block is linked to a previous block via an email since
emails keep referring to each other. Together the blocks form a block chain of
transactions. If all miners were honest the following chain of messages could
be observed by the subscribers of chain@gmail.com:

~~~ text
(A, email1@gmail.com) <- (B, email2@gmail.com) <- (C, email3@gmail.com)
A, B, C - lists of transactions.

~~~

In the meantime, a crooked miner is working hard to sneak in invalid
transactions (such as 27) to the block chain. Since majority of the
participants are honest his messages are ignored by the rest of the miners.
The following picture occurs:

~~~ text
(A, email1@gmail.com) <- (B, email2@gmail.com) <- (C, email3@gmail.com)
                           \
                            (D, email4@gmail.com) <- (E, email5@gmail.com) <- ...

~~~

The person who cracked email4@gmail.com decided not to refer to email3@gmail.com
in his message since he spotted that some transactions in **C** were invalid.
He is an honest miner and he just wants to earn the fee and he doesn't like
double spending. Similarly the miner afterwards ignored **C** and referred to
(**D**, email4@gmail.com) instead.

We are done: the longest line in the above picture is the valid history, i.e.
the chain. The subscribers to chain@gmail.com can reconstruct a series of
messages sent by the miners and determine balance of each wallet and whether
a person A transferred Bitcoins to a person B.

In order for this to work consistently the major idea of Bitcoins is that
honest participants overpower crooked miners. In my analogy dishonest miners
need to be able to crack the email accounts faster then the rest in order to
construct the longest line.  However, they are outnumbered by honest
participants and their attempts to corrupt the chain are ignored.

[graham]: http://waitbutwhy.com/2014/11/1000000-grahams-number.html
