# pod5.io source

http://pod5.io
Jekyll site for Pod5
BSD Licensed.

## Setup

```
  git clone https://github.com/pod5/pod5.github.io.git`
  cd pod5.github.io`
  bundle install
  jekyll serve -w
```
  
#### Niceness

Create a new post with the rake task `new_page` e.g.

```
  rake new_page ../path/to/file.mp3
```

and it will generate the metadata for the podcast feed. It will not upload it.