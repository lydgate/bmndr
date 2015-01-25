bmndr
=====

Simple python CLI to [Beeminder](https://www.beeminder.com/) which uses the [Beeminder API](https://www.beeminder.com/api). I live in the command-line so this script makes it really simple for me to check my goals and enter data without waiting for a webpage to load. This is my version of laziness.

It is very simple but I'm a hobbyist programmer so there may be bugs. It works for me and I use it every day. I'd be really happy if you use it so [let me know](https://twitter.com/bryankam) if it's useful for you or if I can improve it!

Requirements
============

* A [Beeminder account](https://www.beeminder.com/users/sign_up) account. Get one, they are free and you won't regret it.

Usage
=====

Usage is simple. First, log into Beeminder, [get an auth_token](https://www.beeminder.com/api/v1/auth_token.json), then create a ~/.bmndrrc file with the following contents:

    [account]
    auth_token: <auth_token>

Running the command with no arguments will show you all of your goals in order of how close you are to losing. Here's an example for [my Beeminder goal](https://www.beeminder.com/bkam/oto) of watching the films in the book [_1001 Films You Must See Before You Die_](http://msls.net/films/):

    $ bmndr
    ...
    oto        14 safe days   Way above the yellow brick road with 14 days of safety buffer
    ...

Get more data on a specific goal by using the slug as an argument. It gives you the headline summary as well as a summary of the graph, plus the URL to the graph, and the last ten datapoints entered.

    $ bmndr oto
    ::: Progress on 1001 Films :::

     Way above the yellow brick road with 14 days of safety buffer 
     618 on 2013.07.17 (3 datapoints in 2 days) targeting 1001 on
    2020.12.01 (2694 more days). Yellow Brick Rd = +1 / week. 

    http://brain.beeminder.com/nonce/bkam+oto+51e59450cc1931715100000c.png

    16 616 "initial datapoint of 616 on the 16th"
    16 1 "Nanook of the North"
    17 1 "Onibaba"

Enter a datapoint by adding further arguments after the slug. The format is the same as beeminder the Beeminder emails except that quotes aren't necessary. The script prints the server's response, pretty-printed.

    $ bmndr oto 1 Out of Africa
    {'canonical': '17 1 "Out of Africa"',
     'comment': 'Out of Africa',
     'id': '51e685eecc193104d800008c',
     'requestid': None,
     'timestamp': 1374076800,
     'updated_at': 1374062062,
     'value': 1.0}

You can also tell Beeminder to refresh a goal by using the following syntax:

    $ bmndr refresh <slug>

Configuration
=============

Currently there are only a few options. `auth_token` is required as described above.

`filter` can be set to one of `frontburner` (default), `all`, or `backburner`. Definiton of backburner [here](http://blog.beeminder.com/glossary/#b).

Ideas
=====

You could put the default output into [Conky](http://conky.sourceforge.net/) or another tool that can display text. I do this to have an at a glance view of my goals on my desktop.

If you use [Taskwarrior](http://taskwarrior.org/projects/show/taskwarrior), you can declare a [backlog](http://markforster.squarespace.com/blog/2009/8/31/backlog-method.html). First create a weight loss goal with the current number of your tasks in. Then put something like this into cron:

    0 9 * * * bmndr backlog $(task status:pending entry.before:2013-07-17 count) automatic import from taskwarrior

This will upload your daily total tasks created __before__ 2013-07-17 to Beeminder. The reason I set a date was so that I could make progress on old tasks without being penalized for creating new tasks.

You can also Beemind disk space usage on Linux pretty easily:

    0 9 * * * bmndr disk $(df <list of partitions> | awk '{s+=$3} END {print s/1024/1024}}') automatic import from df

This will upload the amount of diskspace used by the partitions you specify. This could be useful if you're trying to clear out space and want to Beemind it.

Todo
====

* More configuration.
  * Make the number of goals displayed by default configurable.
* Nicer formatting of the JSON that comes back when you submit data.
* Silent switch for scripts when posting data.
* Verbose switch for summary.
