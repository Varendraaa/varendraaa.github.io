---
title: "Doom-like Game Engine Development"
description: ""
summary: "'"
date: 2024-08-21
lastmod: 2024-10-19
layout: "simple"
showTableofContents: false
categories: ["software"]
tags: ["web-scraping", "nomic", "chunkig", "retriever", "rag", "gabriel-garcia-marquez", "llm", "langchain", "chromadb", "retrieval-augmented-generation", "ollama", "llama3-1", "llama3-2"]
---

This is a very summarised overview of my MSc Thesis Project, done at the University of Dundee. It focuses on the creation and development of a Doom-like 3D FPS game, and its underlying engine, completely from first principles. It utilises simple, open-source C++ libraries to support some features, but every other aspect was done manually by the game code.

Since the thesis itself was over 18000 words, I'll not be going into a similar level of detail here. I'll highlight some of the ways I solved certain complex problems in development. If you're interested in reading it, please do send me a message.

# Background

For my thesis, there were a couple projects I was interested in. I ultimately decided to go with the game engine development for the following reasons:
1. It was something I'd be motivated to develop, and therefore avoid falling into the trap that is *lack of motivation.*
2. It offered me the opportunity to learn about graphics programming, specifically about OpenGL.
3. It was a **really cool idea.** I mean, I'm making a game that people could later play, and enjoy. **EXTREMELY DOPE.**

However, I severely underestimated the sheer amount of work required to get a good game going. I had a development window of around 8 weeks max, and I ended up working maybe 12-15 hours a day, learning as I went along.

# Development
## Initial Setup

My project ran over a 12 week timeline, from May 23rd 2024 to August 22 2024, but that also included the thesis write-up, poster creation and presentation preparation. My supervisor, Dr. Iain Martin, and I set out a goal of getting the core game done within 8-9 weeks, to allow enough time for revision, modification and other improvements.

The first couple weeks was spent getting up to speed with understanding OpenGL and the graphics pipeline. This was paramount since I'd need to learn how to manipulate objects throughout the pipeline if I ever wanted to even display a single pixel on the screen. Some fantastic resources I used were:
- [LearnOpenGL](https://learnopengl.com)
- [OpenGL-tutorial](https://www.opengl-tutorial.org/)
- [FreeCodeCamp's OpenGL Tutorials by Victor Gordon](https://www.youtube.com/watch?v=45MIykWJ-C4)

By the end of week 3, I had my first window displayed, my "Hello World" OpenGL triangle rendered, and a basic framework set up to let me start crafting my game.

## Picking a Game Concept

Early on, I created two gaming concepts that I'd be interested in making, each with their pros and cons:
1. **A racing game** not dissimilar to MarioKart or F-Zero, where players could race each other or AI opponents around a track.
    - Pros:
    - Cons:
2. **A Dungeon Crawler FPS** inspired by classics such as Wolfenstein and Doom, where a player character explores a dark dungeon while fighting off enemies.
    - Pros:
    - Cons:

Eventually, I settled on the Doom-like FPS game, and set about making the game work. 

## Figuring out FPS Games

Let me be the first to say, **your first game will probably be one of the most fun, yet challenging things you work on.** </br>


FPS games are among the most complex type of games to create, especially without the use of an engine. They need many complex systems to all work in tandem, and the best way to structure this is to look at ***Design Patterns.***
