---
layout: post
title: Hue-CLI: Control the lights of your home from your terminal. Part #1
date: 2024-07-15 00:00:00 -0500
description: Launching a CLI tool to control your Phillips Hue devices.
categories: project hue-cli phillips-hue typescript
author: Angel Baez
---

Three years ago, I had a personal need. I wanted to control the lights in my studio from the terminal. Three years just flew by

And now, after a brief hiatus, I’m back on track with this project and gearing up for a significant launch (for my own sanity, and hopefully for your convenience too).

However, as I have limited time during the day, I will break down the tasks into manageable chunks and document my progress on my blog.

## The goal

Launch the first version of the hue-cli: a CLI tool to control your Phillips Hue devices.

The CLI tool is able to:

1. Connecting the CLI tool with the Hue Bridge.
2. See and manage your bridge
3. See and manage all your rooms
4. See and manage all your devices

## Commands:

hue init <your-bridge-ip> # connects the CLI too with Hue Bridge
hue rooms # see all your rooms
hue on <room> # turn on the lights of your room
hue off <room> # turn off the lights of your room

## Progress so far:

1. Read the Phillips Hue documentation
2. Started the repo
3. Worked on the first command hue init

## Tomorrow:

1. Continue with the hue rooms command
