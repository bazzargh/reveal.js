These are the slides of a talk I wrote for induction into our team,
and subsquently presented at the Edinburgh Devops meetup (http://www.meetup.com/Edinburgh-DevOps-Meetup/)

Because this was originally internal, there's some background that
may be useful, which the target audience would have known. We're a deployment support team,
but also deal with some first-line sysops requests from ~150 developers. We work in 2
slack channels, one where all tech discussion takes place, and another for non-technical
private team chat. We have one person 'on point' (we've been trying to avoid 'on call' since the
talk was given, because this is not an out of hours rota) and a number of others working on
internal tooling. This talk is aimed at the person on point.

I had been asked to explain how I knew the answers to so many of the questions we're
asked in our channel, and it's about a way of working. We don't use our knowledgebase
(Confluence) much because it produces dead pages; problems are hopefully ephemeral as
they get fixed or worked around, but with > 100 microservices, all depending on different
library versions, we keep hitting those same problems again and again through upgrades.
Searching in chat is more effective.

Now some slide notes...

# Wrong

The first reaction when a question comes in to slack should not be to stop, think and investigate.
The person at the other end will not know you are doing this, and will start forum shopping
to find someone who _will_ respond, dragging more people in. Also your teammates may not know
you are investigating, and we get the other half of the problem: dogpiling, when multiple people
are trying to help out. We have other things we should be doing!

# Better

Respond first, eg "looking into that". _Then_ investigate.

# Select Player One

If you're not on point _do not answer immediately_. You need to give the person on point a chance
to answer, that's why they're there; you have other tasks assigned. 

If you are on point but are too busy with other support requests to help out, _say this_. That
way you release the others in the team to offer help.

# WWHD

You've got a problem in production, how should you handle it? Think - What Would House Do?

He takes a history of the patient, then _speculatively_ treats, then later in the episode
he'll finally come up with a fuller diagnosis. He does not wait for investigation to complete
because then the patient would be dead.

# WWRD

In deployment support, we're in a similar position: What Would Roy Do? Once you've captured
information about what's wrong, and preserved anything that you might have to to investigate
the problem later, your first instinct should be to roll back the deploy, or revert the change
and redeploy, unless you are fairly certain you have an immediate fix. Downtime is money.
Turn it off and turn it on again! Once the fire is out, you can diagnose the problem at your
leisure.

# Clues

To find out what went wrong, your first step should be to ask the developer (they may often
tell you nothing changed. This usually isn't true. There may have been data changes, or the
act of deploying may have flushed caches and revealed a problem with an _earlier_ change)
Next, look at the deploy log, it's fairly detailed, and check splunk for errors. If you can't
tie the problem to a code change from that, use the error messages you do have in google/slack.
As we'll see, the answers _should_ be there in slack.

# Think out loud

As you investigate, don't go dark. Mention what you tried that failed. Mention what you've
figured out so far. This encourages others to collaborate and provides better information
when someone tries to solve the same bug later on.

On this slide I mentioned actionable knowledge; this is because a few times I've seen
investigations head down blind alleys where (eg) someone tried to figure out how often
we were having the problem, or eg what is the average response time, which are not things
you can do much with. More useful information is when did the problem start (ties it to a
deploy or other events), or in the second case, what proportion of requests exceed a
target time? (average response time does not matter as long as we are meeting response
time targets, if it doesn't we want to deploy more servers)

# Ask for help.

It's ok to ask for help if you're stuck. Do it in public, we want those responses
to be searchable too. Part of the job is learning who the squeaky wheels are who
know about various systems - if there's a good individual to ask you usually get
a faster & better response than if you just throw a question out into a team room.

# Show your working

Once you've solved the problem, explain what it was and how you solved it in the room.
Try to give enough context that someone searching in future will find this, so
not "oh, I solved that problem earlier, adding X fixed it". Instead, "I solved the problem
where the frobnicator responded "foo bar baz" to every query, it's bug no #964 upstream.
Adding X to the config in blah/blah.yml fixes it"

# END

And if you work this way, building a knowledgebase in chat, people will think you
have psychic powers. All you've done is extend your memory by leveraging chat as an
ad-hoc knowledgebase.
