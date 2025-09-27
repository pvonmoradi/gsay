# gsay
A simple shell script to fetch pronunciation of an English word from Google.

When you search for an Enlgish vocabulary, Google shows a pronunciation in an
"answer box" at top of the page. This script fetches that _hopefully human-made_
mp3 sound file.

## Features
- Query for 2020 and 2024 sound files
- Support British and American accents
- Cache to disk (enabled by default)

## Dependencies
- `curl`
- `ffplay | mpv | pw-play` : A headless mp3 player, one is enough

In a Debian-like distro, these can be installed with:

``` shell
sudo apt install curl ffmpeg # or mpv
```

## Usage
Check `gsay -h`:

```console
Usage: gsay [OPTIONS] QUERY
DESCRIPTION
    A simple client to fetch/play pronounciation of words from Google

ARGUMENTS
     QUERY
        Query string
        Note: Use "" or '' if query string contains special shell characters
OPTIONS
    [--year | -y]
        Set desired database year: 2020 (default) | 2024
    [--accent | -a]
        Set desired accent: gb (default) | us
    [--link | -l]
        Only print pronounciation link. Don't play their audio
    [--no-cache | -n]
        Disable cache mode: ignore cache and don't save to cache directory
        Default: cache mode is enabled. dir: /home/username/.cache/gsay
    [--debug | -v]
        Increase log verbosity to debug
    [--version | -V]
        Print version
    [--help | -h]
        Display the help message

EXAMPLES
    gsay legend
    gsay -y 2024 -a us Leicester
    gsay -n -y 2020 -a gb Forte
    gsay Pneumonoultramicroscopicsilicovolcanoconiosis
    gsay -l perfect
    gsay carte blanche
```

## Notes
- I may be wrong but the `2024/04/19` pronounciations sound synthetic to me!
  Hence, `2020/04/29` is default despite being slower and less
  exhaustive. Caller scripts can run it like `gsay -y 2020 || gsay -y 2024`
- The HTTP URLs are faster, HTTPS ones are provided as comments too
- Fun examples:
`echo Supercalifragilisticexpialidocious Antidisestablishmentarianism Grandiloquent | xargs -n1 gsay`
- [Oxford
  3000](https://www.oxfordlearnersdictionaries.com/about/wordlists/oxford3000-5000)
  [word list](https://github.com/sapbmw/The-Oxford-3000) pronunciations, limited
  to first 1000 words, can be downloaded (cached) to disk like this:

``` shell
curl -sL https://github.com/sapbmw/The-Oxford-3000/raw/refs/heads/master/The_Oxford_3000.txt \
    | tr -d \r | head -1000 | xargs -I {} gsay -l "{}"
```


## Development
- The early versions of this script supported a scraping mode besides the
  current heuristic mode. But, recently Google has been preventing scrapers from
  accessing its search results page. I tested their "Custom Search JSON API"
  from "Programmable Search Engine", but it doesn't provide the output from
  Answer Box that contains pronounciations
- linter: `shellcheck`
- formatter: `shfmt -i 4 -bn -ci -sr`

