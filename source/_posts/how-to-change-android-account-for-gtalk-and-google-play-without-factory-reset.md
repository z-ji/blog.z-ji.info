title: How to change android account for gtalk and google play without factory reset
tags:
  - android
id: 43
categories:
  - geek life
date: 2012-04-25 14:45:30
---

Now you can now add multiple accounts to an android phone, and all these accounts can be used in google play(market), gmail sync, and other google services.
However, only the first account you added to the phone can be used in google talk on the phone.
And, you can delete all the accounts except the first account that you used to activate the phone.
When you try to remove that account, the phone will tell you the only way to remove that account is to do a factory reset.
Actually, there is a simply way to do so, as long as your android phone has been rooted.
You can follow the instruction below to delete all the accounts you've added to the phone.

1\. Download Root Explorer from Android Market
2\. Explore to /data/system/ and find this file : accounts.db
3\. Delete that file (accounts.db)
4\. Reboot your Android device.
5\. now you can add the account you want to.

or, you can type the following script in terminal
```
su
cd data/system
rm accounts.db
reboot
```

the above methods have been verified on multiple android 2.x phones.