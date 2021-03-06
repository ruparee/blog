title: Python, sandman2, and Open Data
date: 2016-09-12 09:13
categories: python sandman2 opendata

[`sandman2`](https://github.com/jeffknupp/sandman) (and its predecessor, `sandman`) has been far and away my most
successful open source project. I fully attribute this to its genuine usefulness. It is most often used as a command
line tool, through which you provide the connection details of a legacy database and run it, and in return it starts
both a RESTful API service for your data, as well as a web-based UI that allows you to add, delete, and edit rows
directly. For many (especially in the enterprise), interacting with legacy databases is a pain at best, and impossible
at worst. The ability to access data via a simple REST API, then, is a godsend.
<!--more-->
But what about **outside** the enterprise? Given my position at [Enigma](http://enigma.io), how could an organization
wanting to open its data make use of `sandman2`? At first, the answer seems obvious: just run `sandman2` as is, and let
would-be consumers of the data access it via the API.

That *would* work. But it's clunky and overly time consuming (especially given that there is a limit of only twenty resources per request, and your table may have one million rows). More than anything, folks who want access to open data want access to _all_ of it. The first thing they'll do, if it's a possibility, is to take a dump of all of the data. They almost never interact with it programatically at its source. Analysis is done elsewhere.

So forcing those folks to get twenty resources at a time is silly. They should be able to hit a single endpoint and get
**all** of the data. And it should be in a format they're used to (like, say, CSV) rather than JSON.

Now they can do exactly that.

By adding eleven lines of code, `sandman2` got the ability to "export" a collection to CSV. By simply adding the
`?export=true` URL parameter, export mode is triggered, and *all* of the data is available in one go. It's fast, simple,
and above all, just works.

**So if you're an organization looking to "free" your data and make it available to the world but don't have the tools to do so, well, you do now.**

Simply download and run `sandman2`, point users at it, and check off that pesky "give back to the data community" box
off of your to-do list. This trivial act may not make much difference to *you*, but there are whole communities out
there that care **a lot** about this stuff, and you'll be doing them a favor and making yourself look good in the
process.

## Aside

I would be remiss if I didn't highlight just how important [Enigma](http://enigma.io) was in getting this functionality
added. No one at the company suggested this to me, but just working there has brought data and its availability to the
front of my mind. I'm mostly just ashamed it took me this long to realize there was something *I* could do to help out
the folks in the data community. Regardless, if even a single data set is made public based on `sandman2`, I'll consider
it a win. After all, eleven lines of code is still just eleven lines of code.
