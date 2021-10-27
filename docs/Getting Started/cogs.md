---
description: Learn how to use Cogs in Pycord to create your own Discord Bot - detailed guide with examples - Pycord Guide
authors: Pycord Guide
---

# Cogs

You must have seen "modules" listed in a bot's help command. These are called "cogs". Instead of having all your code in the Main file, you can create Cogs for each type of command. For example, `meme` and `anime` commands go to a `Fun` cog, `help` and `ping` to `Misc`, and so on. 

But how do you create Cogs in the first place? Well, lets learn!

First, lets create a folder named "`cogs`". This is where we will store different Cogs.

Next, create a file in the folder with the name of the cog - for example, `greetings.py`. 

Now, lets get coding.

=== "`greetings.py`"

	``` py

	import discord
	from discord.ext import commands

	class Greetings(commands.Cog, name="Greetings"): # We Subclass commands.Cog . if the name kwarg is not passed, the Cog name is taken to be the same as the class name
 	   def __init__(self, bot):
 		   self.bot = bot

		@commands.Cog.listener() # just like @bot.event
		async def on_member_join(self, member): # called when a member joins
 		   channel = member.guild.system_channel # get the guild's system messages channel - where welcome and boost messages are sent. You can change this to bot.get_channel(channel_id)
			if channel is not None:
 			   await channel.send('Welcome {0.mention}.'.format(member))

		@commands.command() # this is how you create a command in a Cog. The same as @bot.command()
		async def hello(self, ctx, *, member: discord.Member = None): # always remember to pass "self"!
			"""Says hello""" # command description
			member = member or ctx.author 
			await ctx.send('Hello {0.name}~'.format(member))

	def setup(bot): 
		bot.add_cog(Greetings(bot)) # register the cog 
	```

Now, lets add some things in `main.py`/`bot.py`/your main bot file.

=== "`bot.py`"
	```py
	bot.load_extension('cogs.greetings') # cogs_folder.filename

	# OR, if you want to load multiple cogs, its better to use:

	for ext in ['cogs.greetings', 'cogs.fun', 'cogs.anime']:
		bot.load_extension(ext)

	```

And that's it! You have your first Cog! Of course, you can do a lot more things, but for that you need to [Read the Docs](https://pycord.readthedocs.io/en/master/ext/commands/cogs.html)