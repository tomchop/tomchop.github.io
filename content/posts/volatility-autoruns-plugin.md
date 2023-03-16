---
title: Volatility autoruns plugin
date: 2014-09-18
summary:
  Finding persistence points is a recurring task of any investigation
  potentially involving malware. Here's how to do it using Volatility.
tags:
  - memory forensics
  - volatility
  - tool
---

Finding persistence points (also called **Auto-Start Extensibility Points**, or
ASEPs) is a recurring task of any investigation potentially involving malware.

Checking for persistence is relatively easy when you have a forensic copy of the
hard-drive: tools such as
[Sysinternal's Autoruns](http://technet.microsoft.com/en-us/en-en/sysinternals/bb963902.aspx)
(and its lesser-known ability to analyze offline systems by manually selecting
registry hives) or
[regripper](https://code.google.com/p/regripper/wiki/RegRipper) and its
`user_run` and `soft_run` plugins all do the job perfectly.

When all you have is a live memory dump and your trusty Volatility framework,
things get a little more tedious since you have to play around with `printkey`
to display every possible Run or Service key. The `svcscan` plugin can come in
handy but it won't tell you which DLL the service is hosting or when was the
service created.

## Plugin features

To make an analyst's life a bit easier, I came up with the `autoruns` plugin.
`autoruns` basically automates most of the tasks you would need to run when
trying to find out where malware is persisting from. Once all the autostart
locations are found, they are matched with running processes in memory.

Today, the plugin goes through:

- Popular keys in the HKLM and HKCU hives (Run, RunOnce, etc.). This is the most
  common place for malware to establish persistence in.
- Winlogon parameters (such as `Shell` and `Userinit`) and Notify registrations
- Services: It will go through all services that are set to automatically start
  with Windows. If the service is invoked through `svchost.exe`, it will grab
  the loaded DLL name.
- AppInit DLLs: The DLLs specified here are loaded with every application that
  is ran on the system.
- Scheduled tasks in memory (tested on Windows 7 only)

Besides listing all these persistence points and their corresponding values, the
plugin will match them with a running process. This is particularly useful to:

- Immediately identify the process that corresponds to the service that loaded a
  suspicious-looking DLL.
- Determine which binaries where set to load at system start and are no longer
  running. Malware often injects itself into other processes before exiting, so
  an entry with no associated PIDs may be an indicator of strange activity.

## How-to

The plugin is pretty straightforward to use. The folder where the plugin is
located should be passed on to Volatility using the `--plugins=` parameter.

Relevant options for the plugin are:

- `-v` or `--verbose` - Shows extra information that would normally be filtered
  (like Services from the `System32` folder)
- `-t` or `--asep-type` - Use it to focus on specific ASEPS. Options are:
  `autoruns`, `services`, `appinit`, `winlogon`, and `tasks`. You can specify
  any combination of them with a comma-separated list: `autoruns,services`.
  Leave blank to get all ASEPs.
- `--output=[text|table]` - `table` will output the text in a table format (less
  readable but somehow more consice; see screenshot below). The default output
  mode is `text`, where more information is avialable.

Sample plugin output:

{{< image src="/img/plugin-sample-output.png" alt="Sample plugin output." position="center" style="border-radius: 8px;" >}}

## Roadmap

I plan on including some more ASEPs like Scheduled tasks _(done!)_ and Startup
folders. If you see any other way than going through the MFT, please do let me
know!

I also plan on extending support to OS X and hopefully Linux.

Since the plugin needs to parse a lot of registry keys, it can take a while to
run (it took approximately 3 minutes to do all the checks on the memory sample I
tested it on).

## Details and download

The plugin has its own
[GitHub repo](https://github.com/tomchop/volatility-autoruns). Check the
[README](https://github.com/tomchop/volatility-autoruns/blob/master/README.md)
there for more details on the specific checks that are made.

It was tested with Volatility 2.4 on several of the memory samples available
[here](https://github.com/volatilityfoundation/volatility/wiki/Memory-Samples).
