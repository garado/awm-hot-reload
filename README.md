# awm-hot-reload
Library to enable hot reloading colorscheme within AwesomeWM. Originally from [my own config](https://github.com/garado/cozy) and now being moved into its own library for others to enjoy.

## In action
### From my config
![From my config](https://private-user-images.githubusercontent.com/50887322/269703459-85248a1d-81d9-4d11-a169-169bd4d1fd52.gif?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDQ5MzkwODksIm5iZiI6MTcwNDkzODc4OSwicGF0aCI6Ii81MDg4NzMyMi8yNjk3MDM0NTktODUyNDhhMWQtODFkOS00ZDExLWExNjktMTY5YmQ0ZDFmZDUyLmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMTElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTExVDAyMDYyOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTNmMzI3NWJkNjE5MWI1YzMyNTkzN2Q1ZjRiOTQyMjhmYjhhMDgyZjYxZDk3ZGZhYTZiZDViNDJkZTdlOTNiYTMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.QzF-fVk0lxBTJ2g7Zipw8dn2kbjdfgJedfHJHXRgSpY)

## How to use
1. Copy the `wibox/` directory into `~/.config/awesome/`

2. Create a lookup table mapping your **old colors** to your **new colors**

```lua
# [Old color] = New color
local theme_lut = [
  [beautiful.fg_color_0] = "#eceff4",
  [beautiful.fg_color_1] = "#d8dee9",
  [beautiful.bg_color_0] = "#2e3440",
  [beautiful.bg_color_1] = "#3b4252",
  [beautiful.bg_color_2] = "#434c5e",
  [beautiful.bg_color_3] = "#4c566a",
]
```

3. Emit the `theme::reload` signal somewhere, passing the 

`awesome.emit_signal(theme::reload, theme_lut)`

## Other stuff
### How it works
It adds a connection to the `theme::reload` signal within the constructors for every widget. Once this signal is received, the widget updates its own color 

### Are there performance issues from adding so many new signals?
No idea. I don't notice anything new added latency though, and the startup time when watching the awmtt output remains the same.
