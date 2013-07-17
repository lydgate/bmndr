bmndr
=====

Simple python CLI interface to beeminder.com

Requirements
============

* Python 3. It won't run on Python 2 currently due to capitalization differences (ConfigParser) and urllib differences.

Usage
=====

Usage is simple. First, log into Beeminder, [get an auth_token](https://www.beeminder.com/api/v1/auth_token.json), then create a ~/.bmndrrc file with the following contents:

    [account]
    auth_token: <auth_token>

Running the command with no arguments will show you five goals in order of how close you are to the wrong lane:

    $ bmndr
    ::: oto :::	(1001 Films)
     Way above the yellow brick road with 14 days of safety buffer 
     618 on 2013.07.17 (3 datapoints in 2 days) targeting 1001 on
    2020.12.01 (2694 more days). Yellow Brick Rd = +1 / week. 

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

Enter a datapoint by adding further arguments after the slug. The format is the same as beeminder the Beeminder emails except that quotes aren't necessary:

    $ bmndr oto 1 Out of Africa
    {'canonical': '17 1 "Out of Africa"',
     'comment': 'Out of Africa',
     'id': '51e685eecc193104d800008c',
     'requestid': None,
     'timestamp': 1374076800,
     'updated_at': 1374062062,
     'value': 1.0}

Todo
====

* Make the number of goals displayed by default configurable.
* Possibly allow monitoring other people's public graphs
* Nicer formatting of JSON that comes back when you submit data.
* Automatically 
