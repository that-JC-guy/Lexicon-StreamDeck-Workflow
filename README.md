a# Lexicon StreamDeck Workflow Enhancement

This repo contains the configuration files and instructions to setup enhanced workflows for Lexicon and StreamDeck integration.

[WORK IN PROGRESS, DO NOT USE YET]

## Enhanced Workflow Control Using Lexicon and a StreamDeck XL

Starting from Christian's Lexicon blog post [Controlling Lexicon from your Stream Deck](https://discuss.lexicondj.com/t/controlling-lexicon-from-your-stream-deck/54), this project was created to improved the efficiency of initial track preparation workflow using a StreamDeck XL. This project enables you to set all hotcues preferences (with your labelling/coloring preferences) while scanning through the track without ever touch your keyboard or mouse. 

A combination of tools are used to accomplish this, including the the Lexicon Plug-In for StreamDeck, the WebSocket Proxy for StreamDeck, and Node-RED for advanced workflow management and Lexicon API communication. 

You do not need to know how to code to use this. All hotcue customization (naming/coloring) is handled in the StreamDeck. 

If you are feeling adventurous, you can expand on the workflows to further customize it for your own needs (an example of this can be seen in the "advanced_workflow.json" file, which requires some knowledge of Node-RED, Javascript, and message flow.)

>[!IMPORTANT]
>To set this up, you need to be comfortable with doing the following:
>- Opening a command prompt/terminal window. 
>- Installing software.
>- Following directions. 

### Windows or MacOS?
To simplify the process, the setup is basically the same for both Windows and MacOS users (the two supported platforms for Lexicon.) If you are on Windows and don't want to install Docker, you can opt to install NodeJS and Node-RED locally, directions can be found [here](https://nodered.org/docs/getting-started/windows). Just ignore the "Installing Node-RED" installation instructions below and be sure you have a Node-RED instance running somewhere. 

### What you will need
- StreamDeck XL - [https://www.elgato.com/us/en/p/stream-deck-xl](https://www.elgato.com/us/en/p/stream-deck-xl) - You don't necessarily need the XL, it will work on smaller units, but it is really convenient and reduces page changes. 
- Lexicon Plug-In for StreamDeck - This will get you control options to manipulate Lexicon from your StreamDeck.
- WebSocket Proxy for StreamDeck - This is the interface from the StreamDeck to Node-RED. 
- Node-RED - This handles managing inputs from the StreamDeck (read: button pushes) and create the proper message formatting to send to the Lexicon API.
- Docker - This is the runtime environment for Node-RED. 

### Setup Guide
#### Getting the StreamDeck Ready
1. Get your StreamDeck and install the StreamDeck software.
2. Install the Lexicon Plug-In for StreamDeck. Follow the directions here: [https://discuss.lexicondj.com/t/controlling-lexicon-from-your-stream-deck/54](https://discuss.lexicondj.com/t/controlling-lexicon-from-your-stream-deck/54)
3. Install the WebSocket Proxy for StreamDeck. Go here - [https://apps.elgato.com/plugins/org.tynsoe.streamdeck.wsproxy](https://apps.elgato.com/plugins/org.tynsoe.streamdeck.wsproxy) - and then click "Install".
4. Download the StreamDeck profile  from the links below (recommend starting with Basic):
   - [Lexicon Basic Workflow.streamDeckProfile](./workflows/basic/Lexicon%20Basic%20Workflow.streamDeckProfile)
   - [Lexicon Advanced Workflow.streamDeckProfile](./workflows/advanced/Lexicon%20Advanced%20Workflow.streamDeckProfile)
5. Import the StreamDeck profile you downloaded by clicking the gear icon go to the "Profiles" tab, click the down arrow under the list of profiles and select "Import..."
6. Browse to the StreamDeck profile you downloaded and click "Open"

#### Installing Node-RED
4. Install Docker:
   - Browse to [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/).
   - Select the appropriate OS version (if it isn't automatically detected.)
   - Save it to your computer. Note where you saved it.
   - Double-click "Docker Decktop installer.exe" (for Windows) or "Docker.dmg" (for MacOS.)
   - Accept the default setup options and install Docker Desktop.
5. Open a command prompt (Windows) or terminal (MacOS).
6. Type the following:
```
docker run -it -p 1880:1880 -v node_red_data:/data --name mynodered nodered/node-red
```
   - This will download the Node-RED docker image, start it and setup the internal networking. 
7. Test if Node-RED is working by opening a web browser to this address - "http://127.0.0.1:1880". If you see the Node-RED page, you are good to continue.
>[!NOTE]
>Tips and tricks for managing your Node-RED docker instance can be found at [https://nodered.org/docs/getting-started/docker](https://nodered.org/docs/getting-started/docker)

#### Configuring Node-RED
8. Copy the contents one of the workflows (basic recommended at first):
   - [./workflows/basic/basic_workflow.json](./workflows/basic/basic_workflow.json)
   - [./workflows/advanced/advanced_workflow.json](./workflows/advanced/advanced_workflow.json)
9. In your Node-RED Window, click the upper-right hamburger menu and select "Import".
10. Paste the content of the workflow JSON file into the "Import nodes" window and then click the "Import" button.
11. Double-click the "Lexicon Hotcue Workflow" tab to open its properties.
12. In the properties, click the "Environment Variables" button.
13. Change the "HOST_IP" value to your computer's local IP address and then click "Done".

## What Now?
At this point, any changes you need to make will be in the StreamDeck software only. A default set of colors (e.g. "Green". "Violet", etc.) and hotcue names (e.g. "Groove", "Lyrics", etc.) have included in the StreamDeck profile.

To modify a Name in the StreamDeck software, simply click the name and its properties will appear below. The "Title" is the name you see on your StreamDeck, the "id" is the information that is sent when you click the button. The id
defines how Lexicon names the hotcue. 

Similarly, the colors work the same, but the id of colors should be entered all lower-case.

The first page of the StreamDeck profile sets and removes hotcues and controls some player functions (play/pause, skip forward/backward, etc.) You can also skip around the song using the "<1 M", "> 1M","< 16M", and "> 16M" buttons, moving 1 measure and 16 measure respectively. 

The second page of the StreamDeck profile updates the hotcues once set. It will not update a hotcue that doesn't already exist. 

## Usage
### Setting a hotcue
With the song you want to update loaded into the Lexicon player and playing, when you reach a point where you want to set a hotcue, simply click the "Hotcue #" button within one-half beat of the point you want it set. Once set, this will also allow you to jump to that hotcue as well.

### Deleteing a hotcue
With the song you want to update loaded into the Lexicon player, simply press the "Delete Hotcue #" you want to delete.

### Updating a hotcue
With the song you want to update loaded into the Lexicon player, on the StreamDeck:
   - Press color key (e.g. orange)
   - Press label key (e.g. Drop)
   - Press Update Hotcue #
   ```
   To set hotcue 1 as orange with a "Drop" label -> Press "Orange", Press "Drop", Press "Update Hotcue 1".
   ```
>[!NOTE]
>If your hotcue selection only shows up as the color black, you likely are setting an invalid color in that color's id in StreamDeck.
>To check this, and "Invalid color sent" message will be in the Node-RED debug log along with a list of valid colors will be displayed.
 


