```
# dev
BACKBLAZE_KEY_ID=2e59b692c328
BACKBLAZE_APPLICATION_KEY=00242446495e15579d801c495b2a3472b5a7b5ac23

# prod
BACKBLAZE_KEY_ID=889445f36885
BACKBLAZE_APPLICATION_KEY=0042e31fe17b69f9250b5023946b5b7cdd3fdc9a89
```


## Cloudflare Configuration

### DNS
![image](https://user-images.githubusercontent.com/7555972/203256069-b5674ac1-2fcf-4735-b7ef-5bc51549f808.png)

### Page Rules
![image](https://user-images.githubusercontent.com/7555972/203256006-e3212ac3-a442-4eac-8954-fc36e0e414cb.png)

<hr>

## Pull Zones

### General

#### Origin

- Origin URL: _https://f002.backblazeb2.com/file/upydemo2022_

- Pricing & Routing: **Standard Tier**
  ![image](https://user-images.githubusercontent.com/7555972/203254767-bfe880d7-dcf7-4944-a717-1fa5948877c3.png)

- Pricing & Routing: **High Volume Tier**

#### Bunny Optimizer

- Basic Settings
![image](https://user-images.githubusercontent.com/7555972/203255726-f20cbbc5-ad7b-41cd-92c6-1a5eec46f403.png)

- Image Classes

  There are 3 main classes used:
  - **feed**: width=450; quality=55;
  - **thumbnail**: width=250; quality=25;
  - **view**: width=450; quality=40;
