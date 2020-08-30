---
title: "Personal Spotify API Bot"
date: 2020-04-10
tags: [API]
excerpt: "A program that helped me create a quarterly Spotify playlist out of randomized songs from my music library."
---

## Introduction
So I have an odd ritual where I would create current favorite songs Spotify playlist
4 times a year. This process grew to be very long as I have about 1200+ songs saved
which I would shuffle play and manually add to a playlist and then slowly narrow
down to my favorite songs at that time.

Growing tired and frustrated I decided to speed this process up by one click by creating
a program that utilized Spotify API calls that did most of the work for me.

##Methods
The first method, **createPlaylist()**, will simply create a new empty playlist to
be used in Spotify to store all my saved music.

Next, **checkLibrary()** will return the total amount of saved songs in the user
music library. To be used to compare with the final result.

**loadSongs()** will load all songs from the user music library into a list and randomize
it. Since Spotify API only allows reading up to 20 tracks at a time, calls will keep
being made until entire library is read thanks to checkLibrary(). After tracks are
in a list in raw format another for loop will of through the raw list and grab just
the track ids and load them into another list to be used in the next method.

**addToPlaylist()** add the list of randomized songs into the newly created playlist
in groups of 20 since Spotify API only allows up to 20 songs per query.
