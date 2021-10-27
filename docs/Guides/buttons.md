---
description: Learn how to create and use buttons in your Discord bot with the Pycord library!
authors: Pycord Guide
---

# Buttons
[Read the Docs](https://pycord.readthedocs.io/en/master/api.html#discord.Button)


## Usage
### Subclassing discord.ui.View

In order to make a proper button which does something on click, you may need to use this method. 

By subclassing, we mean that our class gets all the attributes of `discord.ui.View`.
Here's an example of a simple button.

```py
class MyView(discord.ui.View):
    @discord.ui.button(label='A button', style=discord.ButtonStyle.primary, emoji='😎')
    async def button_callback(self, button, interaction):
        await interaction.response.send_message('Button was pressed', ephemeral=True)

@bot.command()
async def button(ctx):
    await ctx.send('Press the button!', view=MyView())
```

### Subclassing discord.ui.Button

We may use this instead of subclassing `discord.ui.View`.

```py
class MyButton(discord.ui.Button):
    def __init__(self):
        super().__init__(label='A button', style=discord.ButtonStyle.primary)

    async def callback(self, interaction):
        await interaction.response.send_message('Button was pressed', ephemeral=True)

view = discord.ui.View()
view.add_item(MyButton())
@bot.command()
async def ctx.send('Press the button!', view=view)
```

However, this method can become way longer.

### Using buttons without subclassing
You CAN send buttons without subclassing them, however, they won't be able to have a callback. They won't be able to respond. Thankfully, in URL Buttons, we don't need to respond, so we can this method.
```py
view = discord.ui.View()
view.add_item(discord.ui.Button(label='Go to website', url='https://gist.github.com/MhmCats/500eafdad0aaf278b94c612764688976', style=discord.ButtonStyle.url))
```

## Styles

![button styles image](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-MjRmS_9-Ymj5w997Vyy%2F-MjS3UTWUlcj14jreQ-m%2Fimage.png?alt=media&token=fe5b4b74-7688-4388-a003-176bcec77173)
![button styles image](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-MjRmS_9-Ymj5w997Vyy%2F-MjS3tajHE3t5XKC_UHC%2Fimage.png?alt=media&token=4b1b62b3-16cd-4640-83d2-5983a848be35)

| Name     | Usage                          | Color  |
| ----------- | ----------------------------|------- |
| Primary  | `discord.ButtonStyle.primary`  | Blurple|
| Secondary| `discord.ButtonStyle.secondary`| Grey   |
| Success  | `discord.ButtonStyle.success`  | Green  |
| Danger   | `discord.ButtonStyle.danger`   | Red    |
| Link     | `discord.ButtonStyle.link`     | Grey   |

## Timeouts

You may want to set timeouts, so that after a certain time, the buttons will stop working. Its quite easy!

```py hl_lines="8"
class MyView(discord.ui.View):
    @discord.ui.button(label='A button', style=discord.ButtonStyle.primary, emoji='😎')
    async def button_callback(self, button, interaction):
        await interaction.response.send_message('Button was pressed', ephemeral=True)

@bot.command()
async def button(ctx):
    await ctx.send('Press the button!', view=MyView(timeout=30))
```

This will stop the button from working after 30 seconds, however, the button will simply stop working, and if someone tries to use it, it will simply show "Interaction Failed". You might want to replace it with a custom message, and perhaps disable the buttons too!
```py hl_lines="4-7"
class MyView(discord.ui.View):
    def __init__(self):
        super().__init__(timeout=10)
    async def on_timeout(self):
        for child in self.children:
            child.disabled = True
        await self.message.edit(content="You took too long!", view=self)

    @discord.ui.button(style=discord.ButtonStyle.primary)
    async def callback(self, button, interaction):
        await interaction.response.send_message('Button was pressed', ephemeral=True)


@bot.command()
async def hi(ctx):
    view = MyView()
    view.message = await ctx.send("hi", view=view)
```

## Action Rows
A message can have up to five "action rows" and each of these "action" rows have five slots where you can put message components. A button takes up one of these slots but a select menus takes up all five slots of a "action row". Keep this in mind when creating your views since you don't want to run out of space!

Action rows are handled internally, and you can pass the `row` kwarg to use them.

Example:
```py hl_lines="2 6"
class MyView(discord.ui.View):
    @discord.ui.button(label="Button 1", row=1, style=discord.ButtonStyle.primary)
    async def first_button_callback(self, button, interaction):
        await interaction.response.send_message("You pressed me!")

    @discord.ui.button(label="Button 2", row=2, style=discord.ButtonStyle.primary)
    async def second_button_callback(self, button, interaction):
        await interaction.response.send_message("You pressed me!")
        
@bot.command()
async def button(ctx):
    view.message = await ctx.send("Wow!", view=MyView())
```

## Persistent Views

When your bot goes offline, all active buttons will become useless, like a remove that does nothing. You will be able to see the buttons, but nothing will happen when you press them. If you are trying to make self roles with buttons, then it will be useless! This is where Persistent Views step in.

Persistent views will keep responding forever - if the bot goes offline, it will just hibernate. When it comes back online, it will start working again! These views have no timeout, and are active forever.

Persistent Views have no timeout, and all items in them have a `custom_id` set.

```py hl_lines="3 7 9"
@bot.event
async def on_ready():
	bot.add_view(MyView())

class MyView(discord.ui.View):
	def __init__(self):
		super().__init__(timeout=None)

	@discord.ui.button(label='A button', custom_id="button-1", style=discord.ButtonStyle.primary, emoji='😎')
	async def button_callback(self, button, interaction):
		await interaction.response.send_message('Button was pressed', ephemeral=True)

@bot.command()
async def button(ctx):
    await ctx.send(f'Press the button! Is Persistent: {MyView.is_persistent(MyView())}', view=MyView())
```

## Disabling Buttons
### Making Pre-Disabled Buttons
```py hl_lines="2"
class MyView(discord.ui.View):
    @discord.ui.button(label='A button', style=discord.ButtonStyle.primary, disabled=True)
    async def button_callback(self, button, interaction):
        ...
        
@bot.command()
async def button(ctx):
    await ctx.send('Press the button!', view=MyView())
```

### Disabling Buttons on Press
=== "Disabling a single button"
	```py hl_lines="4-6"
	class MyView(discord.ui.View):
    @discord.ui.button(label='A button', style=discord.ButtonStyle.primary)
    async def button_callback(self, button, interaction):
        button.disabled = True
        button.label = 'No more pressing!'
        await interaction.response.edit_message(view=self)
        
	view = MyView()
	@bot.command()
	async def button(ctx):
    	await ctx.send('Press the button!', view=view)
	```

=== "Disabling all buttons"
	```py hl_lines="4-7 10-13"
	class MyView(discord.ui.View):
            @discord.ui.button(emoji='😀', label="Button 1", style=discord.ButtonStyle.primary)
            async def button_callback(self, button, interaction):
                for child in self.children:
                    child.disabled = True
                button.label = 'No more pressing either button!'
                await interaction.response.edit_message(view=self)
            @discord.ui.button(label="Button 2", style=discord.ButtonStyle.primary)
            async def second_button_callback(self, button, interaction):
                for child in self.children:
                    child.disabled = True
                button.label = 'No more pressing either button!'
                await interaction.response.edit_message(view=self)
            
	@bot.command()
	async def button(ctx):
    	await ctx.send(content="Press the button!", view=MyView())
	```

