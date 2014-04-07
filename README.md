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

Create a new post with the rake task `new_page` e.g.

```
  rake new_page ../path/to/file.mp3
```

and it will generate the metadata for the podcast feed. It will not upload it.