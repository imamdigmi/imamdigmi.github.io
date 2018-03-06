---
date: 2018-02-12T04:48:09Z
lastmod: 2018-02-12T04:48:09Z

title: "Setup i3wm di Arch Linux"
description: "Memasang i3 (window manager) di Arch Linux"
author: "Imam Digmi"

weight: 50
draft: true
keywords: [i3, arch, linux, window manager]
tags: [archlinux]
categories: [Arch Linux]
---

```
sudo pacman -S i3
```

```
sudo pacman -S compton
```

`configuration`
```
#=========== STARTUP APP
exec --no-startup-id nm-applet
exec amixer sset Master 20%
exec --no-startup-id compton


#=========== APPEREANCE 
set_from_resource $fg i3wm.color7 #f0f0f0
set_from_resource $bg i3wm.color2 #f0f0f0
# CLASS                 	BORDER	BACKGR	TEXT	INDOCATOR	CHILD_BORDER
client.focused          	$bg     $bg     $fg		$bg			$bg
client.focused_inactive 	$bg     $bg     $fg		$bg			$bg
client.unfocused        	$bg     $bg     $fg		$bg			$bg
client.urgent           	$bg     $bg     $fg		$bg			$bg
client.placeholder      	$bg     $bg     $fg		$bg			$bg
client.background       	$bg
font pango:xos4 Terminus 13


#=========== APPLICATION
bindsym Mod1+Return exec i3-sensible-terminal
bindsym Mod1+Shift+q kill
bindsym Mod1+Shift+w exec --no-startup-id wal -i "$HOME/Pictures/Wallpapers/" -g

bindsym Mod1+d exec rofi -show run -font "xos4 Terminus 12"
bindsym Mod1+t exec thunar
bindsym Mod1+b exec google-chrome-stable
bindsym Mod1+m exec mousepad
bindsym Mod1+c exec code


#=========== CUSTOM BINDING
bindsym Print exec scrot '%Y-%m-%d-%T_$wx$h_scrot.png' -e 'mv $f ~/Pictures/Screenshots'
bindsym Print+Control exec scrot -u -b '%Y-%m-%d-%T_$wx$h_scrot.png' -e 'mv $f ~/Pictures/Screenshots'

bindsym XF86AudioRaiseVolume exec amixer -D pulse sset Master 5%+ && pkill -RTMIN+1 i3blocks
bindsym XF86AudioLowerVolume exec amixer -D pulse sset Master 5%- && pkill -RTMIN+1 i3blocks
bindsym Mod1+XF86AudioRaiseVolume exec amixer -D pulse sset Master 1%+ && pkill -RTMIN+1 i3blocks
bindsym Mod1+XF86AudioLowerVolume exec amixer -D pulse sset Master 1%- && pkill -RTMIN+1 i3blocks
bindsym XF86AudioMute exec amixer sset Master toggle && killall -USR1 i3blocks

bindsym XF86AudioPlay exec playerctl play-pause  
bindsym XF86AudioNext exec playerctl next  
bindsym XF86AudioPrev exec playerctl previous

bindsym XF86MonBrightnessUp exec brightnessctl -c backlight s 5%+
bindsym XF86MonBrightnessDown exec brightnessctl -c backlight s 5%-
bindsym Mod1+XF86MonBrightnessUp exec brightnessctl -c backlight s 1%+
bindsym Mod1+XF86MonBrightnessDown exec brightnessctl -c backlight s 1%-

bindsym Mod1+Control+l exec sh ~/.config/i3/i3lock.sh
bindsym Mod1+Control+s exec sh ~/.config/i3/i3lock.sh && systemctl suspend
bindsym --release Caps_Lock exec pkill -SIGRTMIN+11 i3blocks
#bindsym Mod1+d exec dmenu_run -nb "$fg" -nf "$bg" -sb "$bg" -sf "$fg"  -p  "Run: " -fn "pango:xos4 Terminus 13"


#=========== ENVIROUNMENT
set $up l
set $down k
set $left j
set $right semicolon

floating_modifier Mod1
bindsym Mod1+$left focus left								
bindsym Mod1+$down focus down							
bindsym Mod1+$up focus up								
bindsym Mod1+$right focus right							
bindsym Mod1+Left focus left								
bindsym Mod1+Down focus down							
bindsym Mod1+Up focus up								
bindsym Mod1+Right focus right							
bindsym Mod1+Tab focus right
bindsym Mod1+Shift+Tab focus left

bindsym Mod1+Shift+$left move left
bindsym Mod1+Shift+$down move down
bindsym Mod1+Shift+$up move up
bindsym Mod1+Shift+$right move right
bindsym Mod1+Shift+Left move left
bindsym Mod1+Shift+Down move down
bindsym Mod1+Shift+Up move up
bindsym Mod1+Shift+Right move right

bindsym Mod1+h split h
bindsym Mod1+v split v

bindsym Mod1+f fullscreen toggle

bindsym Mod1+e layout toggle split
bindsym Mod1+s layout stacking
bindsym Mod1+w layout tabbed

bindsym Mod1+a focus parent
bindsym Mod1+space focus mode_toggle
bindsym Mod1+Shift+space floating toggle

bindsym Mod1+minus scratchpad show
bindsym Mod1+Shift+minus move scratchpad

set $tag1 "1: www"
set $tag2 "2: code"
set $tag3 "3: read"
set $tag4 "4: shit"

bindsym Mod1+1 workspace $tag1
bindsym Mod1+2 workspace $tag2
bindsym Mod1+3 workspace $tag3
bindsym Mod1+4 workspace $tag4
bindsym Mod1+Shift+1 move container to workspace $tag1
bindsym Mod1+Shift+2 move container to workspace $tag2
bindsym Mod1+Shift+3 move container to workspace $tag3
bindsym Mod1+Shift+4 move container to workspace $tag4

bindsym Mod1+Shift+c reload
bindsym Mod1+Shift+r restart

bindsym Mod1+r mode "resize"
mode "resize" {
	bindsym $left       resize shrink width 10 px or 10 ppt
	bindsym $down       resize grow height 10 px or 10 ppt
	bindsym $up         resize shrink height 10 px or 10 ppt
	bindsym $right      resize grow width 10 px or 10 ppt
	bindsym Left        resize shrink width 10 px or 10 ppt
	bindsym Down        resize grow height 10 px or 10 ppt
	bindsym Up          resize shrink height 10 px or 10 ppt
	bindsym Right       resize grow width 10 px or 10 ppt
	
	bindsym Return mode "default"
	bindsym Escape mode "default"
}

bindsym Mod1+Control+Delete exec "i3-nagbar -t warning -fn 'xos4-terminus-14' -m 'System #---------------------------->> (l)lock, (e)logout, (s)suspend, (h)hibernate, (r)reboot, (Shift+s)shutdown.'" mode "System"
mode "System" {
    bindsym l 		exec sh ~/.config/i3/i3exit.sh lock, 		mode "default"
    bindsym e 		exec sh ~/.config/i3/i3exit.sh logout, 		mode "default"
    bindsym s 		exec sh ~/.config/i3/i3exit.sh suspend, 	mode "default"
    bindsym h 		exec sh ~/.config/i3/i3exit.sh hibernate, 	mode "default"
    bindsym r 		exec sh ~/.config/i3/i3exit.sh reboot, 		mode "default"
    bindsym Shift+s exec sh ~/.config/i3/i3exit.sh shutdown, 	mode "default"
    
    bindsym Return mode "default"
    bindsym Escape mode "default"
}

bar {
	colors {
		background #000000
		statusline #ffffff
		separator #666666

		focused_workspace  #4c7899 #285577 #ffffff
		active_workspace   #333333 #5f676a #ffffff
		inactive_workspace #333333 #222222 #888888
		urgent_workspace   #2f343a #900000 #ffffff
		binding_mode       #2f343a #900000 #ffffff
	}
	position top
	status_command i3status
	tray_output primary
}

#=========== GAPS
for_window [class="^.*"] border pixel 3
gaps inner 8 
gaps outer 18
smart_gaps on
smart_borders on
```

`i3exit.sh`
```bash
#!/bin/sh
lock() {    
    scrot /tmp/screen_locked.png
	convert /tmp/screen_locked.png -blur 2x2 /tmp/screen_locked2.png
	i3lock -n -ti /tmp/screen_locked2.png
}

case "$1" in
    lock)
        lock
        ;;
    logout)
        i3-msg exit
        ;;
    suspend)
        gksu pm-suspend
        ;;
    hibernate)
        lock && dbus-send --system --print-reply --dest="org.freedesktop.ConsoleKit" /org/freedesktop/ConsoleKit/Manager org.freedesktop.ConsoleKit.Manager.Hibernate
        ;;
    reboot)
        # dbus-send --system --print-reply --dest="org.freedesktop.ConsoleKit" /org/freedesktop/ConsoleKit/Manager org.freedesktop.ConsoleKit.Manager.Restart
        /sbin/reboot
        ;;
    shutdown)
        #dbus-send --system --print-reply --dest="org.freedesktop.ConsoleKit" /org/freedesktop/ConsoleKit/Manager org.freedesktop.ConsoleKit.Manager.Stop
        /sbin/poweroff
        ;;
    *)
        echo "Usage: $0 {lock|logout|suspend|hibernate|reboot|shutdown}"
        exit 2
esac

exit 0
```

`i3lock.sh`
```bash
scrot /tmp/screen_locked.png
convert /tmp/screen_locked.png -blur 2x2 /tmp/screen_locked2.png
i3lock -i /tmp/screen_locked2.png
```
