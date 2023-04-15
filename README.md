# Aquatope
Scholar project | Main code

# Modules 
* pyttsx3
* SpeechRecognition
* pyaudio
* rustylib (by ZeyaTsu)

# Terminal, desktop-app

## pyttsx3
```py
engine = pyttsx3.init()

fr_voice = moteur.getProperty('voices')[0]
engine.setProperty('voice', fr_voice.id)

sentence = ""


engine.say(phrase)
engine.runAndWait()
```

```py
import speech_recognition as sr
import pyttsx3

reco = sr.Recognizer()

while True:
    try:
        with sr.Microphone() as mic:
            reco.adjust_for_ambient_noise(mic, duration=0.2)
            audio = reco.listen(mic)

            text = reco.recognize_google(audio, language="fr-FR")
            text = text.lower()

            print(f"{text}")

    except Exception as e:
        print(e)
```

# Discord

```py
import nextcord
import os
import speech_recognition as sr
from dotenv import load_dotenv
from nextcord.ext import commands

load_dotenv()
TOKEN = os.getenv('DISCORD_TOKEN')
GUILD = os.getenv('DISCORD_GUILD')

bot = commands.Bot(command_prefix='!')

@bot.command(name='join', help='Make the bot join the voice channel')
async def join(ctx):
    if ctx.author.voice is None:
        await ctx.send("You are not in a voice channel.")
        return

    channel = ctx.author.voice.channel
    await channel.connect()

@bot.command(name='leave', help='Make the bot leave the voice channel')
async def leave(ctx):
    voice = nextcord.utils.get(bot.voice_clients, guild=ctx.guild)
    if voice is not None:
        await voice.disconnect()

@bot.command(name='listen', help='Listen to voice channel and transcribe speech')
async def listen(ctx):
    if not ctx.author.voice:
        await ctx.send('You are not in a voice channel.')
        return

    # Get the voice client for the bot's current server
    voice_client = nextcord.utils.get(bot.voice_clients, guild=ctx.guild)

    # Check if the bot is connected to a voice channel
    if not voice_client:
        await ctx.send('Bot is not connected to a voice channel.')
        return

    # Check if the bot is already listening to a voice channel
    if voice_client.is_playing():
        await ctx.send('Bot is already listening to a voice channel.')
        return

    # Start listening to the voice channel
    voice_client.listen(discord.VoiceChannel)

    # Create a speech recognition object
    recognizer = sr.Recognizer()

    # Loop to continuously transcribe audio from the voice channel
    while voice_client.is_connected() and voice_client.is_playing():
        try:
            # Get audio from the voice channel
            audio = voice_client.listen(discord.VoiceChannel)

            # Transcribe the audio using Google Speech Recognition
            text = recognizer.recognize_google(audio)

            # Output the transcribed text to the console
            print(f'Transcription: {text}')

            # Send the transcribed text to the Discord chat
            await ctx.send(f'Transcription: {text}')

        except sr.UnknownValueError:
            print('Speech recognition could not understand audio.')
        except sr.RequestError as e:
            print(f'Request error: {e}')

    await ctx.send('Stopped listening to voice channel.')

bot.run(TOKEN)
```
