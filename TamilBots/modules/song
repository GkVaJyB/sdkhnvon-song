from pyrogram import Client, filters
import asyncio
import os
import urllib.request
from pytube import YouTube
from urllib.parse import urlparse
from pyrogram.types import InlineKeyboardMarkup
from pyrogram.types import InlineKeyboardButton
from youtubesearchpython import VideosSearch
from TamilBots.alexa import get_arg
from TamilBots import app, LOGGER
## Extra Fns

def yt_search(song):
    videosSearch = VideosSearch(song, limit=1)
    result = videosSearch.result()
    if not result:
        return False
    else:
        video_id = result["result"][0]["id"]
        url = f"https://www.youtube.com/watch?v={video_id}"
        return url

asanga = "thumb.jpg"
@app.on_message(filters.command("song"))
async def song(client, message):
    user_id = message.from_user["id"]
    args = get_arg(message) + " " + "song"
    if args.startswith(" "):
        await message.reply("Enter a song name. Check /help")
        return ""
    status = await message.reply("**Downloading Song..**")
    video_link = yt_search(args)
    if not video_link:
        await status.edit("✖️ 𝐅𝐨𝐮𝐧𝐝 𝐍𝐨𝐭𝐡𝐢𝐧𝐠. 𝐒𝐨𝐫𝐫𝐲.\n\n𝐓𝐫𝐲 𝐀𝐧𝐨𝐭𝐡𝐞𝐫 𝐊𝐞𝐲𝐰𝐨𝐫𝐤 𝐎𝐫 𝐌𝐚𝐲𝐛𝐞 𝐒𝐩𝐞𝐥𝐥 𝐈𝐭 𝐏𝐫𝐨𝐩𝐞𝐫𝐥𝐲.\n\nEg.`/song Faded`")
        return ""
    yt = YouTube(video_link)
    duration =(yt.length)
    if duration > 3600:
        await status.edit(f"**Songs Longer than** `1 Hour` **are not Allowed**") 
        return ""
    videosd = video_link
    url_data = urlparse(video_link)
    asanga =(url_data.query[2::])
    urllib.request.urlretrieve(f"https://img.youtube.com/vi/{asanga}/mqdefault.jpg", f"{message.message_id}.jpg")
    thambmail =(f"{message.message_id}.jpg")
    audio = yt.streams.filter(only_audio=True).first()
    try:
        download = audio.download(filename=f"{str(yt.title)}")
    except Exception as ex:
        await status.edit("Failed to download song 😶")
        LOGGER.error(ex)
        return ""
    rename = os.rename(download, f"{str(yt.title)}.mp3")
    cap = f"🏷 **Title**: `{str(yt.title)[:40]}`\n🎧 By **AleXa OweNs** "
    await app.send_chat_action(message.chat.id, "upload_audio")
    await app.send_audio(
        chat_id=message.chat.id,
        audio=f"{str(yt.title)}.mp3",
        duration=int(yt.length),
        title=str(yt.title),
        performer=str(yt.author),
        reply_to_message_id=message.message_id,
        thumb=thambmail,
        caption=cap)
    await status.delete()
    os.remove(f"{str(yt.title)}.mp3")
    os.remove(f"{message.message_id}.jpg")
