---
description: Learn how to use threads with Pycord.
authors: Pycord Guide
---

# Threads
[Read the docs](https://pycord.readthedocs.io/en/master/api.html#discord.Thread)

![thread methods and attributes](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-Mk6vejx0Y4T_2gRSty8%2F-Mk79GBkFpQ-o230-mHv%2Fimage.png?alt=media&token=c7cec1fa-cc76-42b0-9ae3-4e0a2f53b6e6)

## Creating Threads

With a few lines of code, we can create threads in Py-cord. Now, here are a few things to keep in mind:

  - All public threads need a starting message. This message will start the thread. However, private threads (server boost level 2 required) do not require a starting message.
## Creating thread from a message
```py
message = await something.send("My Starting Message")
await message.create_thread(name="thread name", auto_archive_duration=60)
```

You may also use other ways, say, on_message or by using buttons.

## Creating thread in a channel
```py
channel = bot.get_channel(...) # define this!
await channel.create_thread(name="Thread Name", message=None, auto_archive_duration=60, type=None, reason=None)
```

A thread type could be `news_thread`, `public_thread`, `private_thread`. You may use it like `type=discord.ChannelType.news_thread`. 

## Deleting Threads
```py
thread = bot.get_channel(thread_id)  # a thread is a kind of channel too, so we use bot.get_channel
await thread.delete()
```

## Editing Threads

### Parameters

-    name (str) – The new name of the thread
-    archived (bool) – Whether to archive the thread or not.
-    locked (bool) – Whether to lock the thread or not.
-    invitable (bool) – Whether non-moderators can add other non-moderators to this thread. Only available for private threads.
-    auto_archive_duration (int) – The new duration in minutes before a thread is automatically archived for inactivity. Must be one of 60, 1440, 4320, or 10080.
-    slowmode_delay (int) – Specifies the slowmode rate limit for user in this thread, in seconds. A value of 0 disables slowmode. The maximum value possible is 21600.

```py
thread = bot.get_channel(id)  # a thread is a kind of channel too, so we use bot.get_channel
    await thread.edit(
        name="New Name",
        archived=True,
        locked=True,
        slowmode_delay=10,
        auto_archive_duration=60,
    )
```

## Joining Threads Automatically

If you have a Moderation Bot, you probably want your bot to join all the threads automatically. Only by doing so will you be able to Moderate Threads properly. Thankfully, there's an event for that.

```py
@bot.event
async def on_thread_join(thread): # called whenever a thread is created/joined
    await thread.join()
```

## More

There are ton of things you can do with threads. Reading this guide, you must have gotten a hang of how to use these features. Now, you should head to the docs, view all the things you can do and use the features you want accordingly.

Docs: [https://pycord.readthedocs.io/en/master/api.html#discord.Thread](https://pycord.readthedocs.io/en/master/api.html#discord.Thread)

### Cheatsheet
```py
# THREADS CHEATSHEET

import discord
from discord.ext import commands

bot = commands.Bot(command_prefix="!", Intents=discord.Intents.all())

## Creating Threads

@bot.command()
async def create(ctx):
    ### Creating Threads by message
    message = await ctx.send("I started a public thread!")
    await message.create_thread(name="New Thread", auto_archive_duration=60)

    ### Creating Threads by channel
    channel = bot.get_channel(...)
    await channel.create_thread(
        name="Thread Name",
        message=message,
        auto_archive_duration=60,
        type=None,
        reason="User told me to do so",
    )

@bot.command()
async def delete(ctx, id: int):  # will fail if you forget to make it an integer!
    thread = bot.get_channel(id)  # a thread is a kind of channel too, so we use bot.get_channel
    await thread.delete()

@bot.command()
async def edit(ctx, id: int):  # will fail if you forget to make it an integer!
    thread = bot.get_channel(id)  # a thread is a kind of channel too, so we use bot.get_channel
    await thread.edit(
        name="New Name",
        archived=True,
        locked=True,
        slowmode_delay=10,
        auto_archive_duration=60,
    )

@bot.event
async def on_thread_join(thread):
    await thread.join()

bot.run("TOKEN")
```

### Problem Solving
#### Unknown Message

If you get an error looking like `discord.ext.commands.errors.CommandInvokeError: Command raised an exception: HTTPException: 400 Bad Request (error code: 10008): Unknown Message`

There could be multiple reasons, for example,

- The message does not exist

- The message already has a thread

- The message is in channel x, you are trying to start a thread in channel y.

- The message was deleted.

#### Forbidden
This means that your bot does not have the necessary permissions to run the command you wanted. Give it the correct permissions, then try again.

#### `int` object has no attribute `id`
You probably used `message.id` instead of `message`.
If not, remember that message is an object. You need to get the object, not the specify the message id. You should try to learn about Object Oriented Programming.

#### AttributeError: 'NoneType' object has no attribute x
This probably means the bot was unable to fetch the thread. Maybe the thread doesn't exist?