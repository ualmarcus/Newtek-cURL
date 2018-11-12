Newtek NDI Remote Control with cURL
------------------

It is  arbitrary to target and configure what Studio Monitor is doing (as long as you have not set a password, there are security implications, will add info for an authenticate the session when time permits) - this should give a starting point for all parameters: 

Accessing the source list and current configuration: 
```
curl http://[target studio monitorIP]/v1/configuration
```
- returns the current config of Studio Monitor and shows all parameters E.G.:
```
{"version":1,"NDI_source":"DESKTOP-UQM4V86 (Intel(R) HD Graphics 520 1)","NDI_overlay":"","PTZ_controller":"","audio_ou tput":"SONY TV *00 (Intel(R) Display Audio)","window":{"display_device":"","always_on_t op":false,"hide_border":false,"showcmd":1,"min_pos n_x":-1,"min_posn_y":-1,"max_posn_x":-1,"max_posn_y":-1,"normal_posn_left":413,"normal_posn_right":1853, "normal_posn_top":127,"normal_posn_bottom":873},"d ecorations":{"checkerboard":false,"picture_in_pict ure":true,"hw_accel":false,"tally":true,"low_bandw idth":false,"mute_audio":false,"audio_gain":-20,"vu_meter":true,"vu_meter_scale":false,"center_ cross":ffairlyalse,"safe_areas":false,"show_4_3":false," best_fit":true,"square_aspect":false,"hide_ptz_con trols":false,"show_alpha":false,"run_on_startup":f alse,"web_server":true,"show_web_url":true,"menu_p osn_x":56.0,"menu_posn_y":8.0}
```
```
curl http://[target studio monitorIP]/v1/sources
```
returns available NDI sources from this studio monitors perspective:
```
{"ndi_sources":["DESKTOP-UQM4V86 (Intel(R) HD Graphics 520 1)","DESKTOP-UQM4V86 (Microsoft Camera Front)","UAL.LOCAL (NDI Signal Generator)","UAL.LOCAL (Scan Converter)"],"audio_devices":["SONY TV *00 (Intel(R) Display Audio)"],"display_devices":["Full Screen"],"controllers":[],"studio_monitors":{""}
```


To post a config change for the primary NDI source:
-----

Main Window NDI source setting, Change a Source from something to 'None':

```
curl -H "Content-Type: application/json" -X POST -d '{"version":1,"NDI_source":""}' http://[target studio monitorIP]/v1/configuration
```

Change Source from something / none to a new source from the available listed at /v1/sources: 
```
curl -H "Content-Type: application/json" -X POST -d '{"version":1,"NDI_source":"DESKTOP-UQM4V86 (Intel(R) HD Graphics 520 1)"}' http://[target studio monitorIP]/v1/configuration
```

Picture in Picture:
-----

Add a new picture in picture source from the available listed at /v1/sources: 
```
curl -H "Content-Type: application/json" -X POST -d '{"version":1,"NDI_overlay":"DESKTOP-UQM4V86 (Intel(R) HD Graphics 520 1)"}' http://[target studio monitorIP]/v1/configuration
```

Remove picture in picture:
```
curl -H "Content-Type: application/json" -X POST -d '{"version":1,"NDI_overlay":""}' http://[target studio monitorIP]/v1/configuration
```
Temporarily toggle PIP on / off (take over whole screen, or return to PIP corner)
```
curl -H "Content-Type: application/json" -X POST -d '{"version":1,"decorations":{"picture_in_picture": true}}' http://[target studio monitorIP]/v1/configuration
```
```
curl -H "Content-Type: application/json" -X POST -d '{"version":1,"decorations":{"picture_in_picture": false}}' http://[target studio monitorIP]/v1/configuration
```

Example configuration settings 
------------------

(any Studio Monitor 3.5.x / 3.7.x setting can be targeted):

Switch on / off Hardware Acceleration:
```
curl -H "Content-Type: application/json" -X POST -d '{"version":1,"decorations":{"hw_accel":true}}' http://[target studio monitorIP]/v1/configuration
```
```
curl -H "Content-Type: application/json" -X POST -d '{"version":1,"decorations":{"hw_accel":false}}' http://[target studio monitorIP]/v1/configuration
```

Switch on / off Tally:
```
curl -H "Content-Type: application/json" -X POST -d '{"version":1,"decorations":{"tally":true}}' http://[target studio monitorIP]/v1/configuration
```
```
curl -H "Content-Type: application/json" -X POST -d '{"version":1,"decorations":{"tally":false}}' http://[target studio monitorIP]/v1/configuration
```

Hide / show window border:
```
curl -H "Content-Type: application/json" -X POST -d '{"version":1,"window":{"hide_border":true}}' http://[target studio monitorIP]/v1/configuration
```
```
curl -H "Content-Type: application/json" -X POST -d '{"version":1,"window":{"hide_border":false}}' http://[target studio monitorIP]/v1/configuration
```

Secondary / Additional windows on the same IP:
-----

The first window uses port 80 - the second 81 etc.
