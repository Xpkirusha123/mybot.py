# mybot.py
from aiogram import Bot, Dispatcher, types
from aiogram.utils import executor
from aiogram.types import InlineKeyboardMarkup, InlineKeyboardButton
import logging

TOKEN = 7695157095
ADMIN_ID =   2131346744 Telegram ID
PRICE_USD = 3

bot = Bot(7695157095:AAENDRQ5KN40ojfeLYW33Zs5fAbfY4zDg-k)
dp = Dispatcher(bot)
logging.basicConfig(level=logging.INFO)

pay_kb = InlineKeyboardMarkup(row_width=1).add(
    InlineKeyboardButton("Оплатити через Monobank", url="https://send.monobank.ua/jar/7qcxrYk2bM"),
    InlineKeyboardButton("Оплатити в крипті", url="https://t.me/xp_kirusha")
)

@dp.message_handler(commands=['start'])
async def start(message: types.Message):
    await message.answer(
        "Привіт! Щоб отримати доступ до airdrop-матеріалів, потрібно сплатити 3$. Обери спосіб оплати нижче:",
        reply_markup=pay_kb
    )

@dp.message_handler(commands=['get_access'])
async def get_access(message: types.Message):
    if str(message.from_user.id) in open("users.txt").read():
        await message.answer("Доступ вже відкрито! Ось твій airdrop: https://t.me/airdrop_channel")
    else:
        await message.answer("Ще немає підтвердження оплати. Напиши адміну @xp_kirusha після оплати.")

@dp.message_handler(commands=['confirm'])
async def confirm(message: types.Message):
    if message.from_user.id == ADMIN_ID:
        parts = message.text.split()
        if len(parts) == 2:
            user_id = parts[1]
            with open("users.txt", "a") as f:
                f.write(user_id + "\n")
            await message.answer(f"Користувач {user_id} отримав доступ.")
            await bot.send_message(user_id, "Тобі відкрито доступ до airdrop!")
        else:
            await message.answer("Використання: /confirm <user_id>")
