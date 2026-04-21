---
sidebar_position: 2
---

# Installation and Setup

:::warning
As of this writing, we're still testing what's the best way in setting up the application.  
Right now, you have to manually input your ip.
:::
  
## Get your local ip
![alt text](/dev-guide/image.png) 

## Paste your ip in map_widget.dart for front-end
![alt text](/dev-guide/image2.jpg)

## Go to the backend codebase and install uvicorn
- `pip install uvicorn`  
And then run  
- `uvicorn main:app`