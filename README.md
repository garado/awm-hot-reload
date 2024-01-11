# awm-hot-reload
Library to enable hot reloading colorscheme within AwesomeWM. Originally from [my own config](https://github.com/garado/cozy) and now being moved into its own library for others to enjoy.

**This only works with awesome-git or awesome-luajit-git. It will NOT work with the latest stable release.**

## In action
### From my config
![From my config](https://private-user-images.githubusercontent.com/50887322/269703459-85248a1d-81d9-4d11-a169-169bd4d1fd52.gif?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MDQ5MzkwODksIm5iZiI6MTcwNDkzODc4OSwicGF0aCI6Ii81MDg4NzMyMi8yNjk3MDM0NTktODUyNDhhMWQtODFkOS00ZDExLWExNjktMTY5YmQ0ZDFmZDUyLmdpZj9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDAxMTElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwMTExVDAyMDYyOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTNmMzI3NWJkNjE5MWI1YzMyNTkzN2Q1ZjRiOTQyMjhmYjhhMDgyZjYxZDk3ZGZhYTZiZDViNDJkZTdlOTNiYTMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.QzF-fVk0lxBTJ2g7Zipw8dn2kbjdfgJedfHJHXRgSpY)

## How to use
1. Copy the `wibox/` directory into `~/.config/awesome/`

2. Create a lookup table mapping your **old colors** to your **new colors**

It's easier to do this if you have a clean way to define your theme variables. Here's a simplified version from my config.

```lua
-- Here's the old theme variables...
local mountain_theme = {
  red       = #c49ea0,
  green     = #89ab8a,
  yellow    = #c4c19e,
  primary_1 = #c3baae,
  primary_2 = #bbb2a3,
  primary_3 = #a79a87,
  primary_4 = #92826b,
  primary_5 = #7d6a4f,
  neutral_1 = #dedede,
  neutral_2 = #afafaf,
  neutral_3 = #808080,
  neutral_4 = #515151,
  neutral_5 = #222222,
}

--- and here's the new theme variables.
local nord_theme = {
  red       = #bf616a,
  green     = #a3be8c,
  yellow    = #ebcb8b,
  primary_1 = #b5c5d9,
  primary_2 = #abbdd4,
  primary_3 = #92a9c7,
  primary_4 = #7895b9,
  primary_5 = #5e81ac,
  neutral_1 = #e5e9f0,
  neutral_2 = #bec3cd,
  neutral_3 = #969daa,
  neutral_4 = #6e7787,
  neutral_5 = #465064,
}

-- Then you can construct the LUT like this.
local theme_lut = {
  mountain_theme.red       = nord_theme.red,
  mountain_theme.green     = nord_theme.green,
  mountain_theme.yellow    = nord_theme.yellow,
  mountain_theme.primary_1 = nord_theme.primary_1,
  mountain_theme.primary_2 = nord_theme.primary_2,
  mountain_theme.primary_3 = nord_theme.primary_3,
  mountain_theme.primary_4 = nord_theme.primary_4,
  mountain_theme.primary_5 = nord_theme.primary_5,
  mountain_theme.neutral_1 = nord_theme.neutral_1,
  mountain_theme.neutral_2 = nord_theme.neutral_2,
  mountain_theme.neutral_3 = nord_theme.neutral_3,
  mountain_theme.neutral_4 = nord_theme.neutral_4,
  mountain_theme.neutral_5 = nord_theme.neutral_5,
}
```

3. Emit the `theme::reload` signal, passing the lookup table as a parameter

`awesome.emit_signal(theme::reload, theme_lut)`

## Other stuff
### How it works
It adds a connection to the `theme::reload` signal within the constructors for every default AwesomeWM widget. Once this signal is received, the widget updates its own colors.

Since it works by modifying AwesomeWM's pre-existing widgets, it might not 100% work out of the box if you are using any complicated custom widgets. To get those to work, you can follow the same implementation: within your widget's constructor, connect to `theme::reload` signal.

### Are there performance issues from adding so many new signals?
No idea. I don't notice anything new added latency though, and the startup time when watching the awmtt output remains the same.

### Disclaimer
This is also the method that [Kasper24](https://github.com/Kasper24/KwesomeDE) uses in his config. While I greatly admire his work (his config is way cooler than mine), this was developed totally independently. Great minds and all that!
