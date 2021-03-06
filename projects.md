---
layout: page
title: Projects
---

What good is it to make stuff if I'm the only one using it? Most of these tools can be found on my [GitHub repo](https://github.com/tomchop/).

## Malcom
[Malcom](https://github.com/tomchop/malcom) is a tool designed to analyze a system's network communication using graphical representations of network traffic, and cross-reference them with known malware sources. This comes handy when analyzing how certain malware species try to communicate with the outside world.

## volatility-autoruns plugin
Finding persistence points (also called "Auto-Start Extensibility Points", or ASEPs) is a recurring task of any investigation potentially involving malware. To make an analyst's life a bit easier, I came up with the [**autoruns** plugin](https://github.com/tomchop/volatility-autoruns). **autoruns** basically automates most of the tasks you would need to run when trying to find out where malware is persisting from. Once all the autostart locations are found, they are matched with running processes in memory.

## unXOR
[unXOR](https://github.com/tomchop/unxor) will search through an XOR-encoded file (binary, text-file, whatever) and use known-plaintext attacks to deduce the original keystream. Works on keys half as long as the known-plaintext, in linear complexity.
