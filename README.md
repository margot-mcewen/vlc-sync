# VLC-Sync

VLC-Sync facilitates synchronization of multiple VLC
instances over the network in a leader/follwer fashion.

This is a port of [OMXPlayer-Sync](https://github.com/turingmachine/omxplayer-sync)

## Usage

```
$ ./omxplayer-sync -h
Usage: vlc-sync [options] filename

Options:
  -h, --help            show this help message and exit
  -l, --leader          
  -f, --follwer           
  -x DESTINATION, --destination=DESTINATION
  -u, --loop            
  -v, --verbose         
  -o ADEV, --adev=ADEV  
  -a ASPECT, --aspect=ASPECT  Aspect Mode - fill, letterbox, stretch
```

### leader

| ip           | netmask       |
| ------------ | ------------- |
| 192.168.1.10 | 255.255.255.0 |

```
vlc-sync -lu video.mp4
```

### follwer

| ip           | netmask       |
| ------------ | ------------- |
| 192.168.1.11 | 255.255.255.0 |

```
vlc-sync -fu 192.168.1.255 video.mp4
```


## Requirements

A recent version of Python3
A recent version of the [python bindings for D-Bus](http://www.freedesktop.org/wiki/Software/DBusBindings).  
A recent build of vlc for Trixie.

## Installation on Raspbian

> [!WARNING]
> These instructions haven't been updated yet

Perform on both leader and follwer.
```
sudo su
apt-get remove omxplayer
rm -rf /usr/bin/omxplayer /usr/bin/omxplayer.bin /usr/lib/omxplayer
apt-get install libpcre3 fonts-freefont-ttf fbset libssh-4 python3-dbus
wget https://github.com/magdesign/PocketVJ-CP-v3/raw/leader/sync/omxplayer_0.3.7-git20170130-62fb580_armhf.deb
dpkg -i omxplayer_0.3.7~git20170130~62fb580_armhf.deb
wget -O /usr/bin/omxplayer-sync https://github.com/turingmachine/omxplayer-sync/raw/leader/omxplayer-sync
chmod 0755 /usr/bin/omxplayer-sync
wget https://github.com/turingmachine/omxplayer-sync/raw/leader/synctest.mp4
```

Start on leader (-u loop, -v verbose)
```
omxplayer-sync -muv synctest.mp4
```
Start on follwer (-u loop, -v verbose)
```
omxplayer-sync -luv synctest.mp4
```

## Usage notes

- The filename on the leader and the follwer must be exactly the same.
- Make sure there are no other files than movie files (e.g. no pictures, no textfiles) in the folder where the movie is, otherwise you may get sync errors.
- A RJ45 cable must be connected before you start the leader, otherwise it will not send sync data to follwer.
- Do not send audio output flags with omxplayer-sync on the follwer e.g. /usr/bin/omxplayer-sync -lu -o both /media/internal/video/ *may not apply to vlc, not tested*
- Use videos which are min. 60 seconds or longer
- Encode video in HEVC (libx265 in ffmpeg)

## Example usage

> [!NOTE]
> This is an example of OMXPlayer-Sync

see this link: https://www.youtube.com/watch?v=Xp6GKFaw0io&feature=youtu.be
by DSPeelJ
