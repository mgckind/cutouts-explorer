# Cutouts image explorer

This is an interactive tool for visualize and classify multiple images at a time. It written in Python and Javascript. It is based on [Leaflet](https://leafletjs.com/) and it reads the images from a single directory (there is no need for multiple resolutions as images are scaled dynamically). It running asyncio server in the back end and support up 10,000 images reasonable well. I can load more images but it will slower.

You can move and label images all from the keyboard.

### Deployment

#### Simple deployment

Clone this repository:

		git clone https://github.com/mgckind/cutouts-explorer.git
		cd cutouts-explorer/python_server

Create a config file template:

		cp config_template.yaml config.yaml

Edit the `config.yaml` file to have the correct parameters, see [Configuration](#Configuration) for more info.

Start the server:

	   python3 server_aio.py

Start the client and visit the url printed python_server:

	   python3 client.py

If you are running locally you can go to [http://localhost:8000/](http://localhost:8000/)

#### Docker

1. Create an internal network

        docker network create --driver bridge cutouts

2. Start the server container and attach the volume with images, connect to network and expose port

	   docker run -it --name server -p 8888:8888 -v {PATH_TO_IMAGE}:{PATH_TO_IMAGES} --network cutouts cex sh

3. Start the client container, connect to network and expose the port

	   docker run -it --name client -p 8000:8000  --network cutouts cex sh

Now the containers can talk internally and names are dns resolved.

### Usage

This is the Help window displayed
----

<h3>Help</h3> <hr><span><i class="fa fa-square-o" aria-hidden="true"></i> -&gt; Fullscreen</span> <br><span><i class="fas fa-star-half-o"></i> -&gt; Invert colors</span> <br><span><i class="fa fa-eye" aria-hidden="true"></i>/<i class="fa fa-eye-slash" aria-hidden="true"></i> -&gt; Toggle On/Off classified tiles. <br>First time it reads from DB.</span> <br> <span><i class="fa fa-random" aria-hidden="true"></i> -&gt; Random. Show a new random subsample (if available data is larger)</span> <br><span><i class="fa fa-filter" aria-hidden="true"></i> -&gt; Apply filter to the displayed data.</span> <br> Use the checkboxes on the left bottom side. -1 means no classified. <br><span><i class="fa fa-refresh" aria-hidden="true"></i> -&gt; Reset fiters and view. Do not display deleted images.</span> <br> <hr><p> Move around with mouse and keyboard , use the mouse wheel to zoom in/out and double click to focus on one image.</p><h4> Keyboard </h4>Use "w","a","s","d" to move the seleted tile and the keyboard numbers to apply a class as defined in the configuration file <br>Use "+", "-" to zoom in/out <br> Use "c" to clear any class selection <br> Use "t" to toggle on/off the classes <br>Use "h" to toggle on/off the Help<br> Use "f" to toggle on/off Full screen <br>Defined classes will appear at the bottom right side of the map

----

### Configuration

This is the template config file to use:

```
dataname: '{FILL ME}' #Name for the sqlite DB and config file
path: '{FILL ME}' #Path to png images
nimages: 1500 #Number of objects to be displayed even if there are more in the folder
xdim: 100 #X dimension for the display
ydim: 30 #Y dimension for the display
tileSize: 256 #Size of the tile for which images are resized at max zoom level
minXrange: 0
minYrange: 0
#### SERVER
server:
  ssl: false #use ssl, need to have certificates
  sslName: test #prefix of .crt and .key files inside ssl/ folder e.g., ssl/{sslName.key}
  host: 'http://localhost' #if using ssl, change to https
  port: 8888
  #workers: None # None will default to the workers in the machine
#### CLIENT
client:
  host: 'http://localhost'
  port: 8000
#### classes, use any classes from 0 to 9
classes:
    - Elliptical: 9
    - Spiral: 8
    - Other: 7
    - Delete: 0
```
