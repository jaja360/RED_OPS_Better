# RED_OPS_Better

Introduction
------------
Autonomous transcoder for Redacted or Orpheus
--

red_ops_better is a script which automatically transcodes and uploads these
files to Redacted, Orpheus or DIC.

The following command will scan through every FLAC you have ever downloaded or
uploaded (if it is in , determine which formats are needed, transcode the FLAC
to each needed format, and upload each format to Redacted, Orpheus or DIC, automatically.

    $ ./red_ops_better

Installation
------------

You're going to need to install a few dependencies before using
red_ops_better.

First and foremost, you will need Python 3.6 or newer. NOTE: this version
has been ported to Python 3.x and will not run under Python 2.x.

Once you've got Python installed, you will need a few modules: mechanize,
mutagen, and requests. Try this:

    $ pip3 install -r requirements.txt
	
If you are on a seedbox, or a system without root priviliages, try this:

    $ pip3 install --user -r requirements.txt

Furthermore, you need several external programs: mktorrent 1.1+, flac,
lame, and sox. The method of installing these programs varies
depending on your operating system, but if you're using something like
Ubuntu you can do this:

    # apt install mktorrent flac lame sox

If you are on a seedbox and you lack the privilages to install packages,
you could contact your provider to have these packages installed.

Configuration
-------------

You've made it far! Congratulations. Open up the file `config` in a text editor.

- `username` and `password` are your Redacted and Orpheus login credentials(Not required if using session_cookie). 
- `session_cookie` working session cookie for tracker of your choice.
- `data_dir` is the directory where your downloads are stored. 
- `output_dir` is the directory where your transcodes will be created.
    If the value is blank, `data_dir` will be used. You can also specify per format
    values such as `output_dir_320` or `output_dir_v0`.
- `torrent_dir` is the directory where torrents should be created (e.g.,
    your watch directory). Same per format settings as `output_dir` apply.
- `formats` is a list of formats that you'd like to support.
- `media` is a list of lossless media types you want to consider for
    transcoding. The default value is all What.CD lossless formats, but if you
    want to transcode only CD and vinyl media, for example, you would set this
    to 'cd, vinyl'.
- `24bit_behaviour` defines what happens when the program encounters a FLAC 
    that it thinks is 24bits. If it is set to '2', every FLAC that has a bits-
    per-sample property of 24 will be silently re-categorized. If it set to '1',
    a prompt will appear. The default is '0' which ignores these occurrences.
- `tracker` is the base url to use in the torrent files.
- `api` is the base url to use for api requests.
- `source` source flag for tracker.
- `piece_length` used with MKtorrent default value.

Usage
-----

```
usage: red_ops_better [-h] [-s] [-j THREADS] [--config CONFIG] [--cache CACHE]
                      [-U] [-E] [-S] [-t TOTP] [-T TRACKER]
                      [release_urls [release_urls ...]]

positional arguments:
  release_urls          the URL where the release is located (default: None)

optional arguments:
  -h, --help            show this help message and exit
  -s, --single          only add one format per release (useful for getting
                        unique groups) (default: False)
  -j THREADS, --threads THREADS
                        number of threads to use when transcoding (default: 3)
  --config CONFIG       the location of the configuration file (default:
                        $PROJECT_FOLDER/config)
  --cache CACHE         the location of the cache (default:
                        $PROJECT_FOLDER/cache)
  -U, --no-upload       don't upload new torrents (in case you want to do it
                        manually) (default: False)
  -E, --no-24bit-edit   don't try to edit 24-bit torrents mistakenly labeled
                        as 16-bit (default: False)
  -S, --skip            treats a torrent as already processed (default: False)
  -t TOTP, --totp TOTP  time based one time password for 2FA (default: None)
  -T TRACKER, --tracker TRACKER
                        pick your tracker for transcoding (choose from RED or
                        OPS) (default: RED)
```

Examples
--------

To transcode and upload every snatch you've ever downloaded along with all
your uploads (this may take a while):

For RED

    $ ./red_ops_better

For OPS

    $ ./red_ops_better -T OPS

To transcode and upload a specific release (provided you have already
downloaded the FLAC and it is located in your `data_dir`):

For RED:

    $ ./red_ops_better "https://redacted.ch/torrents.php?id=1391069&torrentid=3095859"

For OPS

    $ ./red_ops_better -T OPS "https://orpheus.network/torrents.php?id=137293&torrentid=269985"

Note that if you specify a particular release(s), red_ops_better will
ignore your configuration's media types and attempt to transcode the
releases you have specified regardless of their media type (so long as
they are lossless types).

Your first time running red_ops_better might take a while, but after it has
successfully gone through and checked everything, it'll go faster any
consecutive runs due to its caching method.

If you like to use this for OPS as default. Open red_ops_better and search for
"default='RED'" and replace RED to OPS.
