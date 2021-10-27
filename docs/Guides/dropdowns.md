---
description: Learn how to create and use dropdowns in your Discord bot with the Pycord library!
authors: Pycord Guide
---

# Dropdowns
	
[Read the docs](https://pycord.readthedocs.io/en/master/api.html?highlight=dropdown#discord.SelectMenu)

![image](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-MjPu-g4A9to7TurG_WS%2F-MjPvJpnISLfsjh4oqrH%2Fimage.png?alt=media&token=c7aae200-15e3-4539-88c5-34d45d0b193a)

Min Values: The Minimum number of values the user must select. A user can select multiple values in a select menu. If you want the user to be able to select only one option, set both min values and max values to 1.

Max Values: The Minimum number of values the user can select. A user can select multiple values in a select menu. If you want the user to be able to select only one option, set both min values and max values to 1.

![image](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-MjPu-g4A9to7TurG_WS%2F-MjPxAcb3EltghtINizS%2Fimage.png?alt=media&token=48f15d97-1c4a-4408-bbbc-882a1a2f0495)

Placeholder: The text to be displayed when no option is selected.

![image](https://gblobscdn.gitbook.com/assets%2F-MjPk-Yu4sOq8KGrr_yG%2F-MjPu-g4A9to7TurG_WS%2F-MjPxacwrtNRvJ0oZqus%2Fimage.png?alt=media&token=fe498473-07dd-496f-b922-06fcf6f01eaf)

## Usage
### Subclassing discord.ui.View
```py
class MyView(discord.ui.View):
    @discord.ui.select(placeholder='Pick your colour', min_values=1, max_values=1, options=[
        discord.SelectOption(label='Red', description='Your favourite colour is red', emoji='🟥'),
        discord.SelectOption(label='Green', description='Your favourite colour is green', emoji='🟩'),
        discord.SelectOption(label='Blue', description='Your favourite colour is blue', emoji='🟦')
    ])
    async def select_callback(self, select, interaction):
        await interaction.response.send_message(f'Your favourite colour is {select.values[0]}', ephemeral=True)
    
@bot.command()
async def my_select(ctx):
    await ctx.send('What is your favourite colour?', view=MyView(timeout=0))
```

### Subclassing discord.ui.Select
```py
class MySelect(discord.ui.Select):
    def __init__(self):
        options = [
            discord.SelectOption(label='Red', description='Your favourite colour is red', emoji='🟥'),
            discord.SelectOption(label='Green', description='Your favourite colour is green', emoji='🟩'),
            discord.SelectOption(label='Blue', description='Your favourite colour is blue', emoji='🟦')
        ]
        super().__init__(placeholder='Pick your colour', min_values=1, max_values=1, options=options)
     
     async def callback(self, interaction):
        await interaction.response.send_message(f'Your favourite colour is {self.values[0]}', ephemeral=True)
        
view = discord.ui.View(timeout=10) # timeout is optional, it can be defined in seconds
view.add_item(MySelect())
@bot.command()
async def my_select(ctx):
    await ctx.send('What is your favourite colour?', view=view)
```

## Action Rows
A message can have up to five "action rows" and each of these "action" rows have five slots where you can put message components. A button takes up one of these slots but a select menus takes up all five slots of a "action row". Keep this in mind when creating your views since you don't want to run out of space!

Action rows are handled internally, and you can pass the `row` kwarg to use them, thr same way you use them with [buttons](/buttons.md).

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
        
    @discord.ui.select(custom_ids="select-1", placeholder='Pick your colour', min_values=1, max_values=1, options=[
        discord.SelectOption(label='Red', description='Your favourite colour is red', emoji='🟥'),
        discord.SelectOption(label='Green', description='Your favourite colour is green', emoji='🟩'),
        discord.SelectOption(label='Blue', description='Your favourite colour is blue', emoji='🟦')
    ])
    async def select_callback(self, select, interaction):
        await interaction.response.send_message(f'Your favorite color is {select.values[0]}', ephemeral=True)
    
@bot.command()
async def my_select(ctx):
    await ctx.send('What is your favorite color?', view=MyView())
```