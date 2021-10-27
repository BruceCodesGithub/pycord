---
description: Learn how create Context Menu Commands with Pycord!
authors: Pycord Guide
---

# Context Menu Commands


## What are User Commands and Message Commands?

![gif](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-MjSXhYe9K0uVV4_oRsn%2F-MjSYM78m_x2LUvCexhn%2FqxVMvwcH.gif?alt=media&token=90f20216-6860-4234-abbd-1e237224dc65)

When you right click a message, you may see a option called "Apps". Hover over it and you can see commands a bot can run with that message. These are called message commands.

When you right click a message in the user list, you can once again see an option called "Apps". Hover over it and you can see commands a bot can run with that message. These are called user commands.

## Creating User Commands

```py
@bot.user_command(guild_ids=[...])  # create a user command for the supplied guilds
async def mention(ctx, member: discord.Member):  # user commands return the member
    await ctx.respond(f"{ctx.author.name} just mentioned {member.mention}!")
```
If you want to make the command global, remove guild_ids. Note that global application commands can take up to an hour to register.

## Creating Message Commands

Similar to user commands,

```py
@bot.message_command(name="Show ID")  # creates a global message command. use guild_ids=[] to create guild-specific commands.
async def show_id(ctx, message: discord.Message):  # message commands return the message
    await ctx.respond(f"{ctx.author.name}, here's the message id: {message.id}!")
```