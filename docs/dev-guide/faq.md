# ℹ️ FAQ

## Why does the path selector keep showing up during hot reloads?
- Assuming that you have asked the app for paths, the paths have been saved
into the app, so when the debugger reloaded, the listener proceeded to change the
system state to show suggested paths. 