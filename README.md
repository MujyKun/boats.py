# boats.py
The official Python Library for the [discord.boats](https://discord.boats) API

# Installation
## Install via pip (Recommended)
```pip install boats.py```
## Install from source
```
git clone https://github.com/DiscordBoats/boats.py
cd boats.py
python setup.py install
```

# Examples
## Post stats
With Tasks (Must be using discord.py version 1.1.0+):
```python
import discord, discordboats
from discord.ext import tasks

class Discord_Boats(commands.Cog):
    """Interacts with the discord.boats API"""

    def __init__(self, bot):
        self.bot = bot
        self.update_stats.start()
        dbpy = discordboats.Client("Token", loop=client.loop) # Token obtained from discord.boats

    def cog_unload(self):
        self.update_stats.cancel()

    @tasks.loop(minutes=30.0)
    async def update_stats(self):
        """This automatically updates your server count to discord.boats every 30 minutes."""
        try:
            await dbpy.post_stats(self.bot.user.id, len(self.bot.guilds))
        except Exception as e:
            print(f'Failed to post server count to discord.boats\n{type(e).__name__}: {e}')

def setup(bot):
    bot.add_cog(Discord_Boats(bot))
```
Without Tasks:
```python
import discord, discordboats, asyncio

class Discord_Boats(commands.Cog):
    """Interacts with the discord.boats API"""

    def __init__(self, bot):
        self.bot = bot
        dbpy = discordboats.Client("Token", loop=client.loop) # Token obtained from discord.boats
        
    async def update_stats(self):
        """This automatically updates your server count to discord.boats every 30 minutes."""
        while True:
            try:
                await dbpy.post_stats(self.bot.user.id, len(self.bot.guilds))
            except Exception as e:
                print(f'Failed to post server count to discord.boats\n{type(e).__name__}: {e}')
            await asyncio.sleep(1800)

def setup(bot):
    bot.loop.create_task(update_stats())
    bot.add_cog(Discord_Boats(bot))
```
