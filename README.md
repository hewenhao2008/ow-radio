# Network Radio on OpenWrt

## Steps

Tested on MediaTek LinkIt Smart 7688 board.

1. Installation

   Option 1 - build your own sysupgrade bin with `mpd-full`, `mpc`, `ffmpeg` and programm it to board. (**RECOMMEND as option 2 may fail because of STORAGE NOT ENOUGH**)

   Option 2 - install `mpd-full`, `mpc`, `ffmpeg` by `opkg` with the following steps:

   ```
   -> Add opkg source
   root@mylinkit:/# vi /etc/opkg.conf

   src/gz chaos_calmer https://downloads.openwrt.org/chaos_calmer/15.05/ramips/mt7628/packages/packages
   dest root /
   dest ram /tmp
   lists_dir ext /var/opkg-lists
   option overlay_root /overlay
   option check_signature 1

   -> Update opkg source
   root@mylinkit:/# opkg update

   -> Install package
   root@mylinkit:/# opkg install mpd-full mpc ffmpeg
   ```

2. Config `mpd`

   Edit /etc/mpd.conf as the following.

   ```
   music_directory "/home/music"
   playlist_directory "/home/.mpd/music_playlist"
   db_file "/home/.mpd/mpd.db"
   log_file "/home/.mpd/mpd.log"
   pid_file "/home/.mpd/mpd.pid"
   state_file "/home/.mpd/mpd.state"
   user "root"
   group "users"
   bind_to_address "0.0.0.0"
   port "6600"
   input {
       plugin "curl"
   }
   audio_output {
       type "alsa"
       name "My ALAS Device"
   }

   filesystem_charset "UTF-8"
   ```

   NOTE: you are required to create `/home/music` and `/home/.mpd` and `/home/.mpd/music_playlist` by yourself, using the following command:

   ```
   mkdir -p /home/music
   mkdir -p /home/.mpd/music_playlist
   ```

3. Start `mpd`

   ```
   /etc/init.d/mpd start
   ```

3. Add playlist

   ```
   mpc add mms://media.fm1036.com/liveaudio
   mpc add mms://live.fm993.com.cn/musicfm
   mpc add mms://live.hitfm.cn/fm887
   ```

   More network radio station resource:

   - http://blog.renren.com/share/333390391/14357648754
   - http://blog.csdn.net/aminfo/article/details/7646883
   - http://blog.csdn.net/aminfo/article/details/7646887

4. Play

   ```
   mpc play [1/2/3]
   ```
