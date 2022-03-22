---
description: Learn how to create Slash Commands on your Discord Bot using the Pycord library!
authors: Pycord Guide
---

# Slash Commands

This page will show you how to use Slash Commands with Pycord. 

!!! important
	To create commands in a guild, your app must be authorized with the `applications.commands` scope. In order to make commands work within a guild, the guild must authorize your application with the `applications.commands` scope. The `bot` scope is not enough.

[Read the Docs](https://pycord.readthedocs.io/en/master/api.html#discord.Bot.slash_command)

## A simple Slash Command

```python
import discord
from discord.app import Option

bot = discord.Bot()

# Note: If you want you can use commands.Bot instead of discord.Bot
# Use discord.Bot if you don't want prefixed message commands

# With discord.Bot you can use @bot.command as an alias
# of @bot.slash_command but this is overriden by commands.Bot

@bot.slash_command(guild_ids=[...], name='hello', description='Say Hello!')  # create a slash command for the supplied guilds
async def hello(ctx):
    """Say hello to the bot"""  # the command description can be supplied as the docstring
    await ctx.respond(f"Hello {ctx.author}!")

bot.run("TOKEN")
```

The above code will create a guild-specific slash command. Global Slash Commands can take up to an hour to register.

!!! tip
	Use `ctx.respond` instead of `ctx.send` for Slash Commands. `ctx.respond` has all parameters of `ctx.send`, and avoids ":fontawesome-solid-exclamation-circle: Interaction Failed".

## Options

You might want to allow your users to be able to choose options. Here's how you do that:

```py
@bot.slash_command(name='member')
async def choose_a_user(
	ctx,
    user: Option(discord.Member, "Choose a member", required=False), #required is True by default
):
    await ctx.respond(f"You chose {user}!")

@bot.slash_command(name='channel')
async def choose_a_channel(
	ctx,
    channel: Option(discord.abc.GuildChannel, "Choose a channel"), # type, description
):
    await ctx.respond(f"You chose {channel}!")

@bot.slash_command(name='choice', description='Slash command description', guild_ids=[1234567891010, 103432234543])
async def choose(
	ctx,
	choice=Option(str, "Choose an option!", choices=["Choice1", "Choice2", "Choice3"])
):
	await ctx.respond(f"You chose {choice}!")
```


!!! info "Supported Option Types"
	
	| Name     | Value              | Note                           |
	|:---------|:-------------------|:-------------------------------|
	|INTEGER   |`int`| Any integer between -2^53 and 2^53|
	|BOOLEAN   |`bool`|     |	
	|USER	   | `discord.Member` |    |	
	|CHANNEL | `discord.abc.GuildChannel`|	Includes all channel types + categories|
	|ROLE|	`discord.Role`| |	
	|MENTIONABLE|	`discord.abc.Mentionable`|	Includes users and roles|
	|NUMBER| `float`|	Any double between -2^53 and 2^53 |

## Command Groups

### SubCommand Groups

![subcommand group](https://media.discordapp.net/attachments/881405655704039504/891685851195662386/unknown.png "Command Group")

The above image is an example of a SubCommand group. Slash Command names cannot have spaces, and they must all be in lowercase. The above Slash Commands do not have spaces, they are a command group.

A command group is a group of Slash Commands used for the same purpose. They can be useful certain times, as shown above. Now, lets learn how to create Command Groups.

```py
afk = bot.create_group(
    name='afk', description='AFK related commands', guild_ids=[...]) # guild_ids will make this a guild specific slash command. The whole command group can either be global, or guild-specific

@afk.command(name='set') #command instead of slash_command
async def set(ctx):
	await ctx.respond("You are now AFK!")

@afk.command(name='remove')
async def remove(ctx):
	await ctx.respond("You are no longer AFK!")

```


### SubCommand Groups within SubCommand Groups

![subcommand groups within subcommand groups](https://media.discordapp.net/attachments/682157382062964781/898886238621302794/unknown.png)

The below is an example of a SubCommand group within a SubCommand group. This is as far as SubCommand Groups go. Here's how you make one:

```py
settings = bot.create_group(name="settings", description="Change Server Settings", guild_ids=[100000000000000000])

prefix = settings.create_group(name="prefix", description="Change the server prefix")

@prefix.command()
async def set(ctx):
	await ctx.respond("Set the prefix to `!`")
```

## Limitations

Slash Commands, too, make their own limits.

- An app can have up to 100 top-level global commands with unique names

- An app can have up to an additional 100 guild commands per guild

- An app can have up to 25 subcommand groups on a top-level command

- An app can have up to 25 subcommands within a subcommand group

- commands can have up to 25 `options`

- options can have up to 25 `choices`
