# tar1090-zh 中文版


![Screenshot1](https://raw.githubusercontent.com/wiedehopf/tar1090/screenshots/screenshot3.png)

Provides an improved webinterface for use with ADS-B decoders readsb / dump1090-fa

提供改进的 Web 界面，可与 ADS-B 解码器 readsb / dump1090-fa 配合使用

## Installation 安装中文版

```
sudo bash -c "$(wget -nv -O - https://gh-proxy.com/https://github.com/magihub/tar1090-zh/raw/master/install.sh)"
```

## View the added webinterface  查看 Web 界面

Click the following URL and replace the IP address with address of your Raspberry Pi:

单击以下 URL，将 IP 地址替换为 Raspberry Pi 的地址：

http://192.168.x.yy/tar1090

If you are curious about your coverage, try this URL:

如果你对你的覆盖范围感到好奇， 试试这个 

http://192.168.x.yy/tar1090/?pTracks



## Update (same command as installation) 更新中文版

```
sudo bash -c "$(wget -nv -O - https://gh-proxy.com/https://github.com/magihub/tar1090-zh/raw/master/install.sh)"
```

Configuration should be preserved.


## Configuration part 1: History interval and number of snapshots / ptracks duration (optional)  配置 1：历史记录间隔和快照数/ptracks 持续时间（可选）

Edit the configuration file to change the interval in seconds and number of history files saved:

编辑配置文件以更改时间间隔（以秒为单位）和保存的历史记录文件数：

```
sudo nano /etc/default/tar1090
```
Ctrl-x to exit, y (yes) and enter to save.

Apply the configuration:
```
sudo systemctl restart tar1090
```

The duration of the history in seconds can be calculated as interval times history_size.

## Configuring part 2: the web interface (optional):  配置 2 ：Web 界面（可选）：

Remove the // at the start of a line, otherwise the setting will not be used.

删除行首的 // ，否则不会使用该设置。

```
sudo nano /usr/local/share/tar1090/html/config.js
```

Ctrl-x to exit, y (yes) and enter to save.
Then Ctrl-F5 to refresh the web interface in the browser.

If you somehow broke the interface or want the default config back:

如果您以某种方式破坏了接口或想要恢复默认配置：

```
sudo rm /usr/local/share/tar1090/html/config.js
```
Then run the install script again.

然后再次运行安装脚本。


## Enable (/disable) FA links in the webinterface (打开查询功能，并改用飞常准)

```
# ENABLE:  启用链接
sudo sed -i -e 's?.*flightawareLinks.*?flightawareLinks = true;?' /usr/local/share/tar1090/html/config.js
# ENABLE if the above doesn't work (updated from previous version)
echo 'flightawareLinks = true;' | sudo tee -a /usr/local/share/tar1090/html/config.js
# DISABLE:  禁用链接
sudo sed -i -e 's?.*flightawareLinks.*?flightawareLinks = false;?' /usr/local/share/tar1090/html/config.js
```

Then F5 to refresh the web interface in the browser.

If your instance is not at /tar1090 you'll need to edit the config.js in the approppriate html folder, see "Multiple instances".

## Enable Share links to ADSB-X
```
# ENABLE:
sudo sed -i -e 's?.*shareBaseUrl.*?shareBaseUrl  = "https://globe.adsbexchange.com/";?' /usr/local/share/tar1090/html/config.js
# ENABLE if the above doesn't work (updated from previous version)
echo 'shareBaseUrl  = "https://globe.adsbexchange.com/";' | sudo tee -a /usr/local/share/tar1090/html/config.js
# DISABLE:
sudo sed -i -e 's?.*shareBaseUrl.*?shareBaseUrl = false;?' /usr/local/share/tar1090/html/config.js
```

If your instance is not at /tar1090 you'll need to edit the config.js in the approppriate html folder, see "Multiple instances".

## UAT receiver running dump978-fa and skyaware978:

See the instructions for "Configuration part 1".
This is the relevant part in the configuration file:
```
# Change to yes to enable UAT/978 display in tar1090
ENABLE_978=no
# If running dump978-fa on another computer, modify the IP-address as appropriate.
URL_978="http://127.0.0.1/skyaware978"
```
Open and save as described above in the Configuration section.
Follow the instructions in the file.

### UAT only configuration

tar1090 running on the same pi as the skyaware978/dump978-fa:

```
echo /run/skyaware978 tar1090 | sudo tee /etc/default/tar1090_instances
```

After that run the install script and it should work.
978 should be disabled in the config file for this configuration.
UAT traffic will be displayed as ADS-B, this can't be avoided.

### Installation / Update to work with another folder, for example /run/combine1090  与其他文件夹共用


```
wget -nv -O /tmp/install.sh https://gh-proxy.com/https://github.com/magihub/tar1090-zh/raw/master/install.sh
sudo bash /tmp/install.sh /run/combine1090
```

## Remove / Uninstall   卸载中文版

```
sudo bash -c "$(wget -nv -O - https://gh-proxy.com/https://github.com/magihub/tar1090-zh/raw/master/uninstall.sh)"
```

## Using the filters   使用过滤器

js regex format, some basics (much more extensive issue available online on javascript regex syntax):
JS 正则表达式格式，一些基础知识

- a single `.` is the wildcard for exactly one character
- multiple patterns can be combined with or: `|`

### Some examples of useful constructs:

#### Filter by type code:    按类型代码筛选

```
B737 family: B73. (B73 and any character in the fourth position)
A320 family: A32.
B737-900 and B737 Max 9: B739|B39M
737 family including max: B73.|B3.M
B737 / A320 families: B73.|B3.M|A32.|A2.N
Only A320 and B737 models: A32|B73
Exclude a certain type: ^(?!A320)
Exclude multiple patterns: ^(?!(A32.|B73.))
```

#### Filter by type description:   按类型描述筛选

```
Helicopters: H..
Landplanes with 2 jet engines: L2J
Landplanes with any number of piston engines: L.P
Helicopters any number of turbin engines : H.T
All turboprop powered things, including turbine helicopters: ..T
Everything with 4 engines: .4.
Everything with 2 3 and 4 engines: .2.|.3.|.4.
``` 

## Keyboard Shortcuts  键盘快捷键

- Q and E zoom out and in. 
- A and D move West and East.
- W and S move North and South.
- C or Esc clears the selection.
- M toggles multiselect.
- T selects all aircraft
- B toggle map brightness

## URL query parameters 查询参数 (/tar1090/?icao=123456&zoom=5 and similar)

See [README-query.md](README-query.md)

## Multiple instances  多个实例

The script can install multiple instances, this is accomplished by first editing `/etc/default/tar1090_instances`:
该脚本可以安装多个实例，首先需编辑

On each line there must be one instance.
First on the line the source directory where the aircraft.json is located.
Second on the line the name where you want to access the according website.
(http://pi/tar1090 or http://pi/combo or http://pi/978 in this example)

If you want the instance at http://pi/, use webroot as a name.

The main instance needs to be included in this file.

Example file:
```
/run/dump1090-fa tar1090
/run/combine1090 combo
/run/skyaware978 978
/run/dump1090-fa webroot
```

After saving that file, just run the install script and it will install/update
all instances.

Configuration for each instance will be separate, in the example the config files would be:
```
/etc/default/tar1090
/etc/default/tar1090-combo
/etc/default/tar1090-978
/etc/default/tar1090-webroot
```

The config.js will also have another path, to edit each config:
```
sudo nano /usr/local/share/tar1090/html/config.js
sudo nano /usr/local/share/tar1090/html-combo/config.js
sudo nano /usr/local/share/tar1090/html-978/config.js
sudo nano /usr/local/share/tar1090/html-webroot/config.js
```

HTML folders will be:
```
/usr/local/share/tar1090/html
/usr/local/share/tar1090/html-combo
/usr/local/share/tar1090/html-978
/usr/local/share/tar1090/html-webroot
```

The run folder and systemd service will be called tar1090-combo and tar1090-978
in this example file.
The main instance is the exception to that rule, having systemd service and run
directory called just tar1090.

### Removing an instance

For example removing the instance with the name combo and 978:

First remove the corresponding line from `/etc/default/tar1090_instances` and
save the file so when you update it doesn't get installed again.

Then run the following command adapted to your instance name, you'll need to
include the tar1090- which is automatically added for the service names:

```
sudo bash /usr/local/share/tar1090/uninstall.sh tar1090-combo
sudo bash /usr/local/share/tar1090/uninstall.sh tar1090-978
```

If the instance was installed with the old method without the tar1090_instances
file, you'll have to try without the tar1090- before the combo, like this:

```
sudo bash /usr/local/share/tar1090/uninstall.sh combo
sudo bash /usr/local/share/tar1090/uninstall.sh 978
```

## lighttpd

tar1090 is now available at :8504 by default when using lighttpd. (port 8504)

To display tar1090 at /, add an instance as described above that has the name webroot.
It will be available at /


## nginx configuration  NGINX 配置

If nginx is installed, the install script should give you a configuration file
you can include.  The configuration needs to go into the appropriate server { }
section.
In the usual configuration that means to add this line:
```
include /usr/local/share/tar1090/nginx-tar1090.conf;
```
in the server { } section of either `/etc/nginx/sites-enabled/default` or `/etc/nginx/conf.d/default.conf` depending on your system configuration.
Don't forget to restart the nginx service.


## /tar1090/?pTracks  轨迹查询

![Screenshot2](https://raw.githubusercontent.com/wiedehopf/tar1090/screenshots/screenshot4.png)

- Add /?pTracks to the usual /tar1090 URL, should look like this: http://192.168.x.yy/tar1090/?pTracks
- Shows the last 8 hours of traces you have seen, gives a nice visual representation of your coverage / range
- Can be filtered by altitude with the altitude filter
- Configure a longer duration than 8 hours via the [configuration](#configuration-part-1-history-interval-and-number-of-snapshots--ptracks-duration-optional)
- Restrict the duration shown to 2 hours: /tar1090/?pTracks=2
- Draw less points which reduces display time (higher interval, lower compute time, default 15): /tar1090/?pTracks=8&pTracksInterval=60


## A separate instance with longer data retention for gauging range  一个单独的实例，具有更长的数据保留时间，用于测量范围

If this seems too complicated for you or you don't want a 2nd instance, changing / adding PTRACKS=24 to the /etc/default/tar1090 configuration should also extend the history (for /?pTracks only).
如果这对您来说似乎太复杂了，或者您不想要第二个实例，那么在 /etc/default/tar1090 配置中更改/添加 PTRACKS=24 也应该扩展历史记录（仅适用于 /？pTracks）。

```
sudo nano /etc/default/tar1090_instances
```

put in these two lines if you're using readsb
```
/run/readsb tar1090
/run/readsb persist
```

put in these two lines if you're using dump1090-fa
```
/run/dump1090-fa tar1090
/run/dump1090-fa persist
```

if you then run the tar1090 install script afterwards you'll have an extra instance you can configure the history retention for.   中文版
```
sudo bash -c "$(wget -nv -O - https://gh-proxy.com/https://github.com/magihub/tar1090-zh/raw/master/install.sh)"
sudo nano /etc/default/tar1090-persist
```

change to these values for 24h of history:  改为24h历史记录

```
# Interval at which the track history is saved
INTERVAL=20
# How many points in time are stored in the track history
HISTORY_SIZE=4300
```

then
```
sudo systemctl restart tar1090-persist
```
and the persist instance will start saving more data.
You can then visit `/persist/?pTracks` instead of `/tar1090` to get the complete 24h history displayed.
Press T to toggle the traces on and off, this is recommended for zooming and panning as with the traces showing this can be slow.

(you can also look at /tar1090/?pTracks if you want to look only at the more recent tracks, interval / history can be configured in /etc/tar1090 for that instance)

For adding the range outline to the /persist instance after having used the method described earlier, copy over the json:

```
sudo cp /usr/local/share/tar1090/html/upintheair.json /usr/local/share/tar1090/html-persist
```


## readsb wiedehopf fork --heatmap feature:  热图功能

/var/globe_history needs to be a directory writeable by the user readsb.
`sudo mkdir /var/globe_history` and `sudo chown readsb /var/globe_history` are useful for that.

Add readsb options:
```
--heatmap-dir /var/globe_history --heatmap 30
```


## heatmap in conjunction with readsb wiedehopf fork --heatmap feature:

```
/tar1090/?heatmap=200000
```
Maximum number of dots to draw is the number after heatmap.
Optional arguments that can be added to the URL:
- duration in hours that shall be displayed: &heatDuration=48 (default: 24)
- set the end of the duration 48 hours into the past: &heatEnd=48 (default:0)
- radius of the dots: &heatRadius=2
- opacity of the dots: &heatAlpha=2
- only redraw the dots when pressing R on the keyboard: &heatManualRedraw

alternative display style: &realHeat
- blurryness: &heatBlur=2
- weight of each dot for the heatmap: &heatWeight=4


## offline map   离线地图

<https://github.com/adsbxchange/wiki/wiki/tar1090-offline-map>


## Uses this library for decompressing zstd

<https://github.com/wiedehopf/zstddec-tar1090>


## NO WARRANTY  免责声明

   BECAUSE THE PROGRAM IS LICENSED FREE OF CHARGE, THERE IS NO WARRANTY
FOR THE PROGRAM, TO THE EXTENT PERMITTED BY APPLICABLE LAW.  EXCEPT WHEN
OTHERWISE STATED IN WRITING THE COPYRIGHT HOLDERS AND/OR OTHER PARTIES
PROVIDE THE PROGRAM "AS IS" WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESSED
OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE.  THE ENTIRE RISK AS
TO THE QUALITY AND PERFORMANCE OF THE PROGRAM IS WITH YOU.  SHOULD THE
PROGRAM PROVE DEFECTIVE, YOU ASSUME THE COST OF ALL NECESSARY SERVICING,
REPAIR OR CORRECTION.

  由于该程序是免费许可的，因此在适用法律允许的范围内，该程序不提供任何保证。除非另有书面说明，否则版权所有者和/或其他方“按原样”提供程序，不作任何明示或暗示的保证，包括但不限于对适销性和特定用途适用性的暗示保证。有关程序质量和性能的全部风险由您承担。如果程序被证明有缺陷，您将承担所有必要的服务、维修或纠正费用。
