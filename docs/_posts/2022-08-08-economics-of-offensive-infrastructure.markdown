---
layout: post
title:  "Economics of Attacker Infrastructure"
date:   2022-08-08 21:41:16 -0700
categories: offensive
---

It's been a while since I wrote a blog, but something sparked my noodle a few weeks ago and this has been burning a hole in my head since then. Actually, there are a couple of thoughts I'm interested in that are only tangentially related.  But here we go!

## Background (the first one)

The first thought hearkens back to the Lapsus$ hacking group. During their spree of compromising large organizations, there were many scoffs to be had when it came out that some people allegedly involved were a [couple of teenagers](https://www.bbc.com/news/technology-60953527). I witnessed a number of hot takes on the Internet that boiled down to something like, "goodness, with all that money poured into security for billion-dollar businesses, all it takes is some bored teenagers to compromise them?!" While there is an element of truth to that, I feel like it is glossing over some very fundamental nuances. Because I believe that *there is a world of difference in what is required to compromise a billion dollar business compared to what is required to compromise a billion dollar business in a way that will not eventually land you in jail*. 

Is a bored teenager with an Internet connection able to look up how to do some phishing and set up something like [Evilginx2](https://github.com/kgretzky/evilginx2) and get on their way? Yes. But are they familiar with how seasoned grizzled investigators are going to track and find them? Are they aware of how the tools/infrastructure they're using could be used against them? Potentially not as much.

## Background (the Great Seconding)

One of my friends was digging into some malware and was telling me about it. One part of the attack chain was convincing people to download and run a file. This attack in particular happened to use [transfer.sh](transfer.sh) for the file upload/hosting/download functionality. Then, of course, they'd use a url shortener to point to the file itself, and send around that shortened link trying to get people to download/run it. 

I just thought this was one of the most fascinating behaviors and made me wish I made and was running transfer.sh. 

## Disclaimer

For full disclosure, I actually know very little about transfer.sh. I don't know who runs it, what their intentions are, or anything like that. My entire thought process below is purely hypothetical and should not be interpreted in the context of the actual site. 

## Down the Rabbit Hole

The old adage, "if you are not paying for the product, you ARE the product" exists for a reason. The Internet is no exception. Which raises the question, what could that possibly mean in the context of running an arbitrary file upload/download site that attackers are inevitably use for their own operations? What could be done with something like that? Below are a few things that just initially come to mind:

  1. Track who is uploading files based on IP addresses/ASNs.

  2. When someone uploads a binary to your hosting, then you have the binary. You can technically do whatever analysis you want to do on it. Yara scan, extracting build artifacts/metadata, etc. There is a fair amount of information you _could_ extract whenever you wish.  In the above bullet, I already know some of you are priming your fingers to type "BuT uZe ToR" in your keyboards. But that may not be a silver bullet. I'm reminded of [this](https://xkcd.com/1105/) XKCD comic. I don't see it being too far off being able to say, "Ah, it's another binary matching specific Yara signatures coming from another Tor IP"...then it's only one step away from the attacker slipping up and uploading a binary with distinct signatures from a non-Tor IP to potentially blow their identity. Or maybe they upload a binary from a Tor IP, but the payload calls out to a domain registered in their own name. The list of potentials goes on.

  3. Track who is downloading files. I find this one particularly fascinating. If your service is being used by attackers, maybe you make an assumption that they will take steps to protect their own identities, but more than likely unwitting victims are taking no such steps. So you can potentially track what IP addresses are downloading malware and start to be able to build a picture of potentially who is targeting/compromising who.

  4. One other potential is either patching or just replacing the binaries themselves. Like, if you were an advanced cyber organization that set yourself up to be a file hosting services, you could view all of the telemetry above. But remember, you control the files themselves. If you wanted access to <insert network here>, you could just let someone else do the dirty work of phishing into that network, then when you get a file download request from an IP range/ASN you care about, serve up your own binary, now you get the bear the fruit of someone else's labor. Or maybe you just patch binaries so your own functionality gets bundled into the attacker's binary so you can piggyback off of their operations.  

## "The Best Way to Rob a Bank is to Own One"

I really love the phrase "the best way to rob a bank is to own one", because I feel like that applies here. Organizations like threat intel providers, nation states, advanced hacking groups _could_ spend an exorbitant amount of time and tooling tracking other people doing operations on the Internet. However, if you can set yourself up as the purveyor and administrator of "free tools" that people use for their hacking operations on the Internet, the amount of intelligence and operational capabilities that could provide you would be worth its weight in gold. 

There is definitely more that _could_ be said about this, but it was such an interesting use case I had to write something about it touching on the subject.  Many people doing "illegal hacking" on the Internet are not on the cutting edge of Opsec, and don't have the expertise, time, resources, or wherewithal to run their own operations on their own infrastructure in a secure way, which leaves so many opportunities like the above for taking advantage of.

Where ever you are, I hope you are well! 

(as an aside, I'm going to be at DefCon this week, feel free to hit me up on Twitter or [LinkedIn](https://www.linkedin.com/in/kylebelitz/). I'm always happy to say hi and meet new people)
