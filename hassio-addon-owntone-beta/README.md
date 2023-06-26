# Home Assistant Owntone AddOn (known as forked-daapd)

Based on [docker-daapd](https://github.com/linuxserver/docker-daapd), with spotify, chromecast and airplay and enable the possibility to run the Owntone Server - forked daapd - on your Raspberry Pi. The configuration is implemented and will give you the possibility to adjust the Owntone server in Home Assistant by your need. 

## Supported Architectures

The addon support multiple architectures such as  armv7, aarch64, amd64. *Only tested on aarch64 Raspberry Pi 4b yet*

## Installation
In Homeassistent go to `Supervisor` -> `AddOn Store`, click on the three vertical dots in the top right corner and on `Repositories`. Add the URL of this GitHub page into the Input field and click okay. After some seconds, there should be a `Owntone server` AddOn in your list. After a click on this tile, it's possible to install as any other Home Assistant AddOn. 

This AddOn will operate in the `/share/owntone` folder that contains a folder named `dbase_and_logs` that is used for configurations and logs. The folder named `music` is the place, where the iTunes libary or music at needs to be stored.

## Configuration
| Configuration Key | Default Value | Description |
| ------ | ------ | ------ |
| general.admin_password | `''` | Admin password for the web interface. Note that access to the web interface from computers in "trusted_network" (see below) does not require password
| general.loglevel | `log` | Log level. Available levels: `fatal`, `log`, `warning`, `info`, `debug`, `spam`
| general.trusted_networks | `{ "localhost", "192.168", "172.17", "172.30" }` | Sets who is allowed to connect without authorisation. This applies to client types like Remotes, DAAP clients (iTunes) and to the web interface. Options are "any", "localhost" or the prefix to one or more ipv4/6 networks. |
| library.name | `Hassio music` | Name of the library as displayed by the clients (%h: hostname). If you change the name after pairing with Remote you may have to re-pair. |
| library.password | `''` | Password for the library. |
| library.m3u_overrides | `false` | Should metadata from m3u playlists, e.g. artist and title in EXTINF, override the metadata we get from radio streams? |
| library.itunes_overrides | `false` | Should iTunes metadata override ours? |
| library.itunes_smartpl | `false` | Should we import the content of iTunes smart playlists? |
| airplay | `[]` | AirPlay per device settings (make sure you get the capitalization of the device name right) |
| airplay[].name || make sure you get the capitalization of the device name right |
| airplay[].max_volume || OwnTone's volume goes to 11! If that's more than you can handle you can set a lower value here |
| airplay[].permanent || Enable this option to keep a particular AirPlay device in the speaker list and thus ignore mdns notifications about it no longer being present. The speaker will remain until restart of OwnTone. |
| airplay[].exclude || Enable this option to exclude a particular AirPlay device from the speaker list |
| airplay[].password || AirPlay password |
| airplay[].reconnect | `false` | Some devices spuriously disconnect during playback, and based on the device type OwnTone may attempt to reconnect. Setting this option  overrides this so reconnecting is either always enabled or disabled. |
| airplay[].raop_disable || Disable AirPlay 1 (RAOP) |
| airplay[].nickname || Name used in the speaker list, overrides name from the device |
| chromecast | `[]` | Chromecast per device settings  (make sure you get the capitalization of the device name right) |
| chromecast[].name || make sure you get the capitalization of the device name right
| chromecast[].max_volume || OwnTone's volume goes to 11! If that's more than you can handle you can set a lower value here
| chromecast[].exclude || Enable this option to exclude a particular device from the speaker list
| chromecast[].nickname || Name used in the speaker list, overrides name from the device
| spotify.use_libspotify | `false` | Spotify settings |
| spotify.settings_dir' | `/share/owntone/libspotify` | The server can stream from Spotify using either its own implementation or using Spotify's libspotify (which was deprecated many years ago) - take care that this folder is created before starting the server |
| spotify.cache_dir | `/tmp` | Cache directory (only has effect with libspotify) |
| spotify.bitrate |  `0` | Set preferred bitrate for music streaming (0: No preference (default), 1: 96kbps, 2: 160kbps, 3: 320kbps) |
| spotify.base_playlist_disable | `false` | Your Spotify playlists will by default be put in a "Spotify" playlist folder. If you would rather have them together with your other playlists you can set this option to true. |
| spotify.artist_override | `false` | Spotify playlists usually have many artist, and if you don't want every artist to be listed when artist browsing in Remote, you can set the artist_override flag to true. This will use the compilation_artist as album artist for Spotify items. |
| spotify.album_override | `false` | Similar to the different artists in Spotify playlists, the playlist items belong to different albums, and if you do not want every album to be listed when browsing in Remote, you can set the album_override flag to true. This will use the playlist name as album name for Spotify items. Notice that if an item is in more than one playlist, it will only appear in one album when browsing (in which album is random). |

### Examples
#### HomePod
Disable Airplay 1 for an HomePod. The name needs to be the exact name of your Homepod.
```
airplay: 
  - name: "HomePod"
    raop_disable: true
```
## Considerations

### AirPlay 2
This Owntone server supports Airplay 2 but only if your devices are not requesting any password. To enable your HomePods to work with Home Assistent together it's required to open the Home App on the iPhone / iPad and click on the small home icon in the top left corner -> `Home Settings` -> `Allow access` and set it to `All in the same network`.

### Stability 
This AddOn runs in docker with the default network `bridge` mode. Whenever we tried it to set to `host`, the docker container crashed after some minutes and the whole Raspberry PI needs to be restarted. It's tested with AirPlay devices and OwnTone was able to discover them even without a working MultiCast network possibility. 

## Credits
Insipred by https://github.com/Ulrar/hassio-addons/tree/master/forked-daapd

## License

MIT

**Free Software, Hell Yeah!**

