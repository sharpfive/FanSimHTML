
Thes are some swift command line apps I use to hack on my roto baseball league.

No warranty or claim of usability what-so-ever. This represents work I did in my free time for my own enjoyment. It is not intended to represent production quality work. Many sins lurk within, not the least of include:

- Domain logic not fully baked. This could be considered a decent start but I haven't examined enough actual use cases to determine how they should actually be constructed
- Hard coded files names. These could eventually be legit command line tools, but that's a ways off. I'm the only person using this thing.
- Comments, logs and debug code sprinkled throughout.
- Hard coded files point to a directory on my dev machine. It would be cool if someone else could run the most recent projections, but not today.
- Duplicated code. This code has prioritized getting things done so I don't annoy my wife by hacking on Swift on my days off.
- Non uniform output. Some scripts output to a csv file, some print to the standard output. So it goes.
- Error handling non-existent. If the file formats change, things just don't work and you have to dig a bit to find out why.
- Not many unit tests.



You need to have Swift installed on your machine to run this. Most people do this by downloading Xcode and installing the command line tools. Consult google for more directions.


That said, here are the apps:


### ESPNScrape

Aborted attempt to log in to my ESPN site and scrape data. Not much interesting here.

### FreeAgentFinder

Scrape a list of the League's rosters and compare that to projected values to determine the best available free agent.

### LeagueRostersScrape

* Scrape league roster data from a copied-and-saved text file from a **Rosters** page from an ESPN league.
* Imports projected auction values for hitters and pitchers from a csv
* Correlates the roster players with their auction value and prints out a primitive power ranking.

See sample directory for expected formats. Downloadable and especially ESPN formats are likely to change in the future. They were last known to work March, 2019.

swift run LeagueRostersScrape --hitters data/FanGraphs-batters-2019-03-16.csv --pitchers data/FanGraphs-pitchers-2019-03-16.csv --rosters data/ESPN-rosters-2019-05-29.txt

### PlayerRelativeValues
Scrapes a csv of team rosters and compares them to csv's of project Fangraphs auction values. This show the relative value of a player.

swift run PlayerRelativeValues --hitters data/FanGraphs-batters-2019-03-16.csv --pitchers data/FanGraphs-pitchers-2019-03-16.csv --auction data/PlayerRelativeValues.auctionvalues.csv

### ProjectionsToAuctionValues
Takes a csv of raw stat projections and converts them to auction values. It uses an algorithm similar to [this site](https://www.friendswithfantasybenefits.com/2019-rankings-and-auction-values-using-z-score-and-steamers). Note: my code was written before this blog post, but I didn't invent the algorithm anyway.

### ProjectionsToAuctionValuesPitchers
Turns out things like ERA & WHIP need to be calculated differently (a lower value is better), so a different script is used to calculate pitcher values.

### HitterAuctionValues

swift run HitterAuctionValues --hitters data/Zips-projections-2020-batters.csv --output data/Zips-projections-2020-batters-auction-values.csv

### TeamRelativeValues

This takes a scrape of the league rosters and adds the values for all the players on their team. It is used to estimate which teams have the best chance of winning. Turns out I can often predict the league winners before the season even starts.

swift run TeamRelativeValues --hitters data/FanGraphs-batters-2019-03-16.csv  --pitchers data/FanGraphs-pitchers-2019-03-16.csv --auction data/TeamRelativeValues.auctionvalues.csv

### RotoSwift

Common code that is shared between all the scripts.
#### Math
Some methods for calculating statistics like Mean, Z Score and Standard Deviation

#### Models
First pass at domain models. Mostly light weight swift structs.

#### Repository
Class used to turning data from various file formats to usable Models.

#### Roster
Code to build up the league rosters from a scraped text file. It's used by FreeAgentFinder.

#### How do I compile the apps?
_swift build_

#### How do I run one of the apps?
* Change the filename variables in the swift app to point to something on your local machine
* _.build/debug/TeamRelativeValues_ to run a file from your working directory

To build and run an app in one command

_swift build && .build/debug/PlayerRelativeValues_

Run unit tests (not there are many... shame on me)

_swift test_


### How can I hack on this?

Go ahead. You can edit the files in your text editor of choice. If you use Xcode and add new files, you'll need to have swift update the xcodeproj 

_swift package generate-xcodeproj_

Note: this isn't needed for other editors than don't use the xcodeproj.


### What is aiai?

Something that should be cleaned up. If something like that was committed to a production repository I'd feel ashamed, but this is not one of those...




