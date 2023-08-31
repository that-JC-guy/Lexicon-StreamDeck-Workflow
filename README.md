# Lexicon StreamDeck Workflow Enhancement

This repo contains the configuration files and instructions to setup enhanced workflows for Lexicon and StreamDeck integration.

[WORK IN PROGRESS, DO NOT USE YET]

## Advanced Workflow Control Using Lexicon and a StreamDeck XL

Starting from Christian's Lexicon blog post [Controlling Lexicon from your Stream Deck](https://discuss.lexicondj.com/t/controlling-lexicon-from-your-stream-deck/54), I've been working on speeding up my initial track setup workflow using my StreamDeck XL. I  have reached a point where I'm able to add all of my cuepoints (with my labelling/coloring preferences) while scanning through the track without ever touch my keyboard or mouse. To do this I am using a combination of tools, including the the Lexicon Plug-In for StreamDeck and Node-RED for advanced workflow management and Lexicon API communication. Since I expect that others might have an interest in doing likewise, I am creating this how-to so we can work together to improve the speed and efficiency of our own particular preparation workflow.

You do not need to know how to code to use this. All customization is handled in the StreamDeck. You can expand on the workflows to further customize it for you (an example of this can be seen in the "advanced_workflow.json" file.)
>[!IMPORTANT]
>To set this up, you need to be comfortable with doing the following:
>- Opening a command prompt/terminal window. 
>- Installing software.
>- Following directions. 

### Windows or MacOS?
To simplify the process, the setup is basically the same for both Windows and MacOS users (the two supported platforms for Lexicon.) If you are on Windows and don't want to install Docker, you can opt to install NodeJS and Node-RED locally, directions can be found [here](https://nodered.org/docs/getting-started/windows). Just ignore the Docker installation instructions below and be sure you have a Node-RED instance running somewhere. 

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
4. Import the StreamDeck profile (recommend starting with Basic):
   - Basic
   - Advanced

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

#### Configuring Node-RED
8. Copy the contents one of the workflows (basic recommended at first):
   - [./workflows/basic/basic_workflow.json](./workflows/basic/basic_workflow.json)
   - [./workflows/advanced/advanced_workflow.json](./workflows/advanced/advanced_workflow.json)
9. In your Node-RED Window, click the upper-right hamburger menu and select "Import".
10. Paste the content of the workflow JSON file into the "Import nodes" window and then click the "Import" button.
11. Double-click the "Lexicon Hotcue Workflow" tab to open its properties.
12. In the properties, click the "Environment Variables" button.
13. Change the "HOST_IP" value to your computer's local IP address and then click "Done".

### What have I done so far?
You have done the 

## Usage (Basic Workflow)
### Updating a hotcue
On the StreamDeck:
   - Press color key (e.g. orange)
   - Press label key (e.g. Drop)
   - Press Update Hotcue #
   ```
   To set hotcue 1 as orange with a "Drop" label -> Press "Orange", Press "Drop", Press "Update Hotcue 1".
   ```
>[!NOTE]
>If your hotcue selection only shows up as the color black, you likely are setting an invalid color in that color's id in StreamDeck.
>To check this, and "Invalid color sent" message will be in the Node-RED debug log along with a list of valid colors will be displayed.
 
>[!NOTE]
>Tips and tricks for managing your Node-RED docker instance can be found at [https://nodered.org/docs/getting-started/docker](https://nodered.org/docs/getting-started/docker)



