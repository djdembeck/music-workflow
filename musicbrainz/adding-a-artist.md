# Introduction
MusicBrainz already has [guidelines for artist data](https://musicbrainz.org/doc/Style/Artist), which I recommend reading for any amount of serious editing. This guide will stick to the more important points and less of the fine print.

## Tools/scripts
- Wikipedia importer

    Make your life a lot easier, and install the '[Create artist from Wikipedia data](https://bitbucket.org/loujine/musicbrainz-scripts/raw/default/mb-edit-create_from_wikidata.user.js)' plugin for Tampermonkey, by loujine. This will fill in a lot of info for you when the artist you're adding has a Wikipedia page. It can also fill in data for existing artist MusicBrainz entries.

# 1. Important data for every artist
Since this guide is focusing on data currently or planned to be used by Plex, I will give you the quick and dirty of what the bare-minimum is, but also what you should be adding when you have time.

- Sort name:

    I'm not going to [re-invent the wheel](https://musicbrainz.org/doc/Style/Artist/Sort_Name) here, but if someone has a first and last name, sort name should always be 'last, first'.

- Disambiguation:

    If there are any artists with the same or very similar name, put short, detailed information in the disambig section. Something like 'American rapper', or 'Lo-fi artist from ChilledCow'. Basically anything that quickly distinguishes this artist from another of the same name.

- Type/gender:

    These are oftentimes very easy to determine, so don't skip out on them.

- Area:

    This is the *country* where the artist is most associated with. If an artist has a city in this field, Plex likely won't recognize the country.

- External Links

    These ensure Plex has a static link, instead of fuzzy matching, especially when the artist name has special characters. As of right now, there are 2 critical links for perfect metadata:

    - Last.fm
  
        This is the track popular data, secondary artist image source, and secondary artist bio source. With no link on MusicBrainz, Plex reverts to fuzzy matching based on name.

    - AllMusic

        This is the album critic rating, album review, artist bio, similar artists, styles, genres and moods source. So, obviously, you want this. Again, with no link, Plex defaults to fuzzy matching.

# 2. Optional data

# 3. Extra data