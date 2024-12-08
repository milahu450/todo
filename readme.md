# milahu/todo

public todo list of milahu

i have started so many things...  
i would need 100 people to finish them all



## todo



### print a million copies of my book

print a million copies of my book  
[Pallas. Who are my friends. Stable groups by personality type](https://github.com/milahu/alchi)  
and give them away for free to many different people  
in many different places.

the printing cost of one book is around 1 euro,  
so to finish this project,  
i would need at least 1 million euros only to cover the printing costs,  
assuming i do the printing myself.  
if i would outsource printing to a company,  
then the printing costs would roughly double.

i do everything myself:  
writing, translating, uploading, printing, binding, distributing.  
currently, my biggest limiting factor is time.



### git forges should add the commit object to archive files

git forges: github, gitlab, gitea/forgejo/gogs, sourcehut?, cgit?

gitea: [archive endpoint should provide commit object gitea#17834](https://github.com/go-gitea/gitea/issues/17834)

the gitea maintainer did not see the value in my request.  
this made me angry, because to me, the value is so obvious.  
git is a [content-addressed store](https://en.wikipedia.org/wiki/Content-addressable_storage)  
so when i request content by its commit hash,  
then obviously, i want a way to verify the content by the commit hash.  
this feature is lacking in all git forges,  
because of the lame excuse "people dont need it",  
and "if people want to verify the content,  
then they should not use the archive endpoint"

in gitea#17834 i suggested to send the git commit object as http header

currently, i would prefer to add the commit object  
as a file into the archive, with the path

```
.git/objects/12/34567890123456789012345678901234567890
```

with the commit hash `1234567890123456789012345678901234567890`

the only problem this may cause is that  
some tools check for the presence of a `.git/` folder,  
and if a `.git/` folder exists,  
these tools assume that the workdir is a git repository

also, it may require more work,  
to make these archive files lossless,  
when compared to a `git clone`,  
because some files may be missing in the archive



### nix-build should be more verbose on cycle errors

- ["Cycle detected in the references of '/nix/store/...'" message could be more useful nix#481](https://github.com/NixOS/nix/issues/481)
- [feat: make cycle errors more verbose nix#6585](https://github.com/NixOS/nix/pull/6585)



### reverse-engineer jdownloader

jdownloader is advertised as "open source" software,  
but its core is closed source

every closed source part (binary blob, jar file) can contain malware

the "decode DLC" part is not the reason for closed source,  
because "decode DLC" is done online,  
which is also supported by open-source download managers like pyload

see also

- [Request for Package: 'jdownloader' nixpkgs#41321](https://github.com/NixOS/nixpkgs/issues/41321)
- [jdownloader2: init at 2.0 nixpkgs#136998](https://github.com/NixOS/nixpkgs/pull/136998)
- [pyload](https://github.com/pyload/pyload) - a truly "open source" alternative to jdownloader



### reverse-engineer talonvoice

talon obfuscation is probably similar to dropbox

https://news.ycombinator.com/item?id=13848035

> the encryption keys are not in the interpreter. **The interpreter is patched** to not expose co_code and more (to make this memory dumping more difficult; injecting an shared object is a different technique that I used too). It's also patched to use the different opcode mapping and the unmarshalling of pyc files upon loading them. However the key for each pyc file is derived from data strictly in those files themselves. It's pretty clear when you load up the binary in IDA Pro and compare the unmarshalling code with a standard Python interpreter's code

so probably the usual reverse-engineering tools (ida, ghidra, frida, ...) are more useful here

- [decompile py4 binary files - Bad MAGIC](https://github.com/zrax/pycdc/issues/316) ([archive](https://archive.is/qjC7l))
- [talon should be open-source (why is talon closed-source?)](https://archive.is/0uYOz) (issue was censored)
- [Writing and coding by voice with Talon](https://news.ycombinator.com/item?id=18793378)
- [To anyone tempted to get into Talon: note that it is very much not open source](https://news.ycombinator.com/item?id=32711410)



### convert the nixos wiki to a git repo on github

why? because im banned from nixos for "hate speech",  
and mediawiki is more centralized than git,  
so if the nixos wiki is a git repo, i can simply fork it

the nixos wiki provides a nightly export at https://nixos.wiki/images/wikidump.xml.gz  
via https://nixos.wiki/wiki/Sandbox

converting from wikitext to markdown is non-trivial  
because wikitext can contain templates  
which requires read-access to other wiki articles

ideally, this would use the original PHP code to render wikitext to html,  
and then (optionally) convert from html to markdown

see also

- https://github.com/philipashlock/mediawiki-to-markdown
- https://github.com/outofcontrol/mediawiki-to-gfm
- https://github.com/peterjc/mediawiki_to_git_md
- https://github.com/markc/alsa



### web browsers should load subtitle files from file protocol

~/bin/_yt_render_comments_html

```html
<video id="video" controls>
  <source src="video.mp4" type="video/mp4" />
  <track src="this-does-not-work.srt" type="text/srt" kind="subtitles" srclang="en" />
  <!-- workaround: data url for subtitle tracks -->
  <track src="data:text/srt;base64,..." kind="subtitles" srclang="en" />
</video>
```

this would be useful to download videos from youtube,  
including comments and subtitles,  
and rendering everything in a html file



### rich text translator

aka: translate-richtext, translate-html

done in [translate-richtext](https://github.com/milahu/translate-richtext)



### video release script

releasing a video is some work...



#### compress the video stream

aim for "YIFY" or "YTS" quality

[yify.md](https://gist.github.com/kuntau/a7cbe28df82380fd3467)

aka: ffmpeg-compress-video-yify

```
ffmpeg -i input.mkv -c:v libx264 -crf 27 -x264-params cabac=1:ref=5:analyse=0x133:me=umh:subme=9:chroma-me=1:deadzone-inter=21:deadzone-intra=11:b-adapt=2:rc-lookahead=60:vbv-maxrate=10000:vbv-bufsize=10000:qpmax=69:bframes=5:b-adapt=2:direct=auto:crf-max=51:weightp=2:merange=24:chroma-qp-offset=-1:sync-lookahead=2:psy-rd=1.00,0.15:trellis=2:min-keyint=23:partitions=all -c:a aac -ar 44100 -b:a 128k -map 0 output.mkv
```

> If you don't want audio encoding, just remove `-c:a aac -ar 44100 -b:a 128k`



#### compress the audio stream

1. try to use ffmpeg with libfdk_aac
2. try to use an external fdk_aac encoder like [fdkaac](https://github.com/nu774/fdkaac)
  - in nixpkgs: [fdk-aac-encoder](https://github.com/NixOS/nixpkgs/blob/master/pkgs/applications/audio/fdkaac/default.nix)
  - in arch linux: [fdkaac](https://archlinux.org/packages/extra/x86_64/fdkaac/)
  - in debian: [fdkaac](https://packages.debian.org/bookworm/fdkaac)
  - see also https://opensource.stackexchange.com/questions/2395/why-doesnt-ffmpeg-distribute-a-binary-with-libfdk-aac-enabled
  - see also https://unix.stackexchange.com/questions/98876/piping-sox-and-ffmpeg-together
3. fallback to ffmpeg with ac3. this should always work

https://trac.ffmpeg.org/wiki/Encode/HighQualityAudio

> When compatibility with hardware players does matter then use libmp3lame or ac3 in a MP4/MKV container **when libfdk_aac isn't available**.

when is libfdk_aac not available?

https://trac.ffmpeg.org/wiki/Encode/AAC

> FFmpeg supports three AAC-LC encoders (aac, libfdk_aac, aac_at) and two HE-AAC (v1/2) encoder (libfdk_aac, aac_at). The license of libfdk_aac is not compatible with GPL, so the GPL does not permit distribution of binaries containing incompatible code when GPL-licensed code is also included. Therefore this encoder have been designated as "non-free", and you cannot download a pre-built ffmpeg that supports it. This can be resolved by compiling ffmpeg yourself.

[fdk_aac license](https://en.wikipedia.org/wiki/Fraunhofer_FDK_AAC#Licensing)

https://opensource.stackexchange.com/questions/2395/why-doesnt-ffmpeg-distribute-a-binary-with-libfdk-aac-enabled

> This is a messy situation created by software patents.
>
> GPLv3 has anti-patent clauses
>
> libfdk_aac is under a license that does not grant a patent license. Users are required to obtain a patent license separately

https://superuser.com/questions/1367152/fraunhofer-fdk-aac-in-ffmpeg

> The FDK-AAC library is determined by FFmpeg to have a license compatible with the LGPL but not the GPL. The most commonly desired add-on to FFmpeg is the GPLed x264 library, so it's rare to find a publicly redistributable ffmpeg binary that has libfdk_aac linked.



#### add subtitles in some languages, maybe translate subtitles



#### properly downmix 6ch to 2ch audio

many old yify releases have broken 2ch audio  
because they used a wrong algorithm to downmix the audio track from 6ch to 2ch.  
that algorithm is wrong, because the "center" channel of the 6ch audio is too quiet  
so in the broken 2ch version, all speech is too quiet, and all ambient sounds are too loud

detect such broken 2ch audio tracks by correlating audio track and subtitles,  
and comparing the audio volume of speech parts versus non-speech parts



#### normalize audio volume

to make this faster, run this after downmixing to 2ch



#### properly scale the audio tempo with a "timestretch" filter

adapt audio tempo to video framerate with timestretch effect aka "rubberband"

use a high-quality (and slow) timestretch filter

https://superuser.com/questions/1683607/how-do-i-fix-audio-video-sync-in-ffmpeg

```
ffmpeg -i src.ac3 -c ac3 -b:a 192k -af rubberband=tempo=23980/24000 -y dst.ac3
```

or

```
ffmpeg -hide_banner -i src.ac3 -f sox - |
sox -t sox - -t sox - speed $(echo 24 25 | awk '{ print $1/$2 }') |
ffmpeg -f sox -i - -c ac3 -b:a 192k -y dst.ac3
```



#### add info on sources of video/audio streams



#### use a *.md file, not a *.nfo file



### seed more german torrents

the german torrent scene is dead since the website torrent.to was busted

but legally, this makes no sense. seeding torrents is "not really" illegal in germany  
and the copyright lawyers (copyright trolls) only want money from stupid people  
who out of fear sign the wrong documents, and then they must pay the damage repair of 1000 euros



### nix-build should not read all stdin

when i run `nix-build` in my terminal  
and enter some command like `echo hello`  
then nix swallows that command

expected:  
after `nix-build` is done,  
my command `echo hello` should be passed to the bash shell



### social media aggregator

aggregate social media feeds

p2p distributed web archive

based on ssb? based on git?

self-hosted, open-source, offline-first, caching proxy, ...

censorship-resistant, tamper-proof

gnupg signatures?

submit posts to multiple publishers ("platforms")  
and keep track of censored posts

bypass account-deletions:  
manage multiple accounts for one identity

use gnupg for proof of identity?  
but once my private key is stolen, all "proofs" are worthless...  
even if i can revoke my key in theory,  
in practice, this can fail easily

https://github.com/search?q=social+media+aggregator

https://github.com/TID-Lab/downstream

https://github.com/real-evolution/asma

https://github.com/jonas-kgomo/socal

https://github.com/milosa/social-media-aggregator-bundle

related: RSS reader, news reader



### sneakernet

future of filesharing

future scenario:  
censorship will become more aggressive.  
"copyright" laws will become more aggressive.  
"patent" laws will become more aggressive.  
captchas will become impossible to bypass,  
because captchas will require some government ID,  
which is linked to a "social credit score" system.



### stop using email webinterfaces

download all my emails, and delete them from the mail servers

use a lightweight selfhosted webinterface like squirrelmail,  
to browse all my downloaded emails

related: [social media aggregator](#social-media-aggregator)



### socks5 proxies p2p network

socks5 proxies with domain-whitelists to limit abuse

useful for web scraping

not needed? enough public proxies?

- https://github.com/TheSpeedX/PROXY-List
- https://github.com/monosans/proxy-list
- https://github.com/monosans/proxy-scraper-checker

its always a cat-and-mouse game between proxies and cloudflare



### python socks5 proxy rotate upstream proxies sessions

python proxy server rotate upstream proxies

socks5 proxy server manually rotate upstream proxies

https://github.com/search?q=socks5+proxy

socks5 proxy server upstream socks5 proxy

no. mitmproxy does not support socks5 upstream proxies.  
https://github.com/mitmproxy/mitmproxy/issues/211  
https://docs.mitmproxy.org/stable/concepts-modes/#upstream-proxy  
If you want to chain proxies by adding mitmproxy in front of a different proxy appliance, you can use mitmproxy’s upstream mode. In upstream mode, all requests are unconditionally transferred to an upstream proxy of your choice.  
mitmproxy supports both explicit HTTP and explicit HTTPS in upstream proxy mode. You could in theory chain multiple mitmproxy instances in a row, but that doesn’t make any sense in practice (i.e. outside of our tests).

https://github.com/mitmproxy/mitmproxy/issues/211#issuecomment-997330809

```
gost -L=socks5://localhost:LOCAL_PORT -F=socks5://someproxy.com:PROXY_PORT
mitmproxy --mode upstream:socks5@localhost:LOCAL_PORT
```

https://github.com/ginuerzh/gost#%E8%AE%BE%E7%BD%AE%E5%A4%9A%E7%BA%A7%E8%BD%AC%E5%8F%91%E4%BB%A3%E7%90%86%E4%BB%A3%E7%90%86%E9%93%BE

multiple upstream proxies = multiple forward proxies

```
gost -L=:8080 -F=quic://192.168.1.1:6121 -F=socks5+wss://192.168.1.2:1080 -F=http2://192.168.1.3:443 ... -F=a.b.c.d:NNNN
```

- https://github.com/mitmproxy/mitmproxy/issues/211#issuecomment-817197930
- https://github.com/mitmproxy/mitmproxy/issues/2813#issuecomment-764338432
- https://github.com/semigodking/redsocks

no. tinyproxy is a *http* proxy server

https://github.com/mitmproxy/mitmproxy/issues/211

<blockquote>

I found a workaround by using [tinyproxy](https://github.com/tinyproxy/tinyproxy) with the following configuration:

```
Port 8080
upstream socks5 localhost:8000
Allow 0.0.0.0/0
```

</blockquote>

todo: to rotate upstream proxy, simply restart the proxy server?

run a local socks5 proxy server  
which has a list of many upstream proxies  
and which allows to manually rotate these upstream proxies on demand  
so that only one proxy is used for each session

https://scrapingant.com/blog/how-to-use-rotating-proxies-with-puppeteer#rotate-proxy-servers-by-your-own

https://scrapingant.com/blog/python-proxy-server-proxy-py#how-to-make-rotating-proxies-with-proxypy

> Handling Proxy Rotation Frequency: You can add a mechanism to control the frequency of proxy rotation. For instance, you can set a minimum time interval between each rotation to avoid overloading the proxy servers.

aka: rotating proxy

https://github.com/kitabisa/mubeng -
1.3K stars.
An incredibly fast proxy checker & IP rotator with ease
Proxy IP rotator: Rotates your IP address for every specific request.

https://github.com/TeamHG-Memex/scrapy-rotating-proxies -
700 stars

https://github.com/nfx/slrp -
rotating open proxy multiplexer, 130 stars

https://github.com/danilopolani/rotating-proxy-python -
80 stars

https://github.com/constverum/ProxyBroker -
3.6K stars.
last commit 2019.

https://github.com/imWildCat/scylla -
3.8K stars.
Whenever an HTTP request comes, the proxy server will select a proxy randomly

https://github.com/topics/proxy-rotator

https://github.com/topics/proxy-rotation

https://github.com/markgacoka/selenium-proxy-rotator -
30 stars

https://github.com/oxylabs/selenium-proxy-integration-python

https://github.com/oxylabs/proxy-chrome-extension -
Chrome browser proxy extension that has all of the essential proxy session features in your browser

... or simple:
use CDP
([Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/))
(aka: chrome debugging protocol)  
to change proxy and useragent of a persistent chromium process.  
also clear cookies.  
maybe change the MAC address (does cloudflare see my MAC address?)  
https://github.com/cyrus-and/chrome-remote-interface/issues/152  
-> not supported by chromium

socks5 proxy in python

https://github.com/qwj/python-proxy

https://github.com/MisterDaneel/pysoxy



### get stackoverflow notification when my answer or question was deleted

todo: write a notification bot that watches all my questions and answers on all stackoverflow/stackexchange/... websites

stackoverflow, or more generally: stackexchange

stackexchange moderators use shadow-banning by default to avoid discussions, to avoid dirty work

"Send notification when question is closed or deleted" stackoverflow

https://github.com/openzim/sotoki  
StackExchange websites to ZIM scraper  
Sotoki (Stack Overflow to Kiwix) is an openZIM scraper to create offline versions of Stack Exchange websites such as Stack Overflow.  
https://github.com/openzim/sotoki/issues/306

https://github.com/Expecho/StackOverflow-To-Slack-Tag-Tracker  
Azure Function that acts as a bot which creates notifications in a Slack channel about StackOverflow activity based on the question tags

https://github.com/missmagenta/updates-notification-bot  
app for tracking content updates on GitHub and StackOverFlow services

[Send notification when question is closed or deleted](https://meta.stackoverflow.com/questions/258057/send-notification-when-question-is-closed-or-deleted)
+81
([archive.org](https://web.archive.org/web/*/https://meta.stackoverflow.com/questions/258057/send-notification-when-question-is-closed-or-deleted))
([archive.is](https://archive.is/8YGDT))

<blockquote>

By chance I found out that an old question of mine has been deleted.  
Given that it was very old and highly upvoted,  
I didn’t lose any reputation and—according to current standards—it was indeed off topic, so that is fine as well.  
However, I would have liked if I got a notification that someone voted to delete the question in the first place.  
Similarly to how you are notified when someone edits your posts.

Can we please add a notification to owners when their posts are close-voted or delete-voted?

tags: feature-request status-declined vote-to-close vote-to-delete notifications

---

we are already notified for comments, answers, upvotes, and downvotes;  
so it makes sense to be notified about the far more radical thing—close or even delete votes—too…

---

You can go put it somewhere more appropriate if you think it has long term value

---

Since when are we not able to do anything about a close/delete?  
What about editing and putting it in the reopen queue?  
Or asking why it was closed on a Meta?  
Or making a new, better question because your old one wont get answered?

---

That comment should be an answer.  
That was exactly the point I was thinking in my head...  
There's a lot that can be done with deleted posts,  
and that includes rallying a band of undelete voters to the rescue...

---

tl;dr:  
this idea is always appealing to people who don't answer support questions here on meta or via email,  
and have no clue just how many questions get deleted.

The naive implementation you suggest would generate entirely too much unproductive whining to be practical.  
You're welcome to suggest a process that would notify only people likely to take constructive action  
in only those cases where such action is possible - but as it stands, this idea is impractical and hence declined.

---

This is the same like not alerting on negative reputation.  
Why are you so afraid of "hurting our feelings"?  
Honestly if an alert saying your question is deleted is so hurtful, you have bigger problems in life.  
Turn it into a preference if you like,  
but some people do not care about notifications being negative or positive, they just want to be notified.

---

> Telling someone "we just took something away from you, and you can't have it back, and you can't do anything about this" is... uh, not a very nice thing to do.

So its better to not tell them? Thats even worse! Hiding actions and potential options because its going to be more work seems like a bad idea.  
This just means we need to do a better job of conveying to users what steps need to be taken to improve to question to reopen status

---

Rejecting a feature request for fear of whining is hardly a reason.

</blockquote>

[Notify users when their post is deleted or provide a way to view own deleted posts](https://meta.stackoverflow.com/questions/256160/notify-users-when-their-post-is-deleted-or-provide-a-way-to-view-own-deleted-pos)
+14

[Why is there no notification of your questions/answers being deleted?](https://meta.stackoverflow.com/questions/327552/why-is-there-no-notification-of-your-questions-answers-being-deleted)
-4

[How does deleting work? What can cause a post to be deleted, and what does that actually mean? What are the criteria for deletion?](https://meta.stackexchange.com/questions/5221/how-does-deleting-work-what-can-cause-a-post-to-be-deleted-and-what-does-that)
+524

[Did one of my questions (maybe older than the deleted recently page's period) get deleted recently and is therefore not shown on that page?](https://meta.stackoverflow.com/questions/391764/did-one-of-my-questions-maybe-older-than-the-deleted-recently-pages-period-ge)

<blockquote>

Why is there no kind of pro-active automated message in the Inbox that something (Q, A or comment) has been deleted so that it's not recognized just by chance (or maybe never)?

---

relevant [roomba](https://stackoverflow.com/help/roomba). Note that the link doesn't say Deleted recently, it says deleted recent questions. The difference is that it only shows questions that you have asked over the last 60 days but are now deleted. Questions that you asked in 2010 will not show up on your profile if they are deleted recently (in the lst 60 days). – 

</blockquote>



### release2torrent

copy releases to bittorrent

because bittorrent is the best technology for filesharing,
and if you need more security, then just use [bittorrent over i2p](https://geti2p.net/en/docs/applications/bittorrent)

todo more generic:
source releases from usenet, anti leech trackers, ftp servers, sneakernet, ... whatever

todo rename to:
release2torrent, warez2torrent, foss-debrid, p2p-debrid, p2p-leecher

maybe create a semi-automatic process  
where people can request och releases  
which are then downloaded and seeded from my machine

to avoid getting blocked for account sharing  
there should be a delay of some days between request and download  
so free users "pay" with long waiting times  
aka: high-latency mixnet  
https://security.stackexchange.com/questions/150232/logging-attack-tor-vs-mixnets  
https://security.stackexchange.com/questions/157506/high-latency-anonymous-communication-minimum-delay

och is a workaround for pussies, who are afraid of copyright trolls,  
but these copyright trolls have no real power (at least here in germany),  
and they only make money by pushing stupid people to sign some contract,  
where people promise to pay 1000 euros for "damage repair",  
but in an actual court of law,  
the copyright trolls could never prove any damage.

on the other hand, censorship is getting more aggressive every day,  
so there is a real risk that filesharing will become illegal in the future.  
for example, thepiratebay admins were jailed for helping wikileaks.

keywords:

- rapidgator2torrent
- ddl2torrent
- jdownloader2torrent
- pyload2torrent
- usenet2torrent
  - nzb2torrent
- leechers paradise
- leech2torrent
- steal2torrent
- share2torrent
- collaborative leeching
- all2torrent
- mirror2torrent
- fossdebrid
- foss-debrid
- p2p-debrid
- free premium link generator
- high-latency debrid proxy
- high-latency debrid mixnet
- high-latency filesharing mixnet

see also:

- [video release script](#video-release-script)
- [seed more german torrents](#seed-more-german-torrents)
- [sneakernet](#sneakernet)



### download managers should be file managers

download managers should be file managers primarily,  
and all the file transfer stuff should be secondary

because in the end, download managers produce files,  
and i want to organize these files into a tree of folders

moving files should be a first-class feature.  
when a download produces a folder of multiple files,  
and i have added extra files to that folder,  
then i want to move the folder with all files,  
downloaded files and extra files



### pretty github raw urls

```
pretty: https://github.com/mikezackles/skipstep/raw/master/skipstep.py
ugly  : https://raw.githubusercontent.com/mikezackles/skipstep/master/skipstep.py
```

&rarr; in the browser UI, dont show the redirect to raw.githubusercontent.com



### censorship-resistant p2p social network

https://github.com/milahu/p2p-killerapp

https://github.com/nostr-protocol/nostr/issues/102

keywords

- transparent moderation



### align different cuts of the same video

useful to extract the audiotrack from one release,
and use it for another release

uploaders can trim releases: remove black frames and silent audio.
this is stupid, because it creates extra work for other people,
but realistically, i will not stop these idiots from trimming their videos,
so i need a way to reconstruct the "original" cut of the video,
where the "original" cut is defined by a second "reference" video

usually, these video pauses (black frames and silent audio) are

- before the video
- after the video
- somewhere in the middle of the video for commercial breaks in tv show episodes

usually, these video pauses last between 2 and 10 seconds

some drafts in https://github.com/milahu/ffmpeg-align-video-scenes



### runlater

[schedule a command to run later](https://askubuntu.com/questions/339298/conveniently-schedule-a-command-to-run-later)

software for dynamic task scheduling:
when the user is active (screen is unlocked, keyboard and mouse are active), then dont run any jobs.
when the user is away from keyboard (screen is locked) and the cpu load is low, then run one job at a time.
this is useful for cpu-heavy jobs like ffmpeg. when a job is running and the user returns, pause the job.

draft in https://github.com/milahu/runlater



### argostranslate: download language models from bittorrent

lets add bittorrent as a "packages source " for [argospm](https://github.com/argosopentech/argos-translate/raw/master/argostranslate/argospm.py)

[argostranslate/package.py](https://github.com/argosopentech/argos-translate/raw/master/argostranslate/package.py)

```py
class AvailablePackage(IPackage):
    # ...
    def download(self) -> Path:
        # ...
        if not filepath.exists():
            data = networking.get_from(self.links)
```

```py
class IPackage:
    # ...
    def load_metadata_from_json(self, metadata):
        # ...
        self.links = metadata.get("links", list())
```



### fix linux network errors: device or resource busy

probably this error is caused by my bittorrent client (qbittorrent), saturating my uplink with 4MByte/sec



### media dirs path in user env

something like

```
MEDIA_DATA_DIRS="/run/media/user/achttera1/torrent:/run/media/user/fourtera1/torrent:/run/media/user/fourtera3/torrent"
```

```
$ readlink -f /run/media/user/*/torrent/ | tr $'\n' : | sed 's/:$//'
/run/media/user/achttera1/torrent:/run/media/user/fourtera1/torrent:/run/media/user/fourtera3/torrent
```

so i can find media files on this system, for media libraries of media players:
mixxx,
strawberry,
mpd,
quodlibet,
foobar2000,
...

so i can single-source this value from many apps,
which need access to media files on this machine



### move files across bind mounts

https://github.com/milahu/move-files-across-bind-mounts

https://serverfault.com/questions/327447/mount-bind-and-moving-2-files-between-mounts

https://unix.stackexchange.com/questions/406351/mount-bind-in-the-same-filesystem-move-files-as-in-the-same-filesystem

https://unix.stackexchange.com/questions/380025/why-cant-i-create-a-hardlink-to-a-file-from-a-mount-bind-directory-on-the/380033#380033



### curl: use fallocate

avoid file fragmentation

missing feature in curl

- https://curl.se/mail/archive-2014-02/0003.html - curl and fallocate
- https://www.man7.org/linux/man-pages/man2/fallocate.2.html - fallocate(2) — Linux manual page



## similar projects

- https://github.com/open-source-ideas/ideas
- https://github.com/otacke/h5p-todo
- https://github.com/grant/todo
- https://github.com/BrunnerLivio/todo
