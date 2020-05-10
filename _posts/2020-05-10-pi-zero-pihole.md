---
title: How to get rid of mobile ads with a raspberry pi zero and pihole
layout: post
---

<!-- You can extract all of the variables in use from a Smarty template using a simple call to `*nix` grep with a trivial regex expression. Here is the command-

{% highlight bash %}
grep -o "\{\$[-_a-zA-Z0-9]*\}" my-template.tpl
{% endhighlight %} -->

## The background story

I hate ads.

I don't think I have ever clicked on an ad on purpose. I have clicked on many ads accidentally, because sometimes it is just hard to navigate your way through all this noise and deceptive content-like advertising. I like to think that each click I make on an add by mistake makes the ad business a little bit more irrelevant. Because yes, [it is irrelevant](https://thecorrespondent.com/100/the-new-dot-com-bubble-is-here-its-called-online-advertising).

I can't help thinking that there must be a better way to run the internet than advertising. In my experience, people will often support the concept of online advertising with considerations like: 

"But how could these small websites survive without advertising ?"

This is just the opposite of thinking out of the box. We live in an ad-fueled ecosystem so of course, it's not easy to think of alternatives.

But in the end, who wants to see ads ? who likes ads ?

Nobody.

Many people, like me, use an ad-blocker when they are browsing the web on their laptop. That does the job pretty efficiently.<br>
However, we are powerless when it comes to blocking ads on our smartphone. 
On iOS, Safari lets you use a [third-party ad-blocking app](https://www.lifewire.com/hate-ads-block-safari-iphone-2000778), called a content blocker. <br>I'm not using Safari, but anyway this will not block in-app ads which are by far the most annoying ones. Think about the flash screen that you see sometimes for several seconds when you launch an app, or this video that will pop up once in a while. Not only is this annoying but it also slows down the apps.

Is there anything you can do about that?

Yes there is, and the solution relies on [Pi-hole](https://pi-hole.net/).

<div class="center">
    <img src="/public/images/pi-hole-logo.svg" style="width: 30%; height: 30%">
</div>

## Pi-hole

When you browse the web or when one of your mobile apps interact with a remote server, your mobile asks a DNS server to resolve domain names (like adrienball.fr) to actual IP addresses, because in the end that's what is required to identify a machine on the internet.<br>
To do that, DNS servers store some sort of big tables which map domain names to IP addresses. If you try to access an unknown website, the DNS server won't find any entry in its tables and will return an NXDomain ("Non-Existent Domain") response or a blank HTML page.

Let's say you run your own DNS server on your local network, and this server keeps a blacklist of domain names that you know are associated with advertising or analytics requests. Now, let's say that this DNS server returns NXDomain responses to all requests corresponding to blacklisted domain names, and just forwards all the other requests to a standard DNS server, like the one you have been using so far without being aware of it.

Now let's say you tell your smartphone, or any device you use at home, to use this DNS server instead of the one it's currently using.

Well, you would kill all the ads, everywhere.

This is precisely what Pi-hole is about. 

Pi-hole is a DNS server which comes with a builtin, and quite exhaustive, blacklist of domain names associated to advertising, tracking and analytics.
The good thing with Pi-hole is that it's pretty lightweight and portable, and it can run on many different clients.

At the time I discovered Pi-hole, I had some DIY kit at home which included a [Raspberry Pi Zero](https://www.raspberrypi.org/products/raspberry-pi-zero/). This device is the smallest of its family, it's super cheap but one can struggle to find an interesting way to use it given its limited power.

Well, running Pi-hole on a Pi Zero proved to be one of the best use cases I could think of.
I was initially concerned about performance but after several weeks relying on it, I can't see any performance decrease.

The initial setup is simple: 
- flash a Raspbian Stretch or Buster image on your Pi Zero
- setup wifi and ssh access as usual
- install Pi-hole with ``curl -sSL https://install.pi-hole.net | bash`` (you may have to insall ``curl`` beforehand)

That's it, the Pi-hole DNS server is running on your Pi Zero. But now, you need to actually use this server instead of the standard DNS server your device is using. At this point you have to retrieve the local IP address of your Pi. Then you have multiple options, I will only mention two of them:

- configure your router so that it uses the Pi Zero as its DNS server
- do not change your router configuration, instead set manually the DNS server on your smartphone

The advantage of the first option is that you will benefit from Pi-hole as soon as you are connected to the network. That may or may not be what you want depending on the situation.
The inconvenient is that, if for some reason you want to deactivate Pi-hole, you will have to change your router configuration.

I went for the second option, and change the DNS used by my iPhone when connected to the WiFi. This way, I can quickly deactivate Pi-Hole just by turning off the WiFi on my phone and switch to Cellular.
I choose this option because I anticipated that some apps or websites may not behave properly if they can't reach their ad / analytics server.<br>
It turns out that I haven't had any such issue so far, but still I like the ability to switch off this DNS server.

An interesting feature of Pi-hole is the dashboard it provides. It shows stats on all the domains being queried on your network. That's a great tool to investigate your apps, and distinguish the bad guys from the good guys.

Happy hacking!