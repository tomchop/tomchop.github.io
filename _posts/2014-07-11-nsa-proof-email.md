---
layout: post
title: How to NSA-proof your email
excerpt: This is the story about a guy who wants to run away from Google's "privacy invasion" and ends up setting-up his own server, with dramatic consequences.
---

<div style="border: 1px solid #AAA; padding: 10px;">
<strong>TL;DR</strong>: This is the story about a guy who wants to run away from Google's "privacy invasion" and ends up setting-up his own server, with dramatic consequences. Click here to jump to the <a href='#lessons-learned'>lessons learned</a>.
</div>


You're on the bus on your way to work, reading yet another article on a document from a source that somehow **most definitely proves** that we're all under mass state-sponsored surveillance. You say to yourself, while chewing off your last piece of organic breakfast bagel:

> Well, I've been using Gmail for quite a few years, with this NSA thing and all, I should probably quit. $diety knows how much information they must have on me. I don't like random people reading through my emails! Even if they're robots, gah! I'm definitely gonna switch this week-end.

So there you are, full of good-will, wanting to finally leave Gmail and head to a more *private*, *secure* haven. Finally get to have some *privacy*, y'know? Whatever those words mean.

# The move

The week-end has finally arrived. You go back home, brew some coffee, sit at your desk, ready to take your privacy back. You look at your options, ranging from #YOLO to what you think is fully-paranoid:

* Another free email provider (~~Gmail~~, Outlook, etc.)
* "Secure" or "privacy-conscious" email provider (Hushmail, Protonmail, Rise.up, etc.)
* Self-hosted email provider & client (MailPile, Thunderbird, Mail.app, etc.)

> Another free email provider doesn't sound too bad. After all Microsoft isn't running analytics on my emails. Or are they? I'm not even sure. Meh, better ignore anything that's free. "If it's free, then you're the product, right?" Heh.

This reasoning's pretty sound. If you want to reduce chances of your data being used otherwise than you intended (that is: not at all), then it's probably better to use private email providers for that. Keep in mind that paying does not guarantee much.

> Let's have a look at these private email providers. Hushmail sounds cool; it's not that expensive, I had some friends using it a few years ago... oh wait, they told me [Hushmail was giving away information to the government](https://www.schneier.com/blog/archives/2007/11/hushmail_turns.html). A private, encrypted email that gives info out to government? Sheesh. ProtonMail is based in Switzerland... oh, but they don't [handle XSS](http://vimeo.com/99599725) that well [[it's been fixed for a while](https://protonmail.ch/blog/update-reported-xss-issue/)]. they probably won't give my info out to anyone; legally at least. Argh, still in beta.

See, you just went from "staying away from prying eyes" to "evading warranted search and seizure". This ain't gonna be easy.

By the way, private email providers are just as subject to the laws of their countries just than to the mercy of any adversary with greater capacities. More on that later.

> Ok, check this out: [MailPile](https://mailpile.is/). Sounds awesome! Plus, it's free. I've never set-up an email server before, but it shouldn't be that hard. I'll call Dave, we'll have some beers and he'll install everything.

So Dave arrives with an old desktop PC of his. ("Your new server!", he says.) That's how you learn you're gonna have to leave the thing on all night or it'll be harder for you to receive email. Dave's installed a brand new mail server on your computer, along with MailPile. He sets up your phone to use your new email address, synchronizes contacts, and your all set! It's 3 a.m. and there's no beer left in the fridge, so Dave heads home.

Time passes. Every time you use Google's search engine, it reminds you that they can't read into your email any more. You feel how nice it is to evade mass-surveilance, not be just some more bits in this big-data ocean...

# The problem

Then one day on your way home, you check your email on your phone and see a weird error message about the server not responding. "Strange", you say; maybe the power was cut-off in the neighborhood and the computer didn't boot on it's own. Old piece of crap. "I'll sort it out once I get home".

So you get home to your perfectly running server. Sendmail is running, so is Mailpile's web interface, but you haven't received any email since at least this afternoon. Weird. You fire-up your laptop, and set off to search for a solution in the Sea of Information. Only... you can't see the sea. There's not internet access. You call technical support:

– Hi, this is $firstname from $ISP, how may I help you?<br>
– My internet connection is down.<br>
– Sure let me check... yeah, it seems you're sending spam. Sorry, but you'll have to clean your computer before we're can put you back on-line.

You check your email server, knowing it's definitely comming from there... You don't remember dave running these programs. You end up unplugging the computer to get your internet access back, and switch back to your old Gmail account for the time being.

You end-up learning that some people broke into your server because of a recent vulnerability in MailPile ("Well you know, it's a beta...", says Dave), and installed quite the collection of programs - a bitcoin miner, a team-speak server, and the famous spambot which got you blacklisted from your ISP. All emails sent and received during the past 6 moths were send to an email address you've never heard of. You just hope that all the documents you were sending to yourself just to keep them somewhere safe will make no sense to whoever is receiving them on the other side...

<h1 id='lessons-learned'>Lessons learned</h1>

What's the problem here? You didn't know what you wanted protection from. Having a **threat model** (knowing *what* to defend from *whom*) is critical in establishing a defensive strategy.

The questions you should be asking are:

* **Who is your opponent?** An intelligence service? Your local police department? Random people snooping on WiFi at the coffee-shop?
* **What information are you trying to protect?** Is it your identity, or the information you're sending, or the fact that someone is sending something to someone?
* What are the **consequences** of failing to protect this information? Will you go to jail? Be killed? Lose a few dollars?

The next stage is identifying what setps you can take to reduce the risk of critical information being disclosed to the enemy.

If you're bothered by the idea of people running ads on your email content, then you should probably not be on Gmail. Opt for another email service which (alledgedly) does not run any analytics on message contents. Also, remember that if ~90% of the people you email are on Gmail, then Gmail can still read ~90% of your email ;-).

Besides, who do you trust more to keep your data safe on the long run: your new, small, still-a-beta email provider, or a well-established giant such as Gmail? Would you rather trust your friend, who's full-time job is *other* than taking care of your email's safety? Do you have the skills it takes to set a server up by  yourself? What about maintaining it *over time*? Your data might be free from ads, but it will be greatly exposed to other threats that might be more dangerous.

Are you running from the NSA? Do you really think the laws in Switzerland or three guys with physics PhD's will stop them from breaking into those servers? Opportunistic encryption does a fair job against "dragnet-style" surveillance, but it won't last a second against an advanced opponent.

People often fail to address these matters when changing security posture. When Snowden was unable to get Greenwald to use PGP, they switched over to Cryptocat. If they had been under the NSA's radars (maybe they were?), I suppose things might have gone a little more awry. **Having a false sense of security is often worse than having no security and knowing it.** The most important when switching (and I'm not only talking about email providers) is to know how your opponent will reacto to it. Always keep in mind who your opponent is, and try to determine how long your defenses will host against it.
