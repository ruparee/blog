title: Your Database Just Got Its Own Website
date: 2013-12-20 14:20
categories: python sandman database rest

**Update 12/21: Sandman can now activate the REST API and admin interface without your listing your database tables! With five lines of code, you get those two services, plus your browser will automatically open to the admin interface when you start `sandman`. This means that by simply inserting your DB host, username and password and running sandman, you get the admin interface and a RESTful service. There is literally nothing more that you need to do.** 

I briefly posted about my [sandman](https://www.github.com/jeffknupp/sandman)
library before, but today it got even cooler. To recap, `sandman` is a service that sits
in front of an existing, legacy database and provides a REST API for it, all
without requiring any tedious boilerplate code. It's pretty amazing to see firsthand, when someone
throws `sandman` in front of a giant legacy database in their enterprise and
starts interacting with it using cURL. You can check out the documentation at [ReadTheDocs](https://sandman.readthedocs.org/).

Even better, starting today, *`sandman` supports filtering in HTTP requests* (as all good REST APIs do). What does that mean and
how does it work? Glad you asked.
<!--more-->
Previously, if you issued a `GET` request to `sandman`, you would get one of
three things:

* A single resource (because your URI included the primary key)
* All resources of a certain type (because it didn't include the primary key)
* An error, because the resource wasn't found

But what if I wanted to filter for a subset of all the resources? Many APIs
allow filter parameters to be sent in the query string. For example, if I want to
find "AC/DC" but I don't know their primary key in the "Artists" table, I should be able to send a
request like this:

    #!bash
    $ curl "localhost:5000/artists?Name=AC/DC"

After all, `sandman` *is* backed by a database, isn't it (yes, it is; **your database**)? Being able to add
`WHERE` clauses is insanely useful. Even more useful is to be able to issue
compound clauses using multiple columns for filtering.

**Sandman now supports this out-of-the-box, with no extra effort on your part.**

![sandman filtering screenshot](/images/new_sandman.jpg)

That's right. By simply defining what tables from your database you want
`sandman` to make available via REST (or admin GUI), you get searching/filtering
for free. Think of how powerful that is:
    
**You can now query any legacy database using cURL**.

And remember, `sandman` also gives you the beautiful admin interface, shown here:

![sandman admin screenshot](/images/admin_tracks_improved.jpg)

It's pretty amazing. If you have a legacy database in your organization,
regardless of size, you can throw `sandman` at it and get an awesome RESTful
service along with a kick-ass admin interface, **without writing any boilerplate ORM code**.

Now that I have `sandman`, I find myself using it for small databases just as often
as large ones. **Did you know your Google Chrome data is stored in an sqlite database? Now it has a GUI! The contact list on your phone? Here, have a REST API!**
Honestly, the list of cool stuff you can do with `sandman` is endless.

It goes without saying that `sandman` is still actively being developed. We (the
Data team at AppNexus) just decided to use it for a large project we're working
on, replacing a Java-based REST service in about 1/100th the number of LOC. Of
course, since `sandman` is so straightforwardly written (and has *100% test
coverage*), adding filtering capabilities *only took five lines of additional code*. 
Honestly. FIVE. [Here's proof](https://github.com/jeffknupp/sandman/commit/37619a4ccdb2d75d629cf63644f63ebae09227cf).

If your organization is thinking about using `sandman`, let me know! I have a
lot of features I'd like to add, but I'll prioritize the ones that others
actually *want* to use. And feel free to contribute changes to the source as
well. Just send me a pull request.
