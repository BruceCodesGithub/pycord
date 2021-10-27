---
title: Installation
description: How to install the Py-cord library - Pycord Guide
---

# :octicons-download-16: Installation

This page will show you how you can install Pycord to your device.

## Fresh Start

**Linux/macOS:**
```bash
python3 -m pip install -U py-cord
```
**Windows:**
```bash
py -3 -m pip install -U py-cord
```

!!! warning
	Remember that you need to install `py-cord`, not `pycord`.

If you need voice support for your bot, for example, playing music, you need to install the Voice Build.

**Linux/macOS:**
```bash
python3 -m pip install -U "py-cord[voice]"
```
**Windows:**
```bash
py -3 -m pip install -U py-cord[voice]
```


## Migrating

In case you are migrating from another module, say, `discord.py`,

```bash
python -m pip uninstall discord.py -y
```

Next, install `py-cord`.

```
python -m pip install py-cord
```

## Other Builds

### Alpha

!!! warning 
	The Alpha build can be unstable
!!! tip
	The Alpha build contains buttons, dropdowns, slash commands, context menu commands, and more features. 
You may install Pycord Alpha by using:

```bash
python -m pip install -U git+https://github.com/Pycord-Development/pycord
```

!!! note
	If you want to install this, you need GIT. [Click here to find out how you can install Git Software.](More/git.md)

### Voice

If you need voice support for your bot, for example, playing music, you need to install the Voice Build.

**Linux/macOS:**
```bash
python3 -m pip install -U "py-cord[voice]"
```
**Windows:**
```bash
py -3 -m pip install -U py-cord[voice]
```

## Repl.it

!!! danger
	Repl.it is not a good IDE. Its online, its free, but you should definitely not use it to host your bot, its not made for that purpose. Your bot will get slower as it gets bigger, and certain files might be deleted.

1. Create a file and name it `.replit`. Just `.replit`, with the dot/period at the start.
2. Next, you need to insert the following into the file.
	``` hl_lines="3"
	language="python3"
	run = """
	pip install py-cord
	python main.py
	"""

	[packager]
	ignoredPackages=["discord.py", "discord"]
	```

	!!! note
		You may need to change a few things according to your needs, for example, the filename or the installation command (`pip install py-cord`).

		Also, you do not need to install py-cord everytime Repl.it installs other libraries automatically for you, so its a problem. However, there is a fix to it too. When you run the program (line 3 does so), it keeps reinstalling `py-cord`, so you may remove it. However, if you are installing alpha, you need it, since repl.it keep rerunning the program after sometime, and installs py-cord stable instead of py-cord alpha if you don't specify it.