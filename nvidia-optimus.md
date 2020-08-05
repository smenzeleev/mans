# Решение проблемы с работой дискретной карты на ноутбуке с поддержкой optimus (работает для xiaomi nootebook pro mx250. Ubuntu 20)
1. Смотрим `/var/log/Xorg.0.log`
2. Грепаем по ошибкам
3. Если есть нечто вроде:
> (EE) NVIDIA(G0): GPU screens are not yet supported by the NVIDIA driver
>
> (EE) NVIDIA(G0): Failing initialization of X screen
И при этом nvidia-smi выдает *No running processes found*
4. Можно попробовать переустановать `nvidia-utils`
5. Скопировать конфиги nvidia из `/usr/share/X11/xorg.conf.d` в `/etc/X11/xorg.conf.d/`
и добавить в секцию `Option "PrimaryGPU" "yes"`
Таким образом получится нечто типа:
```
Section "OutputClass"
    Identifier "nvidia"
    MatchDriver "nvidia-drm"
    Driver "nvidia"
    Option "AllowEmptyInitialConfiguration"
    Option "PrimaryGPU" "yes"
    ModulePath "/usr/lib/x86_64-linux-gnu/nvidia/xorg"
EndSection
```
6. Можно проверить еще через `glxinfo | grep "OpenGL renderer"`

P.S Данный подход работает *без modeset* проверить можно через `grep nvidia /etc/modprobe.d/* /lib/modprobe.d/*`

P.P.S Выключить modeset можно через `options nvidia-drm modeset=1 -> 0`, `sudo update-initramfs -u` и reboot

P.P.P.S Еще есть ман [Xserver'a](http://download.nvidia.com/XFree86/Linux-x86_64/440.82/README/primerenderoffload.html)

Доп.ссылки:

https://forums.developer.nvidia.com/t/ubuntu-18-04-3-blank-screen-at-startup-with-430-drivers-and-gtx-960/107501

https://bbs.archlinux.org/viewtopic.php?id=244351

https://forums.developer.nvidia.com/t/linux-nvidia-gpu-screens-are-not-yet-supported/120834

https://wiki.archlinux.org/index.php/NVIDIA/Troubleshooting#X_fails_with_%22Failing_initialization_of_X_screen%22