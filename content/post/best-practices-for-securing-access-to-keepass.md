+++
date = "2016-12-19T22:24:32Z"
title = "Best Practices for securing access to KeePass"

+++

First things first....calling this article best practices may be a bit strong.  This is more about the way I like to secure access to KeePass; I'm sure things could be done differently or better, but it works for my organisation.

[KeePass](http://keepass.info/) is a great tool.

For teams, or individuals, needing to store passwords, and other data, in a truly secure way KeePass comes out on top.  Unless unlocked with a master password, or a keyfile, or both, the information contained within the database is kept encrypted (using AES and Twofish) to secure it from prying eyes.  As long as the method of securing the database is strong, meaning a strong master password or secured keyfile, you should be good.

So in a team environment where many people need to access the database, how can one ensure that the details needed to access the database are secure?

You could create a strong master password, distribute that to the people who need to know and call it a day; the problem is that people move jobs or leave organisations at which point the master password should really be changed, but is it?

How about an alternative method which doesn't rely on a master password but instead uses security features built-in to the OS.

Do away with the master password and rely on a keyfile instead.  The keyfile should be stored on a network accessible share which is secured to allow the people who need to access it read only permissions.  Now when opening KeePass it is looking at the secure keyfile which is on the network and, assuming you have configured permissions appropriately, is only accessible to those people who should have access to it.  As people move jobs and leave, remove their permissions and in one move the data is secured.

Just two things to be aware of and take into account here:

1. Someone is going to need write access in order to put the keyfile there in the first place and update it should you need to.  Maybe this is best secured with a Domain Admin account (assuming Active Directory here), which should already be a secure and trusted account.
2. You should take a copy of the keyfile and keep it secure elsewhere to protect against corruption or loss.  Without the keyfile, you're not getting into that database again.
