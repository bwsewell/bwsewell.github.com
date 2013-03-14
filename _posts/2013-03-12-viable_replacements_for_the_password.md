---
author: Brian Sewell
excerpt:
image:
  url:
  title:
  alt:
  attribution:
layout: post
published: true
tags:
-
title: Finding a Viable Replacement for the Password
---

After reading [@mat](http://twitter.com/mat)'s article in [WIRED Magazine](http://www.wired.com/gadgetlab/2012/11/ff-mat-honan-password-hacker/all/) about why the password is no longer a strong enough security blanket for all of our digital lives, I revisited the notion of a viable replacement for the password.  Mat brings up a multitude of valid arguments why the password is old technology that just doesn't seem to die.

We've seemed to advance in every form of security except this.  Homes have gone from having a barking Rottweiler on your front porch to owning advanced systems to protect your home from burglary.  Cars have RFID ignition immobilizers.  Ok, so the whole key and lock thing still has obvious issues.  It boils down to the fact that the system is only as secure as its weakest user.  If you still leave your spare key under your doormat, you're part of the problem.  Robbers know where to look for these things... so do hackers.

## What's Wrong With My Passwords?

The same thing that's wrong with you're hide-a-key rock outside your house.  I can't even count on 2 hands the number of accounts I sign up for every day.  Some new beta service here, another app there.  The common practice nowadays is to have your email address double as your username.  That's a pretty easy thing to come by now.  Anyone you've ever sent an email to already has that piece of information.  So your password?  You probably use the same password for dozens of services.  If a hacker gets his hands on your email address and password, there's good chance they'll be able to access numerous accounts with those credentials.

Creating a different password for every single service isn't feasible for the lay user unless you're using a service like 1Password, but even that service boils down to knowing your master password.

This is a bad analogy, but having the same username and password for multiple services would be like having the key to your home also being the one that opens your car, safe deposit box and parent's house.  If you have all those keys on the same key ring, it's practically the same thing, but you get my drift.

We have much more control over protecting our physical keys as well as what they could possibly go to, but we cannot say the same for our online accounts.

## Developers Need to Take More Responsibility

Universities are starting to require all users to reset their passwords every six to 12 months and usually they disallow them to set it to one of their past five passwords.  When was the last time you received a preemptive password change request from Twitter or Facebook?  Usually it's only after they've been attacked, not before.

I do feel app developers need to start taking greater responsibility in how they handle user authentication.  Here are a few things I feel should be required features as far as user authentication and registration validation goes:

- Require passwords to be longer than 8 characters
- Require passwords to include at least 2 numbers
- Require passwords to include at least 1 non-alphanumeric value
- Don't allow passwords to be the same as the username or email address
- Don't allow passwords to be any of the ones on [this list](http://www.zdnet.com/blog/security/25-most-used-passwords-revealed-is-yours-one-of-them/12427)
- Require users to reset passwords once a year
- Allow users to be notified every time someone logs in to their account (via text, email, etc) and have the ability to disable their account to prevent any fraudulent activity
- NEVER SEND PASSWORD RESET EMAILS WITH THE NEW PASSWORD INSIDE
- Use SSL on all registration/login forms
- Salt and hash passwords

## Two Factor Authentication is a Band-Aid

Facebook, Google and Dropbox have all implemented a form of two-step, which requires a user to enter a temporary code sent to their mobile phone after entering their password.  This added layer of protection definitely helps, but is not a long-term solution.  These text messages could still be intercepted if someone really wanted them badly enough, and then of course... what happens if you lose your phone?

## So Now What?

Biometric authentication may be one of the better solutions, but it doesn't seem feasible just yet.  [Physical keys](http://www.technologyreview.com/view/510106/googles-alternative-to-the-password/)?  [Jewelry](http://www.technologyreview.com/news/512051/google-wants-to-replace-all-your-passwords-with-a-ring/#.UT8OP_Bu8_A.reddit)?  We need to find a viable alternative that can be widely adopted.

This should keep us up at night.