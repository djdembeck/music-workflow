# Getting Started

## Installation

For the purposes of this guide, Docker is used for every part of the process. I will be using [Linuxserver.io's beets](https://hub.docker.com/r/linuxserver/beets) image.

For other install directions, see the [official beets install guide](https://beets.readthedocs.io/en/stable/guides/main.html#installing).

## Config

**It's best to read the [beets documentation](https://beets.readthedocs.io/en/stable/guides/main.html#configuring) to understand every config option you'll want.** As a quick start, you can see my [config.yaml.example](config.yaml.example), which contains comments explaining every section.

Not mentioned in the `config.yaml`:

- [Duplicates](https://beets.readthedocs.io/en/stable/plugins/duplicates.html)

    This plugin will notifiy you if you are importing an album that already exists in your library, and give you some useful options (example, remove old or skip new)
- [MBSync](https://beets.readthedocs.io/en/stable/plugins/mbsync.html)

    This is useful for updating tags as they are fixed in the MusicBrainz. Just be aware, changing files that have already been scanned into Plex can cause a myriad of problems.
- [Missing](https://beets.readthedocs.io/en/stable/plugins/missing.html)

    This plugin will notify you which or how many tracks are missing from a release you are importing. Useful for finding broken/incomplete albums in your collection.

## Running

**Skip to [importing](#importing) if you aren't using Docker.**

1.  ### Starting the container
    I am using unRAID, but this is what my `docker run` command looks like:

    ```
    docker run --name=beets --env=HOME=/config --env=BEETSDIR=/config --volume=/mnt/disks/nvmepool/appdata/beets:/config:rw,slave --volume=/mnt/user/Music/Tidal-tosort/:/downloads:rw --volume='/mnt/user/Music/Plex Music/FLAC/:/music:rw' --network=bridge -p 8337:8337 --restart=no linuxserver/beets
    ```
    - `/config` stores necessary config files.
    - `/downloads` is where we will be importing from.
    - `/music` is where files will be moved to when processed.
    - The networking part is only if you plan on using the `web` plugin for beets.

2. ### Entering the container shell

    Once the container is running, enter the container's terminal with

    ```
    docker exec -it beets /bin/bash
    ```

# Importing

Whether you are starting from scratch, modifying, or revamping an existing structure, **your config options will need to look different** (copy not move, don't write, etc). **So make sure you have gone over the [config](#config) section above before proceeding.**

## Beginning the import process
Now for the simple part!

```
beet import /downloads
```

The information it gives you next, will be overwhelming at first! Just know organization and future simplicity are worth the learning curve.

## Reading `import` messages:

### A match that has a good default match, and is telling us some file data will be changed:

```
/downloads/new/Album/$NOT/Beautiful Havoc [2020] (12 items)
Tagging:
    $NOT - Beautiful Havoc
URL:
    https://musicbrainz.org/release/b0eae07d-1eed-4657-b61d-518ffa5e6a5c
(Similarity: 95.6%) (tracks, country) (Digital Media, 2020, XW, 300 Entertainment)
* "Life"                       -> Life
* Who Do I Trust               -> What Do I Trust (title)
* Like Me (feat. iann dior)    -> Like Me (title)
* 4                            -> Four (title)
* Sangria (feat. Denzel Curry) -> Sangria (title)
Apply, More candidates, Skip, Use as-is, as Tracks, Group albums,
Enter search, enter Id, aBort?
```

This is data that we ***want*** to be changed, since [featured artists belong in the artist field](https://musicbrainz.org/doc/Style/Artist_Credits#Featured_artists), not the title field.

Type `a` and then `enter` to apply

### A match that has good data in the default match:

```
/downloads/new/Album/Hundred Hands/Little Eyes (6 items)
Tagging:
    Hundred Hands - Little Eyes
URL:
    https://musicbrainz.org/release/1061f939-86d5-4aeb-b540-114b67bf20ab
(Similarity: 97.7%) (media) (CD, 2001, US, Deep Elm Records, DER-393)
[A]pply, More candidates, Skip, Use as-is, as Tracks, Group albums,
Enter search, enter Id, aBort?
```

This one is an easy decision, we will type `a` to apply the match/data.

### A release with no matches

```
/downloads/new/Album/Angelic Montero/You Get Me [2020] (8 items)
No matching release found for 8 tracks.
For help, see: http://beets.readthedocs.org/en/latest/faq.html#nomatch
[S]kip, Use as-is, as Tracks, Group albums, Enter search, enter Id, aBort?
```

Oh no! This message means nothing matched closely enough based on our [config](#config) filters. [This release needs to be added to MusicBrainz](musicbrainz/adding-a-release.md). Be my personal hero and add it!

Once you have added or found the release on MusicBrainz, use the ID option, by doing `i` and then `enter`. Then, paste the new release URL you just added into the terminal, and press `enter`. Beets will show you a revised screen for this release:

```
Enter release ID: https://musicbrainz.org/release/75d807b8-4745-4ff6-8c43-50b2b10e0ae9 
Tagging:
    Angelic Montero - You Get Me?
URL:
    https://musicbrainz.org/release/75d807b8-4745-4ff6-8c43-50b2b10e0ae9
(Similarity: 100.0%) (Digital Media, 2020, US, Republic Records)
[A]pply, More candidates, Skip, Use as-is, as Tracks, Group albums,
Enter search, enter Id, aBort?
```

Much better. Type `a` and then `enter` to apply.

### A match with bad or missing data

```
/downloads/new/Album/Anna Of The North/Believe [2020] (5 items)
Correcting tags from:
    Anna Of The North - Believe
To:
    Anna of the North - Believe
URL:
    https://musicbrainz.org/release/3b1dce9d-25a7-4ce9-b861-681bef0d3af4
(Similarity: 100.0%) (Digital Media, 2020)
[A]pply, More candidates, Skip, Use as-is, as Tracks, Group albums,
Enter search, enter Id, aBort?
```

Here, country and label are missing. A missing record label will limit Plex's sorting abilities. [This release needs to be fixed or added to MusicBrainz](musicbrainz/adding-a-release.md) like the above example.

### Viewing additional candidates, when the top match is not what you want (data or medium wise):

```
/downloads/new/Gila - Bury My Heart At Wounded Knee (German) (7 items)
Correcting tags from:
    Gila - Bury My Heart At Wounded Knee (German)
To:
    Gila - Bury My Heart at Wounded Knee
URL:
    https://musicbrainz.org/release/908efe2f-307b-4577-ba6a-d121fb29113d
(Similarity: 92.7%) (country, media, year, album) (CD, 1995, LU, Germanofon, 941031)
 * In A Sacred Manner -> In a Sacred Manner
Apply, More candidates, Skip, Use as-is, as Tracks, Group albums,
Enter search, enter Id, aBort? m
```

This is a vinyl release, so we should select `m` to see if a vinyl version exists:

```
Finding tags for album "Gila - Bury My Heart At Wounded Knee (German)".
Candidates:
1. Gila - Bury My Heart at Wounded Knee (92.7%) (country, media, year, ...) (CD, 1995, LU, Germanofon, 941031)
2. Gila - Bury My Heart at Wounded Knee (92.5%) (media, country, album) (12" Vinyl, 1973, DE, Warner Bros., WB 46234)
3. Gila - Bury My Heart at Wounded Knee (88.5%) (tracks, missing tracks, country, ...) (1973, DE)
4. Gila - Bury My Heart at Wounded Knee (84.4%) (tracks, missing tracks, year, ...) (CD, 2000, DE, Garden of Delights, CD 046)
# selection (default 1), Skip, Use as-is, as Tracks, Group albums,
Enter search, enter Id, aBort? 2
```

Option #`2` is a German vinyl release, so that looks like a near-exact match.

```
Correcting tags from:
    Gila - Bury My Heart At Wounded Knee (German)
To:
    Gila - Bury My Heart at Wounded Knee
URL:
    https://musicbrainz.org/release/498491e0-4a34-4bd2-888f-10ada27ddc61
(Similarity: 92.5%) (media, country, album) (12" Vinyl, 1973, DE, Warner Bros., WB 46234)
 * In A Sacred Manner -> In a Sacred Manner
Apply, More candidates, Skip, Use as-is, as Tracks, Group albums,
Enter search, enter Id, aBort? a
```

Type `a` and then `enter` to apply.

## Useful commands:

- Import: `beet import /downloads`

- Update beets database: `beet update`

    This will remove orphan database entries (files you removed), and pull in changes made to MusicBrainz.

- Submit acoustic data to AcousticBrainz: `beet absubmit`

    [At the moment](https://github.com/beetbox/beets/issues/2861), this needs to be done manually and can take a significant amount of time, depending on the size of your library.

- Sync MusicBrainz changes to file data [**(Dangerous!!)**](#config): `beet mbsync`

    While this is an incredible tool to keep data accuracy, I only reccomend doing this yearly, or at any point where you have the time to spot-check Plex for issues.