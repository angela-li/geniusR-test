Black Panther: Kendrick + Friends
================
Angela Li
February 9, 2018

So I've been wanting to try out Josiah Parry's geniusR package for a while, and I hadn't found a good occasion to use it.

Until today, when Kendrick Lamar released [his album](https://open.spotify.com/album/5sOSzueqgCiVpXNcpd6QpL) for the upcoming movie *Black Panther*. Apparently he was asked to only record a few songs for it, but he ended up producing the entire album. I'm for it.

In the spirit of the original inspiration for the geniusR package, the release of Kendrick Lamar's album **DAMN.**, here goes a try with *Black Panther*.

``` r
# Load libraries
library(geniusR)
library(tidyverse)
library(tidytext)
```

Let's get some lyrics!

``` r
bp_song <- genius_lyrics(artist = "Kendrick Lamar", song = "Black Panther")

bp_song %>% 
  mutate(line = row_number())
```

    ## # A tibble: 40 x 2
    ##    text                                                              line
    ##    <chr>                                                            <int>
    ##  1 Wait                                                                 1
    ##  2 King of my city, king of my country, king of my homeland             2
    ##  3 King of the filthy, king of the fallen, we living again              3
    ##  4 King of the shooters, looters, boosters, and ghettos poppin'         4
    ##  5 King of the past, present, future, my ancestors watching             5
    ##  6 King of the culture, king of the soldiers, king of the bloodshed     6
    ##  7 King of the wisdom, king of the ocean, king of the respect           7
    ##  8 "King of the "                                                       8
    ##  9 opps that miss it                                                    9
    ## 10 " and dreamers that go and get it"                                  10
    ## # ... with 30 more rows

Time for a tracklist!

``` r
bp_tracks <- genius_tracklist(artist = "Kendrick Lamar", album = "Black Panther")
```

So putting `artist` = `"Kendrick Lamar"` and `album` = `"Black Panther"` doesn't actually work, because Kendrick worked with a few other folks on this album, including The Weeknd and SZA, to name a few stars.

To make this work, what we want to do is look up the album on Genius and basically take the URL without dashes. The only reason I know this is because I may have been the one to suggest this feature.

``` r
(bp_tracks <- genius_tracklist(artist = "Kendrick Lamar The Weeknd And Sza", album = "Black Panther The Album Music From And Inspired By"))
```

    ## # A tibble: 14 x 2
    ##    title                                                         track_n
    ##    <chr>                                                           <int>
    ##  1 Black Panther by Kendrick Lamar                                     1
    ##  2 All The Stars (Album Version) by Kendrick Lamar & SZA               2
    ##  3 X by ScHoolboy Q, 2 Chainz & Saudi                                  3
    ##  4 The Ways by Khalid & Swae Lee                                       4
    ##  5 Opps by Vince Staples & Yugen Blakrok                               5
    ##  6 I Am by Jorja Smith                                                 6
    ##  7 Paramedic! by SOB x RBE                                             7
    ##  8 Bloody Waters by Ab-Soul (Ft. Anderson .Paak & James Blake)         8
    ##  9 King's Dead by Jay Rock, Kendrick Lamar, Future & James Blake       9
    ## 10 Redemption Interlude by Zacari                                     10
    ## 11 Redemption by Zacari & Babes Wodumo                                11
    ## 12 Seasons by Mozzy, Sjava & Reason                                   12
    ## 13 Big Shot by Kendrick Lamar & Travis Scott                          13
    ## 14 Pray For Me by The Weeknd & Kendrick Lamar                         14

Sweet.

This is sort of a pain to type, so I'll store the strings instead.

``` r
kendrick <- "Kendrick Lamar The Weeknd And Sza"
bp <- "Black Panther The Album Music From And Inspired By"

(bp_tracks <- genius_tracklist(artist = kendrick, album = bp))
```

    ## # A tibble: 14 x 2
    ##    title                                                         track_n
    ##    <chr>                                                           <int>
    ##  1 Black Panther by Kendrick Lamar                                     1
    ##  2 All The Stars (Album Version) by Kendrick Lamar & SZA               2
    ##  3 X by ScHoolboy Q, 2 Chainz & Saudi                                  3
    ##  4 The Ways by Khalid & Swae Lee                                       4
    ##  5 Opps by Vince Staples & Yugen Blakrok                               5
    ##  6 I Am by Jorja Smith                                                 6
    ##  7 Paramedic! by SOB x RBE                                             7
    ##  8 Bloody Waters by Ab-Soul (Ft. Anderson .Paak & James Blake)         8
    ##  9 King's Dead by Jay Rock, Kendrick Lamar, Future & James Blake       9
    ## 10 Redemption Interlude by Zacari                                     10
    ## 11 Redemption by Zacari & Babes Wodumo                                11
    ## 12 Seasons by Mozzy, Sjava & Reason                                   12
    ## 13 Big Shot by Kendrick Lamar & Travis Scott                          13
    ## 14 Pray For Me by The Weeknd & Kendrick Lamar                         14

``` r
(bp_album <- genius_album(artist = kendrick, album = bp, nested = FALSE))
```

**Why doesn't this work???**

Oh, it's because each of the tracks has different artists. Aah, my suggestion of using the URL only works if each of the songs on the album is by the same person.

I think what we need to do is use rvest to actually scrape URLs from the page, instead of hacking together a URL that works 95% of the time.

Guess this doesn't work for compilations. Time to submit an issue.
