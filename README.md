import discord
from discord.ext import commands
import os
from AI import get_class
intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix='$', intents=intents)

SAVE_DIR = "images"
os.makedirs(SAVE_DIR, exist_ok=True)

@bot.event
async def on_ready():
    print(f'We have logged in as {bot.user}')

@bot.command()
async def hello(ctx):
    await ctx.send(f'Hi! I am a bot {bot.user}!')

@bot.command()
async def heh(ctx, count_heh = 5):
    await ctx.send("he" * count_heh)

@bot.command(name="saveimg")
async def save_image(ctx):
    if not ctx.message.attachments:
        await ctx.send("Tidak ada gambar yang dikirim.")
        return

    for attachment in ctx.message.attachments:
        if attachment.filename.lower().endswith(('.png', '.jpg', '.jpeg', '.gif', '.bmp')):
            save_path = os.path.join(SAVE_DIR, attachment.filename)
            await attachment.save(save_path)

            result = get_class(save_path)

            if result == "Gambar_Telur.jpg":
                await ctx.end("ini gambar telur")
            elif result == "Gambar_Sapi.jpg":
                await ctx.end("ini gambar sapi")
            elif result == "Gambar_Tahu.jpg":
                await ctx.end("ini gambar tahu") 
    #tiga gambar dulu

            await ctx.send(f"Gambar disimpan: `{attachment.filename}`")
        else:
            await ctx.send(f"{attachment.filename} bukan file gambar yang didukung.")

bot.run("MTM3NTc3MTU2MTA3NTY3MTEzMg.GP21uT.ptDbMB_V_7kUmE10RGPKY8mno9howC6IxQ3N3c")
