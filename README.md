# Newtorrent

This is my take on [docktorrent](https://github.com/kfei/docktorrent),
This container does not have a web frontend.  To access rtorrent you can use /RPC2 or the terminal

Using [Docker](https://www.docker.com/),
[rTorrent](http://rakshasa.github.io/rtorrent/), and
[Nginx](http://nginx.org/en/) near full-featured
BitTorrent box.

## Highlights

  - All-in-one Docker container, build once and run everywhere.
  - Newest version of rTorrent, with support of DHT and
    asynchronous DNS which will result in a more responsive rTorrent.
  - Get a working BitTorrent box in less than 3 minutes, give it a quick try
    and tune the configs later.
  - rTorrent will automatically restarts on crash or freeze.
  - No more boring installation, also keep your OS in a clean state.

## Quick Start

Clone this repository and build the image locally:
```bash
git clone https://github.com/theduncan/newtorrent
cd newtorrent
docker build -t newtorrent .
```

After the image is built, run the newtorrent container, for example:
```bash
docker run -it \
    -p 80:80 -p 45566:45566\
    --dns 8.8.8.8 \
    -v /data-store:/rtorrent \
    --name newtorrent
    newtorrent
```

Note that:
  - The exposed ports are required for rTorrent listening and the DHT protocol 
    according to the default `.rtorrent.rc`.
  - The `--dns 8.8.8.8` argument is optional but recommended. It seems like the
    current version of rTorrent still has some [DNS
    issues](https://github.com/rakshasa/rtorrent/issues/180), using Google's
    DNS may help.
  - The `/data-store` volume is for all downloads, torrents and session data,
    just make sure the disk space is enough.
  - Override the `upload_rate` setting of rTorrent to 1024KB. Check the full list of
    available [runtime configs](#runtime-configs).

Happy seeding!

## Runtime Configs

There are some environment variables can be supplied at run time:
  - **LOGS_OFF**: Set this to `yes` to turn off all logs generated by rTorrent
    and other services so that you don't have to worry about space for
    `/var/log`. Default is not set.
  - **AUTH_OFF**: Disable HTTP authentication on certain network. E.g.,
    `192.168.1.0/24` or `all`.

Override settings in `.rtorrent.rc`:
  - **IP** overrides `ip`.
  - **MAX_PEERS** overrides `max_peers`.
  - **MAX_PEERS_SEED** overrides `max_peers_seed`.
  - **MAX_UPLOADS** overrides `max_uploads`.
  - **DOWNLOAD_RATE** overrides `download_rate`.
  - **UPLOAD_RATE** overrides `upload_rate`.

## Tips

  - `docker stop` can gracefully shutdown rTorrent if you give it more time by
`docker stop -t 120` (which means 120 seconds to time out).

## Feedback

Bug reports and feature suggestions are both welcome. Feel free to use the
[issue tracker](https://github.com/theduncan/newtorrent/issues).
