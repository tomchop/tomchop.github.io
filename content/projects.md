---
title: "Projects"
date: 2023-03-23
draft: false
---

I enjoy building tools that help me solve the problems I run into during my
day-to-day job. Here's a list of FOSS I currently spend time contributing to:

## ⚡️ Core projects

### Yeti platform

Your Everyday Threat Intelligence platform. Yeti is a free and open source TIP
that is tailored to the needs of incident responders and detection engineers.
Probably the project that is taking me the most time outside of work.

GitHub: https://github.com/yeti-platform/yeti

### dfTimewolf

dfTimewolf is a framework for automating digital forensics and incident
response. Think CyberChef, but for APIs. It's a tool that allows you to chain
together evidence collection, processing and analysis in a scalable way.

GitHub: https://github.com/log2timeline/dftimewolf

### Timesketch

Timesketch is an open-source tool for collaborative forensic timeline analysis.
Using sketches you and your collaborators can easily organize your timelines and
analyze them all at the same time. Add meaning to your raw data with rich
annotations, comments, tags and stars.

GitHub: https://github.com/google/timesketch

## 📦 Projects I've worked on in the past

### FIR - Fast Incident Response

This was my first stab at a project in the area of DFIR. FIR was built as a
fast, simple ticketing system made by and for incident responders. I'm not an
active contributor anymore, but the team behind it is still going strong.

GitHub: https://github.com/certsocietegenerale/FIR

### volatility-autoruns plugin

Finding persistence points (also called “Auto-Start Extensibility Points”, or
ASEPs) is a recurring task of any investigation potentially involving malware.
To make an analyst’s life a bit easier, I came up with the autoruns plugin.
autoruns basically automates most of the tasks you would need to run when trying
to find out where malware is persisting from. Once all the autostart locations
are found, they are matched with running processes in memory.

GitHub: https://github.com/tomchop/volatility-autoruns

### Malcom

Malcom is a tool designed to analyze a system’s network communication using
graphical representations of network traffic, and cross-reference them with
known malware sources. This comes handy when analyzing how certain malware
species try to communicate with the outside world.

GitHub: https://github.com/tomchop/malcom

### unXOR

unXOR is a small golang script that will search through an XOR-encoded file
(binary, text-file, whatever) and use known-plaintext attacks to deduce the
original keystream. Works on keys half as long as the known-plaintext, in linear
complexity.

GitHub: https://github.com/tomchop/unxor
