---
layout: default
title: Frequently Asked Questions
permalink: /doc/faq/
---

Below you will find questions pertaining to this server (and perhaps other sks-keyservers).

**Can you delete my key from the key server?**

* No, we cannot remove your key from the key server. When you submit a key to our key server the key is also forwarded to other key servers around the world, and they in turn forward the key to still other servers. Deleting the key from our server would not cause it to be deleted from any of the other servers in the world and so this is not an effective way to ensure the discontinued use of your key.

**So you can't delete my key, is there anything I can do?**

* If you still have the private key, you can use your PGP software to generate a revocation certificate, and upload that to the keyserver. The exact procedure for generating a revocation certificate varies depending on what PGP software you are using, please consult their documentation for more information. This will not delete your key from the key server, but it will tell people who download it that the key has been revoked, and should not be used in the future.

**But I want to delete my key from the keyserver *because* I lost the private key (so I can't generate a revocation certificate), can you please delete my key for me?**

* No. Due to the keyserver's peering service, any key deleted on one server will be readded by another server almost immediately. So even if I were to delete your key from my database, it would be readded a few minutes later from another keyserver.

* *Sidenote: this situation actually happened to myself, where I have an old key I wasn't able to remove still on the server. If I can't even get rid of mine, I certainly can't get rid of yours :(*

**Can I delete old email addresses / IDs from my key on the keyserver?**

* No, For the same reasons you can't delete a key. However, if you still have your private and public keypair, you can revoke a User ID from within your GPG client, and then reupload your public key to this keyserver. Consult their documentation for more information.

**I think spammers got my email address from the PGP keyserver. What can I do?**

* Yes, there have been reports of spammers harvesting addresses from PGP keyservers. Unfortunately, there is not much that either we nor you can do about this. Our best suggestion is you take advantage of any spam filtering technology offered by your ISP.

**I get a "Malformed Key" error when trying to upload a revocation certificate. Why doesn't this work?**

* Try applying the revocation certificate to your local key-ring and then re-uploading your key to the keyserver.

**I can't look up keys on the keyserver, and may be seeing the error "connection timed out." However, I can read this FAQ.**

* The sks-keyserver may be offline. If this issue persists for more than an hour or two, shoot me an email at the contact details given on the homepage.
