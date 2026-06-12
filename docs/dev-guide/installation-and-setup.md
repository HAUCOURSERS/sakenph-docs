---
sidebar_position: 2
---

# Installation and Setup

:::warning
As of this writing, we're still testing what's the best way in setting up the application.  
Right now, you have to manually input your ip.
:::

## Install requirements
- `python3 -m pip install -r requirements.txt`
  
## Get your local ip
![alt text](/dev-guide/image.png) 

## Paste your ip in \lib\globals\global_vars.dart for front-end
![alt text](/dev-guide/image2.png)

## Go to the backend codebase and install uvicorn
- `pip install uvicorn`  
And then run  
- `python3 -m uvicorn main:app --host 0.0.0.0`
And then enable an android emulator and run `flutter run`