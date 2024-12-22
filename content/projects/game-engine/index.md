---
title: "3D Game Engine Development"
description: ""
summary: "'"
date: 2024-08-21
lastmod: 2024-10-19
layout: "simple"
showTableofContents: false
thumbnail: "game1.png"
categories: ["software"]
tags: ["web-scraping", "nomic", "chunkig", "retriever", "rag", "gabriel-garcia-marquez", "llm", "langchain", "chromadb", "retrieval-augmented-generation", "ollama", "llama3-1", "llama3-2"]
---

*Developing a custom game and its engine from scratch.*

This is a very summarised overview of my MSc Thesis Project, done at the University of Dundee. It focuses on the creation and development of a Doom-like 3D FPS game, and its underlying engine, completely from first principles. It utilises simple, open-source C++ libraries to support some features, but every other aspect was done manually by the game code.

Since my thesis was over 17 000 words, this project post is not meant to be a deep dive into how I solved or implemented some of the game features. If you're interested in reading it (it's a good read!), shoot me an email.

**[Check out the project code here!](https://github.com/Varendraaa/Shadow)**

## Background

I had a couple projects that I was interested in doing for my thesis project:
- A Spacecraft Mission Visualiser 
- A Computer Vision System for Soil Evaluation (the old geoscientist in me was screaming "**Put me in chief!**")
- 3D Game Engine Development

I ultimately decided to go with the game engine development for the following reasons:
1. It was something I'd be excited to develop, and therefore avoid falling into the trap that is *lack of motivation.*
2. It offered me the opportunity to learn about graphics programming, specifically about OpenGL.
3. It was a **really cool idea.** I mean, I'm making a game that people could later play, and enjoy. **EXTREMELY DOPE.**

However, I severely underestimated the sheer amount of work required to get a good game going. My supervisor, Dr. Iain Martin, and I set out a goal of getting the core game done within 8-9 weeks, to allow enough time for revision, modification and other improvements. Since this was my first foray into learning both **graphics programming** and **game development**, *without the use of a commercial engine*, I ended up working maybe 12-15 hours a day, learning as I went along.

The first couple weeks was spent getting up to speed with understanding OpenGL and the graphics pipeline. Some fantastic resources I used were:
- [LearnOpenGL](https://learnopengl.com)
- [OpenGL-tutorial](https://www.opengl-tutorial.org/)
- [FreeCodeCamp's OpenGL Tutorials by Victor Gordon](https://www.youtube.com/watch?v=45MIykWJ-C4)


## Specifications

The game is a 3D Dungeon Crawler First Person Shooter, inspired by Doom. 
It, and it's engine, which I titled "Shadow" (just a working title that ended up sticking), was entirely coded using **C++ 17** and **GLSL (OpenGL Shading Language)**. Several other C++ libraries were used for different purposes, as detailed below.

| Library       | Usage |
| -----         | ----- |
| **GLAD**      | OpenGL library that manages functions and pointers for OpenGL, allowing for flexible cross-platform development.  |
| **GLFW**      | OpenGL library that manages windowing and input/output handling for the engine implementation.                    |
| **GLM**       | OpenGL Mathematics library that provides classes and functions that operate with the OpenGL framework. Used to support matrix, vector and shader calculations.                                                                                             |
| **ImGui**     | Lightweight C++ graphical user interface (GUI) library that operates within the 3D graphics pipeline. It outputs interfaces via vertex buffers, allowing for fast rendering implementations.                                                          |
| **irrKlang**  | Cross platform sound library that manages sound playback throughout the game engine implementation.                |
| **stb**       | Lightweight, single file C++ library used to load image data, in this case textures, into our game implementation.    |
| **tmxlite**   | Lightweight C++ library that enables easy parsing of data from the .tmx files that were used to configure level data. |

Additionally, the **Tiled Map Editor** was used to create the .tmx files, which holds the layout data necessary for generating level terrain (walls, ceilings and floors), their corresponding textures, enemy spawns and item locations. It allows for easy modification and reconfiguration of the level design, and facilitates easy addition of new textures and enemy types.

## Features
The following are some of the more complex features I integrated into the game system.

### Enemy Factory
Essentially a combination between two design patterns, the **Factory Design Pattern** and the **Decorator Design Pattern**. 

A base enemy class was defined, along with its parameters and functionalities that all enemy instances will share.
A particular enemy type is then created using the Factory Design Pattern, which then decorates the base enemy class with specific logic based on the enemy type (eg, zombie logic vs ogre logic.) 

### Animation via Texture Cycling
Animation in modern games is usually handled via a process called rigging, where a 3D model of an entity is created and then manipulated via some movement to its skeletal frame. **This was unfortunately not possible given the time constraints for development.**

Fear not! I still managed to animate enemies via a process I called **texture cycling**. Think about how old school cartoons were made. Textures were rendered onto a simple quad, and then cycled through a preset sequence, at pre-defined intervals, to create animations.

### Billboarding Sprites
Since the enemies and other objects were rendered onto 2D quads, I utilised billboarding to give what were 2D objects the iillusion that they exist in 3D space.

This was achieved by rotating the object's quad to always face the player's point of view (the camera's front vector.)
The quad's normal vectors were also dynamically recalculated to always ensure proper lighting calculations and effects.

### Dynamic Lighting Effects
Usually, techniques such as normal mapping, shadow mapping and ambient occlusion are commonly utilised to improve graphical fidelity.
Due to time constraints, I chose to:
- reduce ambient lighting to give the game a dark, "spooky" feel.
- Used the Phong lighting model to give the player a spotlight that illuminates a small section of the screen. This serves as the main exploration tool.
- Set the lighting matrices to dynamically update based on the player's location and direction vector. Therefore the player is able to "cast a light" on all surfaces, regardless of their position and direction.
- The spotlight was also attenuated (has a distance falloff) using a quadratic model.

###  Breath First Search (BFS) Pathfinding
In order to let the enemies chase after our player character, some type of pathfinding algorithm was needed. 
 
I chose to utilise a BFS graph traversal algorithm, which iterates over all possibilities between the root node (enemy's current location) and the goal node (player's current location) in order to find the shortest path to the player.
While not as efficient in time complexity (it has a linear time efficiency of O(V+E)), it was simple to implement and was more memory efficient when compared to some other algorithms.

### Collision Detection
Collision in games is often handled via Axis Aligned Boundary  Box (AABB) collision, due to its calculation efficiency.
I defined a bounding box for the player character, and created a system that checks if that box contacts any of the level's terrain meshes.

If collision is detected, player movement past the wall’s axis of orientation is stopped, but movement along it is allowed. This ensured two mechanics that significantly improved the fluidity of movement during gameplay:
- The player contacts the wall but does not get stuck onto it.
- The player can slide along the wall, meaning that player movement does not suddenly come to a dead stop.

###  Optimised Level Creation (*this is heavily summarised, since the actual logic I did was really complex.*)
The game's levels were generated using the Tiled Map Editor as a tool to allow easy modification. Tiled stores the layout data in essentially what is a 2D integer array of x-z positions in each layer. I used this, along with the humble cube, to generate the 3D levels.

I created a system using **OpenGL's vertex winding order** that would read the array data to determine if a cube should be placed at that position. I then had an algorithm check which faces of the cube would be obstructed by neighbouring terrain, i.e. if a cube face was side by side to another cube face, then we didn't need to render either since we couldn't see them.
The vertices of the unblocked faces, along with their corresponding texture IDs, were then passed into a mesh. 

This meant that the entire level could be created with a single draw call, massively improving runtime performance and efficiency.


