# pod5.io source

* http://pod5.io
* Jekyll site for Pod5.
* Very simple & BSD Licensed. Good for forking and making your own Podcast with. Lots of work is done to make it config based rather than hardcoded.

## Setup

```
  git clone https://github.com/pod5/pod5.github.io.git`
  cd pod5.github.io`
  bundle install
  jekyll serve -w
```

Site config is held in `_config,yml` and Podcast specific data in `_data/podcast.yaml`
There is a presumption that you will use feedburner to collect RSS stats.

#### Niceness

Create a podcast post with the rake task `new_podcast` e.g.

```
  rake new_podcast ../path/to/file.mp3
```

and it will generate the metadata for the podcast feed. It will not upload it.

To get metadata about pods in a standardised way, use:

```
  rake pod_stats JGORegExpBuilder JTSImageViewController NBThemeConfig
```

It will output to the terminal.


### Credits

We use the font [Gota Light](http://fontfabric.com/gota-free-font/) from Font Fabric.
We use the song [Dolce Vita](http://www.jamendo.com/en/track/1078823/dolce-vita) by [Auquid](http://www.jamendo.com/en/artist/433381/auquid) for the intro.