title: sandman: A Boilerplate-free Python REST API for Existing Databases
date: 2013-07-23 00:28
categories: python rest sandman

At work on Friday, I found myself wishing there was a simple way to
automatically generate a HATEOAS-based service with a REST API from an existing
database. I've lost count of the number of times I've "decided" to build a REST
API for a legacy database only to give up when I realized just how much
boilerplate ORM code I'd need to write.
<!--more-->
On Friday and Sunday I spent a few hours finally writing a tool that does exactly
what I want. I knew that [SQLAlchemy](http://www.sqlalchemy.org) had database introspection 
capabilities. I also knew that [flask](http://flask.pocoo.org) had an extension
that allowed for easy integration with SQLAlchemy. After marrying these two
facts and hacking up some ugly code, my new side-project was born.

**"[sandman](http://www.github.com/jeffknupp/sandman) makes things REST."**

## Batteries Included

`sandman` requires an astonishingly small amount of code to drop a RESTful
service in front of a legacy database. Using a subset of the [chinook](http://chinookdatabase.codeplex.com)
test database as an example, here's all the code you'd need to write to get your
service up:

    #!py
    from sandman import app, db

    app.config['SQLALCHEMY_DATABASE_URI'] = '<your DB's SQLAlchemy connection string>'

    from sandman.model import register, Model

    class Artist(Model):
        __tablename__ = 'Artist'

    class Album(Model):
        __tablename__ = 'Album'

    class Playlist(Model):
        __tablename__ = 'Playlist'

    register((Artist, Album, Playlist))

    app.run()

**Yep. That's it.**

With that code, you'll get a set of HTTP endpoints for both collections (i.e.  'artists') and 
resources (i.e. '/artists/125'). Collection endpoints support HTTP `GET` and `POST`,
while resource endpoints support HTTP `GET`, `POST`, `PATCH`, `PUT`, and `DELETE`. All
HTTP status codes returned should be correct (i.e. `PATCH` on an existing
resource returns `204`, but `201` on a non-existent resource). `PATCH` is idempotent, `POST` isn't, and `rel` and `uri` links are returned (though `rel: self` is the only one supported at the moment).

## But wait, there's more! (Updated 8/21)

Want a Django-style admin interface? You got it. Simply add a single line
(`model.activate_admin_classes`) to get the smooth sweetness you see here:

![sandman admin screenshot](/images/admin_tracks_improved.jpg)

There's also support for user-defined validation on a per-HTTP method basis, the
ability to choose supported HTTP methods on a per-resource basis, and custom
endpoints.

Here's what a simple HTTP request to the service looks like:

    #!bash

    ~jknupp: curl localhost:5000/artists/275                                            
    127.0.0.1 - - [23/Jul/2013 01:05:01] "GET /artists/275 HTTP/1.1" 200 -
    {
    "ArtistId": 275,
    "Name": "Philip Glass Ensemble",
    "links": [
        {
        "rel": "self",
        "uri": "/artists/275"
        }
    ]
## Towards a Level 3 RESTful API

There's clearly still a lot more work to do, but far less than when this post
was originally written. A very useful skeleton is in place, and there are only
two pieces of functionality I consider essential before I would recommend using
sandman in production: support for authentication and automatic generation of
`rel` links to related resources (by finding foreign keys via introspection,
which I've already done for the admin interface). 

I've already begun work on HTTP authentication, so hopefully a true beta release 
isn't that far off.

## Wrapping Up

`sandman` is the type of side-project I love: small, simple, and useful. While
it's still a WIP and shouldn't be used for anything important (or at all,
really), the GitHub repo is open, the tests are being run via TravisCI, and the
`Issues` tab is open for business. Hopefully, someone out there finds it useful.
Either way, I had fun writing it this weekend.

*Note: I am by no means a REST API or HATEOAS expert, so if any of the information above is wrong or unclear, please feel free to correct me in the comments.*
