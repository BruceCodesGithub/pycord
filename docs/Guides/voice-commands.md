---
description: Learn how to create and use voice commands with Pycord.
authors: Pycord Guide
---
# Voice Commands
## Installation
```bash
# Linux/macOS
python3 -m pip install -U "py-cord[voice]"

# Windows
py -3 -m pip install -U py-cord[voice]
```
## Joining a voice channel
```py
@bot.command()
async def join(ctx):
    voice = ctx.message.author.voice

    if voice != None:
        await voice.channel.connect()
        await ctx.send(f"Connected and bound to {voice.channel.mention}!")
    else:
        await ctx.send(
            "You need to be connected to a voice channel to use this command!"
        )
```

## Leaving a voice channel
```py
@bot.command()
async def leave(ctx):
    voice = ctx.voice_client
    if voice != None:
        await voice.disconnect()
        await ctx.send(f"Left the VC!")
    else:
        await ctx.send("I am not connected to any voice channel!")
```

## Playing Audio
```py
@bot.command(name="play")
async def play(ctx):
    voice_channel = ctx.author.voice.channel
    if voice_channel != None:
        vc = await voice_channel.connect()
        vc.play(
            discord.FFmpegPCMAudio(
                executable=r"C:\Path\ffmpeg.exe",
                source=r"C:\Users\HP\Downloads\never_gonna_give_you_up.mp3",
            )
        )
        await ctx.send(f"Connected to {voice_channel.name}, playing audio.")
    else:
        await ctx.send("You need to be in a voice channel to use this command")
```

This is how you play local audio files.
Remember to replace Path and source with your own values. 
Eg. C:\KMPlayer\ffmprg.exe for me. You can find the path through searching in file explorer on windows.

![image](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-Mjw00fjnEorTW0tuw2m%2F-MjwBIGxscMC_m_9JH4b%2Fimage.png?alt=media&token=73a4c216-c4b0-4841-b421-8106a586ba71)



There are several things you can do, these are just the basics. For more commands like stop, resume, pause you will need to read the docs. Its pretty simple and easy to figure out.