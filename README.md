# Communicating Material Culture Diversity by Creating 3D Online or Virtual Reality Scenes or Games with Three.js (Part 1)

## Kristine Hardy & Mathieu Leclerc

### This guide shows how to use the Three.js javascript library to create a website with 3D models to illustrate the diversity of the pottery technologies of communities in the Papua New Guinea area. Selecting a vessel model reveals information on the community and their ceramics. The website is also able to be viewed in Virtual Reality (VR) and can be the basis for a matching puzzle.

The use of interactive 3D models in websites enables examples of archaeological and historical material culture to be presented more effectively and has the potential to increase community engagement with the research. This ability can be further exploited by using WebXR to make the websites viewable in virtual reality (VR), with larger models with increased manipulability.   

There are several different ways fro creaters to make websites that include models, including game engines such as Unity and Unreal Engine. However websites can also be made relatively easily using the Three.js Javascript library. This guide provides an example of making such a website.

## Contents
-  [Lesson Goals](#lesson-goals)
- [Software Requirements and Installation](#software-requirements-and-installation)
- [Creating the Basic Web Page](#creating-the-basic-web-page)
- [Adding the Information Panels and Map](#adding-the-information-panels-and-map)
- [Adding the Jar Models](#adding-the-jar-models)
- [Giving the Jars Colours](#giving-the-jars-colours)
- [Adding Camera Controls to Move Around](#adding-camera-controls-to-move-around)
- [Adding Controls to Select a Jar](#adding-controls-to-select-a-jar)
- [Adding the Option to View in VR](#adding-the-option-to-view-in-vr)
- [Adding Jar Selection to VR](#adding-jar-selection-to-vr)
- [Adding Additional Jars](#adding-additional-jars)
- [Conclusion and Next Steps](#conclusion-and-next-steps)


## Lesson Goals
text
## Software Requirements and Installation

- Text editor (Visual Studio Code recommended).

VSC can be downloaded from https://visualstudio.microsoft.com, it is free and runs on Windows, MacOS, and Linux. It also features a terminal. Install as per website instructions.

- Terminal (ie Windows PowerShell, or the terminal in MacOS or Linux), or the terminal in VSC. 

Some simple command line typing will be required. Most importantly you need to be able to move to the folder that your website file will be in. If you use the VSC terminal, this should be automatic.

- Web browser. Chrome, Safari, Edge etc.

Chrome generally has the better developer tools for code debugging.

- Node.js (https://nodejs.org) is a free JavaScript tool. 

It is easy to install (Windows, macOS, and Linux). This will allow you to ‘serve’ code internally to your browser (using an address in the browser such as http://localhost:3000), and see if the code is working, or how code changes affect your site. Node.js is probably the easiest way to serve code internally. Install as per website instructions, and check it is working in your terminal by typing 

node -v

and confirming that you get a version number and not an error message. 

- A Github page (recommended). 

To you get one free page per account, ie my page at https://github.com/tosca-har/tosca-har.github.io, results in a website at https://tosca-har.github.io/. Alternatively you can deploy your site using a free service such as Vercel (https://vercel.com/).

- The Three.js library.

There are 2 ways to use the Three.js JavaScript library. This tutorial will use library via a content delivery network (CDN). Basically, code at the top of javascript script will fetch and import the library from a server. This removes the need for you to work with build tools like Vite, which you would have to do if you download the actual Three.js code. Downloading, working and building the code is more robust long term but for this lesson the CDN approach is fine.

Make a new folder - call it myscene.

In VSC open the folder.

Create a file and call it *index.html*  
Note that it **must** be called this.

We are going to put all the code in this file, this is not the best practice but the point of the lesson is to learn about Three.js.

In the index.html file, copy and paste the following.
```
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>PNG pottery</title>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
		<link type="text/css" rel="stylesheet" href="main.css">
		<script type="importmap">
            {
              "imports": {
                "three": "https://unpkg.com/three@0.160.0/build/three.module.js",
                "three/addons/": "https://unpkg.com/three@0.160.0/examples/jsm/"
              }
            }
          </script>
	</head>
	<body>

		<div id="info">
			<a href="https://threejs.org" target="_blank" rel="noopener">three.js</a> The Jars of Papua
		</div>

		<script type="module">
			import * as THREE from 'three';
		</script>
	</body>
</html>
```
Save the file.

In the myscene directory create another new file called 'main.css' and paste in the following. This file came from the examples folder at Three.js, it is a style file.
```
body {
	margin: 0;
	background-color: #000;
	color: #fff;
	font-family: Monospace;
	font-size: 13px;
	line-height: 24px;
	overscroll-behavior: none;
}

a {
	color: #ff0;
	text-decoration: none;
}

a:hover {
	text-decoration: underline;
}

button {
	cursor: pointer;
	text-transform: uppercase;
}

#info {
	position: absolute;
	top: 0px;
	width: 100%;
	padding: 10px;
	box-sizing: border-box;
	text-align: center;
	-moz-user-select: none;
	-webkit-user-select: none;
	-ms-user-select: none;
	user-select: none;
	pointer-events: none;
	z-index: 1; /* TODO Solve this in HTML */
}

a, button, input, select {
	pointer-events: auto;
}

.lil-gui {
	z-index: 2 !important; /* TODO Solve this in HTML */
}

@media all and ( max-width: 640px ) {
	.lil-gui.root { 
		right: auto;
		top: auto;
		max-height: 50%;
		max-width: 80%;
		bottom: 0;
		left: 0;
	}
}

#overlay {
	position: absolute;
	font-size: 16px;
	z-index: 2;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	display: flex;
	align-items: center;
	justify-content: center;
	flex-direction: column;
	background: rgba(0,0,0,0.7);
}

#overlay button {
		background: transparent;
		border: 0;
		border: 1px solid rgb(255, 255, 255);
		border-radius: 4px;
		color: #ffffff;
		padding: 12px 18px;
		text-transform: uppercase;
		cursor: pointer;
}

#notSupported {
	width: 50%;
	margin: auto;
	background-color: #f00;
	margin-top: 20px;
	padding: 10px;
}
```
Save the main.css file and then you can close it.

Save the index.html file.

Make sure that the command line of your terminal/shell indicates that you are in the myscene folder (...myscene %). In VSC Terminal > New Terminal will give you a terminal. In the terminal type

npx serve

this will serve your site, normally to port 3000, but check the message to see what local address is being used. Open a web browser and go to that address (ie http://localhost:3000) and if all is working you will see a black page with 'three.js The Jars of Papua'. 

To stop the server use Ctrl C in the terminal. You can restart with npx serve, or use the keyboard up arrows to find previous terminal commands. You may need to reload the page in the browser to apply any code changes. 


## Creating the Basic Web Page
First create a scene with a background colour and a camera. The position of the camera is important, sometimes you can not see your models because the camera is looking away from them. We will use a perspective camera. This background will be peach (0xf7d382).

In the index.html file, **after** the import declare the variables, call and define the init and other necessary functions. 

After:
```
   <script type="module">
    import * as THREE from 'three';

```		
add

	let container;
	let camera, scene, renderer;

    init(); 
	animate();

    function init() {
		container = document.createElement( 'div' );
		document.body.appendChild( container );
        scene = new THREE.Scene();
		scene.background = new THREE.Color( 0xf7d382 );
				
		camera = new THREE.PerspectiveCamera( 50, window.innerWidth / window.innerHeight, 0.1, 10 );
		camera.position.set( 0, 1.6, 3 );

        renderer = new THREE.WebGLRenderer( { antialias: true } );
		renderer.setPixelRatio( window.devicePixelRatio );
		renderer.setSize( window.innerWidth, window.innerHeight );
        container.appendChild( renderer.domElement );
        window.addEventListener( 'resize', onWindowResize );
	}
	function onWindowResize() {
		camera.aspect = window.innerWidth / window.innerHeight;
		camera.updateProjectionMatrix();
		renderer.setSize( window.innerWidth, window.innerHeight );
	}

	function animate() {
		renderer.setAnimationLoop( render );
	}

	function render() {
		renderer.render( scene, camera );
	}

Reload the page after saving the index.html file and check that you have changed the background colour.

Next we need to add lights and something to see.
There are several different types of lights. We will add a hemisphere light and a directional light. The hemisphere light has 2 colours and an intensity, while the directional light has one colour and a position. Use the values supplied first and if everything is working later you can experiment with different values.

In the function init() and after:
```
	camera.position.set( 0, 1.6, 3 );
```
add
```
    scene.add( new THREE.HemisphereLight( 0xffffbb, 0x080820, .5) );
	const light = new THREE.DirectionalLight( 0xffffff );
	light.position.set( 1, 6, 2 );
	scene.add( light );
```
Now we will add some colours and spheres.
A sphere 'geometry' is made with a size (in this case 0.04 m) and used for 9 different sphere meshes. Each sphere mesh gets a material with a colour. We are using the standard material. The colours are set in the parameters list. In this website they will relate to the method the potters used to make the jars. We want to colour the jars by how they were made. Some communities used coils, while others used moulding and the 'paddle and anvil' method. The spheres we are creating now will form part of the key that lets the viewer know how the pots were made, by having them in a parameter list, we can just change the hex code and the key and pots will all change. Start with these values and alter them later if you want.
For each sphere we also set its position in x, y, z order. Different graphics programs use different co-ordinate systems. In Three.js x is left (-) and right (+), y is down (-) and up (+) and z is far (-) and near (+).

After:
```
    let camera, scene, renderer;
```
Add:
```
    let ratio = 2;
	let desk = 0.8; // desk height
    let gheight = desk + 0.55; //panel height
	let psize = 1.0; // panel dimensions

```
and within the init function, after:
```
scene.add( light );
```
Add 
```
const parameters = {
		materialColor: '#9c5315', 
		ringTopColor: '#19ffE7',
		coilColor: '#ff0000',
		paddleColor: '#1e2f97', 
		coilBeatenColor: '#e8e337',
		paddleAddColor: '#a61ef4',
		wangelaColor: '#BEBEBE', 
		amphColor: '#fc9483',
		nabColor: '#209F00' 
    }
    //spheres for key
	const sphere = new THREE.SphereGeometry( 0.04, 15, 5); 

	const sphere1 = new THREE.Mesh( sphere,  new THREE.MeshStandardMaterial( {color: parameters.materialColor })); 
	sphere1.position.set( 0.84, gheight + (psize *.30), -.75); 

	const sphere2 = new THREE.Mesh( sphere,  new THREE.MeshStandardMaterial( {color: parameters.coilColor })); 
	sphere2.position.set( 0.84, gheight + (psize *.21), -.75); 

	const sphere3 = new THREE.Mesh( sphere,  new THREE.MeshStandardMaterial( {color: parameters.wangelaColor })); 
	sphere3.position.set( 0.84, gheight - (psize *.15), -.75); 

	const sphere4 = new THREE.Mesh( sphere,  new THREE.MeshStandardMaterial( {color: parameters.nabColor })); 
	sphere4.position.set( 0.84, gheight - (psize *.06), -.75); 

	const sphere5 = new THREE.Mesh( sphere,  new THREE.MeshStandardMaterial( {color: parameters.paddleAddColor})); 
	sphere5.position.set( 0.84, gheight - (psize *.35), -.75); 

	const sphere6 = new THREE.Mesh( sphere,  new THREE.MeshStandardMaterial( {color: parameters.coilBeatenColor})); 
	sphere6.position.set( 0.84, gheight + (psize *.03), -.75); 

	const sphere7 = new THREE.Mesh( sphere,  new THREE.MeshStandardMaterial( {color: parameters.amphColor })); 
	sphere7.position.set( 0.84, gheight - (psize *.44), -.75); 

	const sphere8 = new THREE.Mesh( sphere,  new THREE.MeshStandardMaterial( {color: parameters.paddleColor})); 
	sphere8.position.set( 0.84, gheight - (psize *.25), -.75); 

	const sphere9 = new THREE.Mesh( sphere,  new THREE.MeshStandardMaterial( {color: parameters.ringTopColor})); 
	sphere9.position.set( 0.84, gheight + (psize *.12), -.75); 

	scene.add( sphere1, sphere2, sphere3, sphere4, sphere5, sphere6, sphere7, sphere8, sphere9 );

```


## Adding the Information Panels and Map
Now we will add some planes.
After:
```
    let camera, scene, renderer;
```
Add:
```
    let gallery, adzeraG, aibomG, mailuG, dimiriG, louisadeG, yabobG;			
```
and within the init function, after:
```
tex
```

## Adding the Jar Models
text
## Giving the Jars Colours
text
## Adding Camera Controls to Move Around
text
## Adding Controls to Select a Jar
text
## Adding the Option to View in VR
It is more difficult if you want to test controller actions on a VR unit such as a Quest. There is an emulator for chrome (https://chrome.google.com/webstore/detail/immersive-web-emulator/cgffilbpcibhmcfbgggfhfolhkfbhmik)- but it only works to a limited extent. I use my Github page, after testing the page on my computer. As Github can take some time to rebuild/update, I recommend changing the title text, so that you know that you are looking at the updated code. Github may limit the number of updates you can do in an hour. 
## Adding Jar Selection to VR
text
## Adding Additional Jars
text
## Conclusion and Next Steps
text


