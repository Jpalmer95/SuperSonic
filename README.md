🚀 SuperSonic VR

A high-speed, browser-based WebXR flight simulator and combat game designed for the Meta Quest 3. Explore procedural cities, alien planets, or real-world locations generated instantly from OpenStreetMap data.

👉 PLAY NOW (Launch in Quest Browser)
https://jpalmer95.github.io/SuperSonic/

✨ Features

** immersive Flight:** Hand-directed flight mechanics ("Iron Man" style). Point your hands where you want to go and hold the triggers.

🌍 Multiple Environments:

Metro City: A dense, procedurally generated metropolis with day/night cycles.

Neon Cyberpunk: A dark, glowing synth-wave city.

Mars: A fully spherical planet with custom gravity physics and craters.

Real World: Import 3D building data from any city in the world using OpenStreetMap (OSM).

⚔️ Combat System:

Dual-wield laser blasters.

Enemy drone targets.

Shield integrity system with "Game Over" states.

🎵 Dynamic Music Player:

Loads music directly from a GitHub repository.

Supports local MP3 file uploads from your device.

Full playlist queue management (Add, Remove, Reorder).

🖥️ In-Game VR Menu:

Floating holographic menu system.

Adjust speed gears, change locations, and manage music without leaving VR.

🎮 Controls (Meta Quest 3)

Action

Input

Fly

Point Hands + Hold Triggers

Turbo Boost

Hold Grip (Inner Triggers)

Turn

Right Stick (Left/Right)

Shoot Lasers

Y Button (Left) or B Button (Right)

Open Menu

X Button (Left)

Select Menu Item

Point with Right Hand + Right Trigger

Change Speed Gear

Click Left Stick

🛠️ How it Works

This project is built using Three.js and the WebXR Device API.

Procedural Generation: Cities are generated on the fly using instanced meshes for performance.

Real-World Data: The app queries the Overpass API to fetch real-time building footprints and height data based on latitude/longitude, extruding them into 3D geometry.

Spherical Physics: When on Mars, the physics engine switches gravity to pull towards the center of the planet (0,0,0) and aligns the player's orientation to the surface normal.

Audio: Music is streamed via direct raw links (Github/CDN) or Blob URLs for local files to bypass CORS restrictions.

🚀 Running Locally

Clone the repository:

git clone [https://github.com/Jpalmer95/SuperSonic.git](https://github.com/Jpalmer95/SuperSonic.git)


This is a static web application. You cannot simply open index.html in a browser due to CORS security policies on textures and modules. You must use a local server.

VS Code: Install "Live Server" extension and click "Go Live".

Python: python -m http.server

Node: npx serve

Open the local host URL (usually http://localhost:5500) in your browser or port-forward to your headset.

🤝 Credits

Engine: Three.js

Map Data: OpenStreetMap & Overpass API

Sky/Lighting: Polyhaven (HDRIs)

Music: Community contributions and open-source tracks.

Note: This game is optimized for the Meta Quest 3 browser but may work on other WebXR-enabled headsets.
