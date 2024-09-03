### Downlod the CT in Proxmox
```
centos-9-stream-default_20221109_amd64.tar.xz
```

### Update the system
```
yum -y install epel-release
yum -y update
yum clean all
```

### Install minimal X Windows system and XFCE Desktop
```
yum -y groupinstall 'X Window System'
yum -y groupinstall xfce
```

### Run the following to start XFCE GUI at boot-time
```
systemctl set-default graphical.target
```

### Update the system and clean the cache
```
yum -y install epel-release
sudo yum -y update
sudo yum clean all
```

### Install XRDP
```
sudo yum -y install xrdp tigervnc-server
```

### Make necessary changes to SELinux (requires more understanding)
```
sudo chcon -h system_u:object_r:bin_t:s0 /usr/sbin/xrdp
sudo chcon -h system_u:object_r:bin_t:s0 /usr/sbin/xrdp-sesman
```

### If necessary - adjust firewall rules to allow port 3389 (RDP server runs on this port)
```
sudo systemctl enable firewalld
sudo systemctl start firewalld
sudo firewall-cmd --permanent --zone=public --add-port=3389/tcp
sudo firewall-cmd --reload
```

### Enable XRDP
`sudo systemctl enable xrdp`

### Set password for your default user on the machine
`passwd`

#### Create a new user
```
sudo adduser sukanta
passwd sukanta
```
#### If necessary - add new user to the sudoers list 
```
sudo usermod -aG wheel sukanta
```
By default, on CentOS, members of the `wheel` group have sudo privileges.

### Start RDP client and use the VM's public address to connect.

If XRDP session immediately closes after loggin in:
```
echo xfce4-session > $HOME/.xsession
chmod +x .xsession
```

### Install Microsoft Edge 
```
sudo dnf clean all
sudo dnf update

```
### Let’s add the Microsoft Edge repository
```
sudo dnf config-manager --add-repo https://packages.microsoft.com/yumrepos/edge
```
###Next, import the GPG key:
```
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
```
### install Microsoft Edge
```
sudo dnf update
sudo dnf install microsoft-edge-stable
```
### Verify the build and version of Edge:
```
microsoft-edge -version
```
### First, download Google Chrome
```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
```
###  install Chrome web browser
```
sudo yum localinstall google-chrome-stable_current_x86_64.rpm
```
###
```
sudo yum upgrade google-chrome-stable
```
### xrdp.ini
```
[Globals]
; xrdp.ini file version number
ini_version=1

; fork a new process for each incoming connection
fork=true

; ports to listen on, number alone means listen on all interfaces
; 0.0.0.0 or :: if ipv6 is configured
; space between multiple occurrences
; ALL specified interfaces must be UP when xrdp starts, otherwise xrdp will fail to start
;
; Examples:
;   port=3389
;   port=unix://./tmp/xrdp.socket
;   port=tcp://.:3389                           127.0.0.1:3389
;   port=tcp://:3389                            *:3389
;   port=tcp://<any ipv4 format addr>:3389      192.168.1.1:3389
;   port=tcp6://.:3389                          ::1:3389
;   port=tcp6://:3389                           *:3389
;   port=tcp6://{<any ipv6 format addr>}:3389   {FC00:0:0:0:0:0:0:1}:3389
;   port=vsock://<cid>:<port>
port=3389

; 'port' above should be connected to with vsock instead of tcp
; use this only with number alone in port above
; prefer use vsock://<cid>:<port> above
use_vsock=false

; regulate if the listening socket use socket option tcp_nodelay
; no buffering will be performed in the TCP stack
tcp_nodelay=true

; regulate if the listening socket use socket option keepalive
; if the network connection disappear without close messages the connection will be closed
tcp_keepalive=true

; set tcp send/recv buffer (for experts)
tcp_send_buffer_bytes=4194304
tcp_recv_buffer_bytes=4194304

; security layer can be 'tls', 'rdp' or 'negotiate'
; for client compatible layer
security_layer=negotiate

; minimum security level allowed for client for classic RDP encryption
; use tls_ciphers to configure TLS encryption
; can be 'none', 'low', 'medium', 'high', 'fips'
crypt_level=none

; X.509 certificate and private key
; openssl req -x509 -newkey rsa:2048 -nodes -keyout key.pem -out cert.pem -days 365
; note this needs the user xrdp to be a member of the ssl-cert group, do with e.g.
;$ sudo adduser xrdp ssl-cert
certificate=
key_file=

; set SSL protocols
; can be comma separated list of 'SSLv3', 'TLSv1', 'TLSv1.1', 'TLSv1.2', 'TLSv1.3'
ssl_protocols=TLSv1.2, TLSv1.3
; set TLS cipher suites
#tls_ciphers=HIGH

; concats the domain name to the user if set for authentication with the separator
; for example when the server is multi homed with SSSd
#domain_user_separator=@

; The following options will override the keyboard layout settings.
; These options are for DEBUG and are not recommended for regular use.
#xrdp.override_keyboard_type=0x04
#xrdp.override_keyboard_subtype=0x01
#xrdp.override_keylayout=0x00000409

; Section name to use for automatic login if the client sends username
; and password. If empty, the domain name sent by the client is used.
; If empty and no domain name is given, the first suitable section in
; this file will be used.
autorun=

allow_channels=true
allow_multimon=true
bitmap_cache=true
bitmap_compression=true
bulk_compression=true
#hidelogwindow=true
max_bpp=16
new_cursors=true
; fastpath - can be 'input', 'output', 'both', 'none'
use_fastpath=both
; when true, userid/password *must* be passed on cmd line
#require_credentials=true
; when true, the userid will be used to try to authenticate
#enable_token_login=true
; You can set the PAM error text in a gateway setup (MAX 256 chars)
#pamerrortxt=change your password according to policy at http://url

;
; colors used by windows in RGB format
;
blue=009cb5
grey=dedede
#black=000000
#dark_grey=808080
#blue=08246b
#dark_blue=08246b
#white=ffffff
#red=ff0000
#green=00ff00
#background=626c72

;
; configure login screen
;

; Login Screen Window Title
#ls_title=My Login Title

; top level window background color in RGB format
ls_top_window_bg_color=009cb5

; width and height of login screen
;
; The default height allows for about 5 fields to be comfortably displayed
; above the buttons at the bottom. To display more fields, make <ls_height>
; larger, and also increase <ls_btn_ok_y_pos> and <ls_btn_cancel_y_pos>
; below
;
ls_width=350
ls_height=430

; login screen background color in RGB format
ls_bg_color=dedede

; optional background image filename (bmp format).
#ls_background_image=

; logo
; full path to bmp-file or file in shared folder
ls_logo_filename=
ls_logo_x_pos=55
ls_logo_y_pos=50

; for positioning labels such as username, password etc
ls_label_x_pos=30
ls_label_width=65

; for positioning text and combo boxes next to above labels
ls_input_x_pos=110
ls_input_width=210

; y pos for first label and combo box
ls_input_y_pos=220

; OK button
ls_btn_ok_x_pos=142
ls_btn_ok_y_pos=370
ls_btn_ok_width=85
ls_btn_ok_height=30

; Cancel button
ls_btn_cancel_x_pos=237
ls_btn_cancel_y_pos=370
ls_btn_cancel_width=85
ls_btn_cancel_height=30

[Logging]
; Note: Log levels can be any of: core, error, warning, info, debug, or trace
LogFile=xrdp.log
LogLevel=INFO
EnableSyslog=true
#SyslogLevel=INFO
#EnableConsole=false
#ConsoleLevel=INFO
#EnableProcessId=false

[LoggingPerLogger]
; Note: per logger configuration is only used if xrdp is built with
; --enable-devel-logging
#xrdp.c=INFO
#main()=INFO

[Channels]
; Channel names not listed here will be blocked by XRDP.
; You can block any channel by setting its value to false.
; IMPORTANT! All channels are not supported in all use
; cases even if you set all values to true.
; You can override these settings on each session type
; These settings are only used if allow_channels=true
rdpdr=false
rdpsnd=true
drdynvc=true
cliprdr=false
rail=true
xrdpvr=true
tcutils=true

; for debugging xrdp, in section xrdp1, change port=-1 to this:
#port=/tmp/.xrdp/xrdp_display_10


;
; Session types
;

; Some session types such as Xorg, X11rdp and Xvnc start a display server.
; Startup command-line parameters for the display server are configured
; in sesman.ini. See and configure also sesman.ini.
[Xorg]
name=Xorg
lib=libxup.so
username=ask
password=ask
ip=127.0.0.1
port=-1
code=20

[Xvnc]
name=Xvnc
lib=libvnc.so
username=ask
password=ask
ip=127.0.0.1
port=-1
xserverbpp=16
#delay_ms=2000
; Disable requested encodings to support buggy VNC servers
; (1 = ExtendedDesktopSize)
#disabled_encodings_mask=0
; Use this to connect to a chansrv instance created outside of sesman
; (e.g. as part of an x11vnc console session). Replace '0' with the
; display number of the session
#chansrvport=DISPLAY(0)

; Generic VNC Proxy
; Tailor this to specific hosts and VNC instances by specifying an ip
; and port and setting a suitable name.
[vnc-any]
name=vnc-any
lib=libvnc.so
ip=ask
port=ask5900
username=na
password=ask
#pamusername=asksame
#pampassword=asksame
#pamsessionmng=127.0.0.1
#delay_ms=2000

; Generic RDP proxy using NeutrinoRDP
; Tailor this to specific hosts by specifying an ip and port and setting
; a suitable name.
[neutrinordp-any]
name=neutrinordp-any
; To use this section, you should build xrdp with configure option
; --enable-neutrinordp.
lib=libxrdpneutrinordp.so
ip=ask
port=ask3389
username=ask
password=ask
; Uncomment the following lines to enable PAM authentication for proxy
; connections.
#pamusername=ask
#pampassword=ask
#pamsessionmng=127.0.0.1
; Currently NeutrinoRDP doesn't support dynamic resizing. Uncomment
; this line if you're using a client which does.
#enable_dynamic_resizing=false
; By default, performance settings requested by the RDP client are ignored
; and chosen by NeutrinoRDP. Uncomment this line to allow the user to
; select performance settings in the RDP client.
#perf.allow_client_experiencesettings=true
; Override any experience setting by uncommenting one or more of the
; following lines.
#perf.wallpaper=false
#perf.font_smoothing=false
#perf.desktop_composition=false
#perf.full_window_drag=false
#perf.menu_anims=false
#perf.themes=false
#perf.cursor_blink=false
; By default NeutrinoRDP supports cursor shadows. If this is giving
; you problems (e.g. cursor is a black rectangle) try disabling cursor
; shadows by uncommenting the following line.
#perf.cursor_shadow=false
; By default, NeutrinoRDP uses the keyboard layout of the remote RDP Server.
; If you want to tell the remote the keyboard layout of the RDP Client,
; by uncommenting the following line.
#neutrinordp.allow_client_keyboardLayout=true
; The following options will override the remote keyboard layout settings.
; These options are for DEBUG and are not recommended for regular use.
#neutrinordp.override_keyboardLayout_mask=0x0000FFFF
#neutrinordp.override_kbd_type=0x04
#neutrinordp.override_kbd_subtype=0x01
#neutrinordp.override_kbd_fn_keys=12
#neutrinordp.override_kbd_layout=0x00000409

; You can override the common channel settings for each session type
#channel.rdpdr=true
#channel.rdpsnd=true
#channel.drdynvc=true
#channel.cliprdr=true
#channel.rail=true
#channel.xrdpvr=true
```


