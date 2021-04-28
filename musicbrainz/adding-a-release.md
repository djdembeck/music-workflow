# Best practices

Before going any further, there are some essential things you should always be thinking about and abiding by, when adding a release:

- Extra Title Information

    You may be used to having information in album (release) and track titles, such as 'clean', 'deluxe edition', 'feat. Artist' or 'EP'. According to [official style guidelines](https://musicbrainz.org/doc/Style/Titles), this is largely wrong, except for outliers. Since MusicBrainz has data types and fields that handle all this, it's much cleaner to not have it in the title.

    If 'deluxe edition' (or version variants) or 'EP' are not listed on the releases cover art, they do not belong in the release title. Instead, put them in the [disambiguation](https://musicbrainz.org/doc/Disambiguation_Comment) field.

# Digital importers

No one wants to do manual data entry. So thank the maker, there are some tools written by the community to handle the bulk of data entry:

- [a-tisket](https://atisket.pulsewidth.org.uk/) - **Recommended**

    This is arguably the most powerful import tool. It checks 3 streaming platforms (Spotify, Deezer and iTunes), comparing the data between them for a release. It provides incredibly rich data, with minimal amounts of user intervention. [Originally by marlonob](https://community.metabrainz.org/t/a-multi-source-seeder-for-digital-releases/444000), [forked by atj](https://community.metabrainz.org/t/a-multi-source-seeder-for-digital-releases/444000/189?u=chilkara). *[Read the how-to.](#using-a-tisket)*

- [Discogs for Tampermonkey (by murdos)](https://github.com/murdos/musicbrainz-userscripts/blob/master/discogs_importer.user.js)

    This importer is great for utilizing Discogs' massive library of data. Keep in mind, this doesn't import all necessary data, so some extra spot checking is required. Install it with [Tampermonkey](https://raw.githubusercontent.com/murdos/musicbrainz-userscripts/master/discogs_importer.user.js) and *[Read the how-to.](#using-discogs-for-tampermonkey)*

- [List of various other importers](https://wiki.musicbrainz.org/User:Colbydray/UserscriptList)

## Using a-tisket
So, you took my advice. Good lad. Let's jump into it:

### Sourcing release links

The easiest (and often best maintained/separated) store is Spotify. You don't need a subscription to use their [search](https://open.spotify.com/search).

Keep in mind, there may be more than 1 version of an album (deluxe, explicit/clean, etc), so make sure you find the specific album version you want to add.

Now, paste that album URL into the `Get ID` field of the a-tisket page. In my usage, I also change `Preferred countries` to just `US`. Press `Search` and wait for the resulting page to load.

### Release aggregate page

This page has a LOT of data on it, so I'm going to break it down so it's not too overwhelming:

- Artwork selection

    In the perfect case, 3 versions of the same artwork will be displayed at the top of the page. The quality is (typically) as follows:

    iTunes>Deezer>Spotify

    On older releases, Deezer may have better quality artwork, so always check if the iTunes art is poor quality/size.

    Save the best artwork, and move to the next section.

- Overview of release info

    The page will show album name, artist (with respective store and MusicBrainz links/icons) and then various release data. Most of the time, you needn't pay attention to this, except maybe:

    - TYPE - Stores may list releases differently (one may list it as EP, another as album).
    - TOTAL TRACKS - It's rare this will differ, but if it does, there will be a red text next to it warning you about differing track lists.
    - BARCODE (UPC) - Spotify my erroneously add 2 0's in front of a UPC, causing an error about differing UPC.
    - RELEASE DATE - It's common for stores to get rights to an album at different dates. If that's the case, multiple dates will be shown here. If one has '01-01' instead of a specific date, use another date with the `Change date` button. It's also frowned upon to submit a digital release with a date before the digital store existed (so any old release).
    - Release group - This only appears when the tool is confident which group the release should be placed in. It's not foolproof, so always make sure you aren't creating a new release group (quick and easy check, is press search button on final release page for release group).

- Tracklist

    Pretty self-explanatory, but it's good to check title differences, and artist credit differences.

- Link relationships

    This section is showing you what links it will be adding to the MusicBrainz links section.

- Release events

    There's not much to actually review here, other than making sure the country you want is in this list. The list is a combination of all region availablity, as given from the store's API.

- Notes

    You can edit here what the annontation and edit note will be. Typically, I **uncheck** the `Copyright notice` box, since this information is already mirrored in release label a large part of the time. It's also good to use `Countries excluded` where possible, to have a shorter annontation.

    The edit note can be left as is.

- Language

    This tool defaults to English. If the release is another language, change it here (or on the final release page). If the language you changed to [does not use the Latin alphabet](https://en.wikipedia.org/wiki/List_of_writing_systems), uncheck the `script is latin` box, and change the script on the final release page.

Now, press the big button!

### MusicBrainz add release page with a-tisket data

The fields here are covered in the [MusicBrainz Add release page breakdown](). A few key things you should check for data accuracy:

- Release information

  - Release information

    - Title

        Make sure the title uses the [correct title case](https://musicbrainz.org/doc/Style/Language/English). An easy breakdown/tool to do this is mentioned in the [best practices section]()

    - Artist

        The field should be green, meaning it's linked. If not, click the search button, and see if there is an obvious match. If not, you may need to [add the artist](adding-a-artist.md) Make sure any featured artists are credited correctly here.

    - Release Group

        This should also be green, unless no existing group for this release is available (do your due dilligence).

  - Release event

      - Date

          Again, make sure this date looks accurate.

      - Label

          This field will not be green. Press the search icon next to the field to find the appropriate label. Please read the [best practices for labels]() section to ensure you are using the right types of labels.

  - Additional information
    
      - Disambiguation

          If an album is clean, deluxe edition, etc, [put that information](https://musicbrainz.org/doc/Disambiguation_Comment), in lowercase, here.

- Release duplicates

    This tab is good to check, to make sure you aren't adding an exact duplicate. But don't check any of the buttons on this page.

- Tracklist
  
    - Tracklist

        Here, there are multiple things you want to be checking:

        - Track title accuracy and case, see [best title practices]()
        - Artist field includes all featured artists (a-tisket may drop all this data, so always double check the store page(s)), and all fields are green. See [best artist practices]() to make sure you catch everything.
        - 

    - Guess case

        No need to mess with this.

- Recordings

    ***Don't create duplicate recordings!!!***
    
    The purpose of the recordings tab, is so you can use existing recordings with a new release. For example, if a single was release by this artist, you want to use the same recording on the album, so they are linked.

    - Tracklist

        Go through each track, and click the edit button next to it. This brings up a search dialog, and more often then not, the recording you are looking for will be the first item in the list. If there are none, leave the option as add new recording (only if you have done due dilligence).

        This is also a good chance to check if your track data differs from the existing recording's data and why. It's ok for the data to be different if there are valid reasons. A glaring issue would be different featured artists, which may indicate you are missing something, or using the wrong recording. Again, check the [best recording practices]()

    - Options
  
        No need to mess with this.

### a-tisket post-add release page

- Complementary links

    - Covers

        - Click `Upload cover to MusicBrainz`
        - On the MusicBrainz page, click `Select images...`, and select the artwork you downloaded earlier.
        - Check the `Front cover` checkbox (it's the very first checkbox).
        - Put the source in the comment area, ie `iTunes`
        - Click `Enter edit` and wait for the upload to finish.

    - Submit ISRCs

        An [ISRC](https://en.wikipedia.org/wiki/International_Standard_Recording_Code) is a unique identifier for track/recordings. a-tisket uses another script to submit ISRCs directly from Spotify.
        
        - Click `Refresh bearer token to submit new data`.
        - It will probably show a blank page. Just go back to the previous page (I press back 2 times), or close and re-open this tab.
        - Verify the Spotify titles and MusicBrainz titles are similar/the same.
        - Press `Submit` at the bottom of the page, waiting for the button to say `Submitted`.

    - Artist relationships

        This section is to help future users using this tool. By adding Spotify/Deezer/iTunes links to an artist page, the tool can automatically link artists on the add release page. This is optional, but helpful.

## Using Discogs for Tampermonkey

# Manual entry (I cri)
