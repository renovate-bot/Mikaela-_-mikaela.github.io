---
layout: post
title: "Inconsistency and privacy issues with Element, Matrix and Synapse"
category: [english]
tags: [english, matrix, privacy]
redirect_from:
  - /matrix.html
  - /element.html
---

*Having used Matrix since 2016 and hearing about its greatness without any
 issues so much, I wish to correct some misconceptions. I attempt to provide
 citations for everything and not name any other solution. I cannot discuss
 administrating experience due to not having any with Matrix personally.*

# Element, what Element?

Element is the defacto Matrix client. If you wish to get into Matrix, you
will likely hear the advice to install Element or use it on the web.

It comes with two problems:

* you will likely register your account on the `matrix.org` homeserver and
  later hear that you made a mistake in using it as it's overloaded and you
  should instead use some other homeserver which would also be good for
  healthy federation, but the interface doesn't suggest or offer you any
  other servers.
  * maybe in the future [your account will be decentralized and that won't matter](https://github.com/matrix-org/matrix-doc/issues/915)?
* if you happen to be like me and use both Element Web and Element iOS, you
  will notice they are wildly inconsistent. I cannot comment on Element
  Android as my phone (Nokia 1 / TA-1047) is too weak powered for pleasant
  Matrix experience and I don't use it much.

Comparing the later two platforms, I imagine you will hit some of these
problems sooner or later:

* <s>You see a link in the channel. If you were using Element Web or
  possibly even Element Android you would immediately know what it was
  about. However you use [Element iOS that never got URL preview support](https://github.com/vector-im/element-ios/issues/888)!</s>
* You hear of interesting room on another room and you wish to join it. You
  touch the name wishing to get into there? What happens instead? You will get
  an error message [cannot rejoin an empty room](https://github.com/vector-im/element-ios/issues/1066).
  * I hope that doesn't annoy you and you wish to hear the workaround of
    running `/join #room:example.net` by hand instead.
* This may be a bit more rare one, but if you share rooms with bots, you may
  notice that on Element Web they are more gray than people. [Element iOS just never got messages from bots being rendered differently](https://github.com/vector-im/element-ios/issues/882).
* I may again be a bit weird, but I wish to have [timestamps for all messages visible all the time](https://github.com/vector-im/element-ios/issues/524),
  but Element says no. They exist on Web, not on iOS. Same if you [wanted to see seconds](https://github.com/vector-im/element-ios/issues/3901)
* I almost forgot, but the [new spaces](https://element.io/blog/spaces-the-next-frontier/)
  just [don't exist on iOS](https://github.com/vector-im/element-ios/issues?q=label%3AA-Spaces+),
  should you attempt to join or be invited to one, you will get a banner
  saying that they aren't implemented yet and you cannot accept or reject
  the invite unless you open Element Web to do that.
* Another issue I am editing in hours later is pills, when you mention
  someone on Element (Web), or someone else mentions someone, there is a clear
  pill shape around their name and it can be clicked to get to their profile,
  [but not on Element (iOS)](https://github.com/vector-im/element-ios/issues/3526)

And that is probably enough of annoyances with Element iOS, I hope the
situation will improve in foreseeable future there due to
[Matrix exploding with Element securing $30M funding to revolutionise the app’s usability, build out major new features, expand in the enterprise market and take Matrix fully mainstream!](https://element.io/blog/element-raises-30m-as-matrix-explodes/)

# You mentioned privacy?

Yes, privacy is a big reason why Matrix is advertised and the lack of it is
a fact you agree to by using Matrix or getting bridged to Matrix (which is
out of scope for this blog post as it involves other protocols too much,
whether you know Matrix or not).

As with the internet in general, the most safe assumption is that once you
post something it's there forever. It may be encrypted in a private Matrix
room or it may be public in a public room, but it will most likely be there
forever.

Matrix does support [history retention if you are advanced enough to enable it](https://brendan.abolivier.bzh/matrix-retention-policies/),
this assumes [your homeserver explicitly enables it as it's not default](https://github.com/matrix-org/synapse/blob/ba5287f5e8be150551824493b3ad685dde00a543/docs/sample_config.yaml#L481-L484)
and as your room is hosted on every homeserver that has users in your room,
have a single homeserver that hasn't explicitly enabled it and the room
history never goes away. (If I am wrong, [please contact me](/discuss) as
I have one private room where history goes away after 7 days, but another with the
same configuration (nowadays 31 days though), which I can scroll as far back
as I want.)

*Technical note: sorry about calling reference homeserver implementation issue
 as a Matrix protocol issue.*

You may say that this requires you to trust the homeserver admin anyway and
that is true, I wish people could trust each other and even if someone
modified their Synapse to never remove anything or had a client logging
everything, they wouldn't throw that history to people who don't want to see it.

Speaking of removals, once you remove a message [it will be stored in the database for server admins for 7 days](https://github.com/matrix-org/synapse/blob/ba5287f5e8be150551824493b3ad685dde00a543/docs/sample_config.yaml#L456-L461) which is fine for me, but if [this message happened to be media instead of text, it would never be removed](https://github.com/matrix-org/synapse/issues/1263) and should you have copied link to the media, it would keep on working
and if you changed the homeserver address in your copied link, it would still
keep on working. Is this something you expect from a private protocol? I don't, or I didn't before getting familiar with Matrix. There is also an [alternative proposal about this](https://github.com/matrix-org/matrix-doc/pull/2228).

*By the way Synapse is still a reference homeserver implementation and not
 Matrix protocol itself, so sorry about that for anyone technical reading this.*

Do you use different names in different contexts? Like your Full Name in
professional context, a nickname somewhere else and maybe what will be your
real name after gender transitioning or even have a diffferent name in direct
chat with your partner? [Congratulations, whatever is your latest room-specific name is public, same with your potential avatar](https://github.com/matrix-org/synapse/issues/5677).

*Synapse didn't become Matrix protocol itself by the way, there are still other implementations!*

This issue does have a potential solution [an API planned for room specific details (2015)](https://github.com/matrix-org/matrix-doc/issues/545)
<s>and what I am hopeful about in the future [open pull request specification for space specific profiles](https://github.com/matrix-org/matrix-doc/pull/3189),
unless it just moves the issue to a different level.</s> Which got [cancelled or delayed for an undefined time period](https://github.com/matrix-org/matrix-doc/pull/3189#issuecomment-905761797),
["until extensible profiles and sync v3 become more concrete"](https://github.com/matrix-org/matrix-doc/pull/1769)

2021-08-27: I don't know how serious issue this may be for you, but any emoji/
[reactions made on end-to-end-encrypted messages aren't encrypted](https://github.com/matrix-org/matrix-doc/issues/2678).
It's fun in [E2EE test rooms](matrix:r/megolm:matrix.org?action=join) when you cannot read the other party, but
regardless see their reactions on your emssages.

I think that was my biggest complaints on Matrix (or Synapse itself), that
don't involve other protocols and I have personally experienced. My notes
for this blog post include [Matrix not having real contacts list](https://github.com/matrix-org/matrix-doc/pull/2228),
but they didn't occur to me and I guess it has been doing fine enough without
implementing those.

If any of these issues is a dealbreaker for you or you don't want to hear
a bad word about Matrix, you may be wondering what is the perfect flawless
solution? I don't know, personally I don't think it may not exist and I don't
want to enter discussing compromise solutions or other protocols in this post
at all. This list also wasn't complete on what issues I have with Matrix
(and so close to the end I don't want to dig for references) and I have
specific wishes that no protocol offers (at least not consistently,
such as using multiple names and knowing which name I am using where or managing
50 different rooms with same operators everywhere, but [that may get answered by Matrix](https://github.com/matrix-org/matrix-doc/pull/2962).)

You may wonder was it nice of me to write so negative blog post. I find it
therapeutic as [I have had an issue to me to write this since 2021-01-15](https://github.com/Mikaela/mikaela.github.io/issues/230)
and now I have finally done it, a bit over half an year late,
spending a bit over an hour to it and I feel better after getting these problems
out of my head and maybe they weren't so big after all. Up to you.

Lastly I apologise to you-know-who-you-are for not titling this post "undefined",
or even M.UNKNOWN (which I would have imagined to be one of the issues for me to write about, but
I don't remember seeing it in a long time, so maybe the situation is improving.

Feedback? I have [a discussion room in many apps](https://mikaela.info/discuss),
or you can find me from a lot of the linked issues and there is also [issue tracker for this site](https://github.com/Mikaela/mikaela.github.io/issues).

* [Changelog, also known as git commit history](https://github.com/Mikaela/mikaela.github.io/commits/master/blog/_posts/2021-08-03-matrix-perfect-privacy-not.md)
  * Clicksaver for edits done on day of publishing: I have fixed a typo resulting one
    link being a 404 error, added mention on Element (iOS) not doing URL previews
    and later added pills not being supported by it either. I didn't consider
    [outdated emoji picker](https://github.com/vector-im/element-ios/issues/4654)
    worth mentioning here, but it came up in the same context as URL previews
    and wasn't reported to upstream, so I might as well mention it in this part.
  * 2021-08-27: Noted cancellation/delay of space-specific profiles,
    mention emoji/reactions not being encrypted at all, added link to E2EE
    test room and this list item.
  * 2021-09-09: It's brought to my attention that URL previews exist on Element
    iOS! It's 23.15 in Finland so I only strikethrough this issue.