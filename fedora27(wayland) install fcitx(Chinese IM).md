## Because fedora27 uses wayland as default enviroment

### Gnome On Wayland 用户无法使用 fcitx
#### 由于 wayland 无法读取 ~/.xprofile 中的环境变量，所以请在/etc/environment中加入：
```
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
```
#### 或在登录时选择 运行于 Xorg 的 Gnome 。
