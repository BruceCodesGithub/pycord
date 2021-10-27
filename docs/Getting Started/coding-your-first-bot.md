---
description: Learn how to make a Discord bot with Pycord using the Pycord Library!
authors: Pycord Guide
---

# Creating your first bot
## Creating the bot
Just like how you needed to sign up to Discord to get started, we need to get your bot signed up too. To do this, 

1. Go to the [Discord Developer Portal](https://discord.com/developers/applications) and click on <button class="blurplebutton">New Application</button>.
2. Give your bot a name, and click <button class="blurplebutton">Create</button>.
3. Now, you should see a page like this.
	![image](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-MjdW3OQnwUhacopqSWw%2F-Mjd_-mxrJCrzmaXrAg8%2Fimage.png?alt=media&token=b8e2ae6c-2290-4d37-ad7c-eb412f3fb00e)
4. Click on <button class="greybutton">Bot</button> tab on the left side of the screen.
5. Click on <button class="blurplebutton">Add bot</button>.
6. You can give it a name, change the Avatar, etc.

### Inviting the bot
Now, lets get the bot added to some servers. Go to the <button class="greybutton">OAuth2</button> tab in the left pane, and select `bot` and `applications.commands` as the scope.

The `applications.command`s scope allows the bot to use Slash Commands, which you may want to have.

Next, we choose what permissions the bot will have. You can select them. For now, lets give your bot the Administrator permission, meaning the bot will have all the permissions.
Once you select the permissions, click on copy to get the bot invite link.

![image](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-Mk6tNY3LfDkjd6pqdpL%2F-Mk6tkdpddEWoa2jczZk%2Fimage.png?alt=media&token=52c8a29f-a798-48f8-a8c7-4ecca2681f79)

You can use this link to invite the bot.

## Tokens
Now that we have an account for our bot, we need to login. In order to login, we need the bot's password.
All users and bots have a "token". You may think of a token as a unique password, since this is what we use to log into the bot and connect it to Discord.

Tokens are "snowflakes". Not actual snowflakes, though. Just like how no two snowflakes in real life have the same pattern, a snowflake in computers is a unique thing - no two bots have the same token - so a token is a snowflake. An ID is a snowflake.

Now, lets get our bot's token. To do this, 

1. Go back to the <button class="greybutton">Bot</button> tab. 
2. Click on the <button class="blurplebutton">Copy</button> button in the "Token" section.
	![image](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-MjdbU12JISJorAZxrKH%2F-MjdbpUsapzb5n15Po5P%2Fimage.png?alt=media&token=118e259f-940a-4f6c-b3a3-c29f3a54100d)

Now, you have your bot's token copied to your clipboard.

!!! danger
	Never leak your bot's token, and never share it with anyone. Even if you get any DMs and someone tells you to do so, maybe claiming to be Discord Staff, do not do so. They are probably lying and are scamming you. Anyone with your token will be able to access your bot fully. They will be able to do anything they want with your bot. 

	Never push it to GitHub, or send it with the code. One way to prevent your token from getting leaked is to store it in `.env` files.

### Storing the token in an ENV file
Storing your bot Token in an ENV File will increase its security, and prevent it from getting leaked. 

1. Create a file with the name `.env`. Just `.env`, with the dot/period at the start.
2. Define the token in the file, like,
	```env
	TOKEN = [PASTE YOUR TOKEN HERE]
	```
	for example,
	```env
	TOKEN = NzkyNzE1NDU0MTk2MDg4ODQy.X-hvzA.Ovy4MCQywSkoMRRclStW4xAYK7I
	```

## Coding the Basics
Make sure to install 
```bash
pip install python-dotenv
```
```py
import discord # make sure you install py-cord - read the Installation Page
from discord.ext import commands # will be discussed later
import os # default module
from dotenv import load_dotenv  # pip install python-dotenv

load_dotenv() # we load all the variables from the env file

bot = commands.Bot(command_prefix='!', case_insensitive=True)

@bot.event
async def on_ready():
    print(f"{bot.user} is ready and online!")

@bot.command()
async def hello(ctx):
    await ctx.send("Hey!")

bot.run(os.getenv('TOKEN'))
```

`import discord` In this line, we import the py-cord module.

`from discord.ext import commands` Frankly speaking, the Discord API doesn't have custom commands as you'd expect. It has events, interactions, and more. Hence, many people use the on_message event, which is triggered every time someone sends a message. To make commands, you need to use an extension called commands. Its installed with the library, so no worries!

`bot = commands.Bot(command_prefix='!', case_insensitive=True)` We create an instance of Bot. This connects us to Discord.

`@bot.event` The @bot.event() decorator is used to register an event. This is an asynchronous library, so things are done with callbacks. A callback is a function that is called when something else happens. In this code, the on_ready() event is called when the bot is ready to start being used.

We use the `@bot.command()` decorator to register a command. The callback also becomes the name of the command.  In the next lines, we send "Hey!" to the user.

`bot.run(os.getenv('TOKEN'))` We get our token from the `.env` file, and login to Discord


Run the bot, and you will have your first bot!

## Making more commands

### Ping
"Ping" or "Latency" of the bot is the time it takes to respond to your commands. The less the ping, the better! Ping is calculated in milli seconds, and here's how you can create a cool ping command.
```py
@bot.command(name="ping")
async def ping(ctx):
    await ctx.send(f"Pong! My latency is: {round(bot.latency * 1000)}ms")
```

A latency of about 300ms is common if you are running the bot from your PC. You may get a better ping by hosting the bot. The top bots pay for the best servers and get a ping of about 10-30ms. Thats 0.04 seconds!

### Embeds
![image](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-MjmZV6mCRB0M5Ln9snc%2F-Mjm_7ULHG8Zme3cokXh%2Fimage.png?alt=media&token=42897d4d-b4cc-443f-9d16-81d51b791a6e)

Embeds can make your bot's messages look a lot cooler. It also has other purposes, like fending off imposers - for example, people used to impersonate Dank Memer and trick users into giving them their items. Seeing this, the developers decided to use embeds - since embeds can only be sent by bots and webhooks, no users could impersonate any longer.

So, its clear that embeds are pretty nice. Adding them is simple enough too!

```py
@bot.command()
async def embed(ctx):
    embed = discord.Embed(title="How to make embeds with Py-cord", description="This tutorial will show you how to make embeds in py-cord. Simple enough, so lets get started!", color = discord.Color.from_rbg(255, 255, 255))
    await ctx.send(embed = embed)
```

=== "Fields"
	```py
	@bot.command()
	async def embed(ctx):
		embed = discord.Embed(title="How to make embeds with Py-cord", description="This tutorial will show you how to make embeds in py-cord. Simple enough, so lets get started!", color = discord.Color.from_rbg(255, 255, 255))
		embed.add_field(name="With Inline", value="This is what I would look like with inline", inline=True)
		embed.add_field(name="With Inline", value="This is what I would look like with inline", inline=True)
		embed.add_field(name="With Inline", value="This is what I would look like with inline", inline=True)
		embed.add_field(name="Without Inline", value="This is what I would look like without inline", inline=False)
		embed.add_field(name="Without Inline", value="This is what I would look like without inline", inline=False)
		await ctx.send(embed = embed)
	```
	![image](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-MjmZV6mCRB0M5Ln9snc%2F-Mjmbc0x5oXwtqS-43f4%2Fimage.png?alt=media&token=07cb472f-772a-41c2-8cd7-cc5414457bc8)

=== "Headers and Footers"
	```py
	@bot.command()
	async def embed(ctx):
		embed = discord.Embed(title="How to make embeds with Py-cord", description="This tutorial will show you how to make embeds in py-cord. Simple enough, so lets get started!", color = discord.Color.from_rbg(255, 255, 255), url="...")
		embed.add_field(name="With Inline", value="This is what I would look like with inline", inline=True)
		embed.add_field(name="With Inline", value="This is what I would look like with inline", inline=True)
		embed.add_field(name="With Inline", value="This is what I would look like with inline", inline=True)
		embed.add_field(name="Without Inline", value="This is what I would look like without inline", inline=False)
		embed.add_field(name="Without Inline", value="This is what I would look like without inline", inline=False)
		
		embed.set_footer(text="...", icon_url="...")
		embed.set_author(name="Pycord Guide", icon_url='...', url="...")
		
		await ctx.send(embed = embed)
		embed.set_footer(..., icon_url=ctx.author.avatar_url)
	```
	![image](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-Mjmbh6S0S_0sZYn0bTN%2F-Mjmdb0_cBWMauQtJGPe%2Fimage.png?alt=media&token=f9263004-7596-4acb-818d-3a797b9963b5)

### User Input
You may want to make commands that require the user to say something, for example, Guess the Number commands. Let's see how you can allow the user to be able to send their own input to the bot.

#### Args
In this kind of input, the user will be able to provide the input with the command itself, for example, `!gtn 5`.

```py
import random

@bot.command()
async def gtn(ctx, guess:int):
	number = random.randint(1, 10)
	if guess == number:
		await ctx.send("You guessed it!)
	else:
		await ctx.send("Nope! Better luck next time :)")
```

**Code Explanation**

`async def gtn(ctx, guess:int):` We set guess as a number parameter. However, if the user enters a value as `!gtn 5 5`, it will only take 5 as the input, splitting at spaces. To take the rest of the text as input too, you may use `async def gtn(ctx, *, guess:int):` instead. This is useful when the args is a string, for example, a feedback command.

`number = random.randint(1, 10)` Using the `random` module, we generate a random number between 1 to 10.

```py linenums="6"
if guess == number:
		await ctx.send("You guessed it!)
else:
		await ctx.send("Nope! Better luck next time :)")
```
We check if the user's guess was the same as the number that was generated, and inform them if they were right or wrong.

#### Wait for

We may want to provide multiple chances to the user, and in this case, args and parameters will not be useful. Instead, we will need to use `wait_for`.

```py
import random

@bot.command()
async def gtn(ctx):
    await ctx.send("Send your guess!")
    number = random.randint(1, 10)
    guess = await bot.wait_for('message', check=lambda message: message.author == ctx.author, timeout=3.0)
    if int(guess.content) == number:
        await ctx.send("Wow! You guessed it!")
    else:
        await ctx.send("Wrong!")
```

## More
There are tons of things you may do. This guide can show you how to do it. **What** you can do, that is up to you. Make sure to learn basic Python, and you will be able to create wonders with Discord Bots. Meanwhile, here are a few ideas to get started:

- Coin Toss Command (Tip: Similar to the `Guess the Number` example.)
- Quotes and Jokes Command (Tip: Use an API! Learn about the `requests` or `aiohttp` module, and use an API, for example, Random Stuff API)
- A simple economy command! (Tip: Learn about Databases. You may learn `SQL` and use the `aiosqlite` module.)

### Final Code
Here is all the code we wrote so far:
```py
import discord # make sure you install py-cord - read the Installation Page
from discord.ext import commands # will be discussed later
import os # default module
from dotenv import load_dotenv  # pip install python-dotenv
import random # default module

load_dotenv() # we load all the variables from the env file

bot = commands.Bot(command_prefix='!', case_insensitive=True)

@bot.event
async def on_ready():
    print(f"{bot.user} is ready and online!")

@bot.command()
async def hello(ctx):
    await ctx.send("Hey!")

@bot.command(name="ping")
async def ping(ctx):
    await ctx.send(f"Pong! My latency is: {round(bot.latency * 1000)}ms")

@bot.command()
async def embed(ctx):
    embed = discord.Embed(title="How to make embeds with Py-cord", description="This tutorial will show you how to make embeds in py-cord. Simple enough, so lets get started!", color = discord.Color.from_rbg(255, 255, 255), url="...")
    embed.add_field(name="With Inline", value="This is what I would look like with inline", inline=True)
    embed.add_field(name="With Inline", value="This is what I would look like with inline", inline=True)
    embed.add_field(name="With Inline", value="This is what I would look like with inline", inline=True)
    embed.add_field(name="Without Inline", value="This is what I would look like without inline", inline=False)
    embed.add_field(name="Without Inline", value="This is what I would look like without inline", inline=False)

    embed.set_footer(text="...", icon_url="...")
    embed.set_author(name="Pycord Guide", icon_url='...', url="...")

    await ctx.send(embed = embed)

@bot.command()
async def gtn(ctx):
    await ctx.send("Send your guess!")
    number = random.randint(1, 10)
    guess = await bot.wait_for('message', check=lambda message: message.author == ctx.author, timeout=3.0)
    if int(guess.content) == number:
        await ctx.send("Wow! You guessed it!")
    else:
        await ctx.send("Wrong!")




bot.run(os.getenv('TOKEN'))
```