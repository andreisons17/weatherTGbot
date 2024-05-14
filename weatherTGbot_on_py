import requests
import datetime
import aiogram
from aiogram import Bot, Dispatcher, executor, types

tg_bot_token = ''
open_weather_token = ''

bot = Bot(token = tg_bot_token)
dp = Dispatcher(bot)


@dp.message_handler(commands=['start'])
async def start_command(message: types.Message):
    await message.reply('Привет, напиши ширину и долготу (через пробел, вот так:50 50) и я пришлю тебе сводку погоды!')

@dp.message_handler()
async def get_weather(message: types.Message):
    lat = message.text.split(' ')[0]
    lon = message.text.split(' ')[1]
    try:
        r = requests.get(f'https://api.openweathermap.org/data/2.5/weather?lat={lat}&lon={lon}&appid={open_weather_token}&units=metric')
        data = r.json()
        city = data['name']
        cur_weather = data['main']['temp']
        hunidity = data['main']['humidity']
        pressure = data['main']['pressure']
        wind = data['wind']['speed']
        country = data['sys']['country']
        sunrise_timestanp = datetime.datetime.fromtimestamp(data['sys']['sunrise'])
        sunset_timestanp = datetime.datetime.fromtimestamp(data['sys']['sunset'])
        lenday = datetime.datetime.fromtimestamp(data['sys']['sunset'])-datetime.datetime.fromtimestamp(data['sys']['sunrise'])

        await message.reply(f'Погода на широте: -{lat}- и долготе: -{lon}- :\nТемпература: {cur_weather}\n'
              f'Влажность: {hunidity}\nДавление: {pressure} мм.рт.ст \nВетер: {wind} м/c\nСтрана: {country}\nГород: {city}\nВосход солнца: {sunrise_timestanp}\nЗакат: {sunset_timestanp}\nПродолжительность дня: {lenday}'
              )

    except Exception as ex:
        await message.reply('Проверьте данные')

if __name__ == '__main__':
    executor.start_polling(dp)
