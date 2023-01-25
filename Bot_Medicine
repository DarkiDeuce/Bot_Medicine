import aiogram
import requests
import asyncio
import time
from bs4 import BeautifulSoup
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.common.action_chains import ActionChains


from aiogram import Dispatcher, Bot, types, executor
from aiogram.types import ReplyKeyboardRemove, ReplyKeyboardMarkup, KeyboardButton, InlineKeyboardButton, InlineKeyboardMarkup, CallbackQuery, InputMedia
from aiogram.dispatcher.filters.state import StatesGroup, State
from aiogram.dispatcher import FSMContext
from aiogram.contrib.fsm_storage.memory import MemoryStorage

Token_bot = ' '

loop = asyncio.get_event_loop()

bot = Bot(token=Token_bot)
dp = Dispatcher(bot, loop=loop, storage=MemoryStorage())
coordinates = [0, 0]

class Form(StatesGroup):
    search_methood = State()
    location = State()
    name_medecine = State()


def pars_pharmacy(name_medicine, coordinates):
    global all_medicine
    all_medicine = []
    link_Medicine = []
    url = 'https://aptekamos.ru/'
    s = Service('C:/Users/User/Desktop/All/Activity/Python/Задачи/chromedriver.exe')

    try:
        driver = webdriver.Chrome(service=s)
        driver.get(url=url)
        driver.set_window_size(1920, 1080)
        time.sleep(2)

        region_list = driver.find_element(By.XPATH, '//*[@id="h-regions"]').click()
        time.sleep(2)

        Moscow = driver.find_element(By.XPATH, '//div[text()="Московский регион"]').click()
        time.sleep(2)

        input_Medicine = driver.find_element(By.CLASS_NAME, 'text-field')
        input_Medicine.clear()
        input_Medicine.send_keys(name_medicine)
        time.sleep(2)

        try:
            Medicine = driver.find_element(By.CLASS_NAME, 'omnibox-item-name').click()
            time.sleep(2)

        except:
            print("В нашей базе нет такого лекарства. Попробуйте ещё раз, возможно вы ввели название не правильно.")

        response_search_result = driver.page_source

    finally:
        driver.close()
        driver.quit()

    soup_search_result = BeautifulSoup(response_search_result, 'lxml')

    card_position = soup_search_result.find('div', id='products').find_all('div', 'product flex-df')

    for i in card_position:
        try:
            link_Medicine.append(i.find('a', class_='am-button-1').get('href'))
            all_medicine.append(i.find('div', 'product-name').text)
        except:
            continue

    return dict(zip(all_medicine, link_Medicine))

def pars_addres(url):
    global user_data, coordinates, list_address, list_link, list_name_address, list_cost
    list_link = []
    list_address = []
    list_name_address = []
    list_cost = []
    if user_data.get('search_methood') == '💰 Просто дешевле':
        s = Service('C:/Users/User/Desktop/All/Activity/Python/Задачи/chromedriver.exe')
        driver = webdriver.Chrome(service=s)
        Map_coordinates = dict({
            "latitude": coordinates[0],
            "longitude": coordinates[1],
            "accuracy": 100
        })
        driver.execute_cdp_cmd("Emulation.setGeolocationOverride", Map_coordinates)

        try:
            driver.get(url=url)
            driver.set_window_size(1920, 1080)
            time.sleep(2)

            city = driver.find_element(By.CLASS_NAME, 'ret-city-flt-img').click()
            time.sleep(2)

            input_city = driver.find_element(By.XPATH, '//*[@id="dialog-search-field"]')
            input_city.clear()
            input_city.send_keys("Москва")
            time.sleep(2)

            choice_city = driver.find_element(By.XPATH, '//span[text()="Москва"]').click()
            time.sleep(2)

            apply = driver.find_element(By.ID, 'dialog-apply-btn').click()
            time.sleep(2)

            mobile_version = driver.find_element(By.XPATH, '//a[text()="Мобильная версия"]').click()
            time.sleep(2)

            cheap = driver.find_element(By.XPATH, '//*[@id="ama-cheap-sort"]').click()
            time.sleep(2)

            requests = driver.page_source

        finally:
            driver.close()
            driver.quit()

        soup = BeautifulSoup(requests, 'lxml')
        card = soup.find_all('div', class_='ama-org-c')

        for i in card:
            try:
                list_name_address.append(i.find('a', 'ama-org-name').text)
                list_link.append(i.find('a', 'ama-org-name').get('href'))
                list_address.append(i.find('div', 'ama-org-addr').text)
                list_cost.append(i.find('span', 'ama-org-minp').text)
            except:
                continue

        return dict(zip(list_address, list_link))
    else:
        s = Service('C:/Users/User/Desktop/All/Activity/Python/Задачи/chromedriver.exe')
        driver = webdriver.Chrome(service=s)
        Map_coordinates = dict({
            "latitude": coordinates[0],
            "longitude": coordinates[1],
            "accuracy": 100
        })
        driver.execute_cdp_cmd("Emulation.setGeolocationOverride", Map_coordinates)

        try:
            driver.get(url=url)
            driver.set_window_size(1920, 1080)
            time.sleep(2)

            city = driver.find_element(By.CLASS_NAME, 'ret-city-flt-img').click()
            time.sleep(2)

            input_city = driver.find_element(By.XPATH, '//*[@id="dialog-search-field"]')
            input_city.clear()
            input_city.send_keys("Москва")
            time.sleep(2)

            choice_city = driver.find_element(By.XPATH, '//span[text()="Москва"]').click()
            time.sleep(2)

            apply = driver.find_element(By.ID, 'dialog-apply-btn').click()
            time.sleep(2)

            mobile_version = driver.find_element(By.XPATH, '//a[text()="Мобильная версия"]').click()
            time.sleep(2)

            nearer = driver.find_element(By.XPATH, '//*[@id="ama-near-sort"]/div').click()
            time.sleep(2)

            requests = driver.page_source

        finally:
            driver.close()
            driver.quit()

        soup = BeautifulSoup(requests, 'lxml')
        card = soup.find_all('div', class_='ama-org-c')

        for i in card:
            try:
                list_name_address.append(i.find('a', 'ama-org-name').text)
                list_link.append(i.find('a', 'ama-org-name').get('href'))
                list_address.append(i.find('div', 'ama-org-addr').text)
                list_cost.append(i.find('span', 'ama-org-minp').text)

            except:
                continue

        return dict(zip(list_address, list_link))

@dp.message_handler(commands=['start'])
async def start(message: types.Message):
    markup = ReplyKeyboardMarkup(row_width=1, resize_keyboard=True)
    GEO = KeyboardButton('📍По геопозиции')
    Point_on_map = KeyboardButton('🗺️ Точка на карте')
    Cheaper = KeyboardButton('💰 Просто дешевле')
    markup.add(GEO, Point_on_map, Cheaper)

    await bot.send_message(message.chat.id, 'Как будем искать лекарство?', reply_markup=markup)
    await Form.search_methood.set()

@dp.message_handler(state=Form.search_methood)
async def search_methood(message: types.Message, state: FSMContext):
    global user_data
    await state.update_data(search_methood=message.text)

    user_data = await state.get_data()

    if user_data.get('search_methood') == '📍По геопозиции':
        await state.finish()

        markup = ReplyKeyboardMarkup(row_width=1, resize_keyboard=True)
        GEO = KeyboardButton('Отправить местоположение', request_location=True)
        markup.add(GEO)

        await bot.send_message(message.chat.id, 'Отправьте Ваше местоположение', reply_markup=markup)

    elif user_data.get('search_methood') == '🗺️ Точка на карте':
        await state.finish()

        await bot.send_message(message.chat.id, '''Для отправки точки на карте:
1. Нажмите на кнопку с изображением скрепки, расположенную рядом с окном ввода сообщения
2. В нижнем меню выберете "Геопозиция"
3. Укажите место, вокруг которого будет производить поиск и нажмите кнопку "Отправить геопозицию."''', reply_markup=ReplyKeyboardRemove())

    elif user_data.get('search_methood') == '💰 Просто дешевле':
        await bot.send_message(message.chat.id, 'Введите название лекарства, которое хотите найти',
                               reply_markup=ReplyKeyboardRemove())

        await Form.name_medecine.set()

    else:
        await bot.send_message(message.chat.id, 'У меня нет такого способа поиска. Выберете один из вариантов.')

@dp.message_handler(state=Form.name_medecine)
async def name_medecine(message: types.Message, state: FSMContext):
    global coordinates, complite_result, number_position, all_medicine
    await state.update_data(name_medecine=message.text)

    user_data = await state.get_data()

    await state.finish()

    await bot.send_message(message.chat.id, 'Ожидайте. Собираю информацию.')

    complite_result = pars_pharmacy(user_data.get('name_medecine'), coordinates)

    number_position = 0

    markup = InlineKeyboardMarkup(row_width=3)
    back = InlineKeyboardButton('Предыдущее лекарство', callback_data='back_address')
    position = InlineKeyboardButton(f'{number_position + 1}/{len(all_medicine)}', callback_data=' ')
    next = InlineKeyboardButton('Следующее лекарство', callback_data='next_address')
    choice = InlineKeyboardButton('Показать адреса с этим лекарством', callback_data=f'Medicine_{number_position}')
    MM = InlineKeyboardButton('Вернуться в начало', callback_data='menu')
    markup.add(back, position, next)
    markup.row_width = 1
    markup.add(choice, MM)

    text = f'''Выберите наименование из предложенных:\n
{all_medicine[number_position]}'''

    await bot.send_message(message.chat.id, text, reply_markup=markup)

@dp.message_handler(content_types=["location"])
async def location(message: types.Message):
    global coordinates

    coordinates = []

    if message.location is not None:
        coordinates = [message.location.latitude, message.location.longitude]

        await bot.send_message(message.chat.id, 'Введите название лекарства, которое хотите найти.',
                               reply_markup=ReplyKeyboardRemove())
        await Form.name_medecine.set()

@dp.callback_query_handler(lambda call: True)
async def call_back(call: CallbackQuery):
    global all_medicine, complite_result, list_address, list_link, list_name_address, list_cost, address_and_link, number_position, list_name_address

    if call.data.startswith('Medicine'):
        await call.message.edit_text("Потребуется ещё немного времени. Ожидайте.")

        index_position = call.data.split('_')
        name_medecine_from_list = all_medicine[int(index_position[1])]
        url = complite_result.get(name_medecine_from_list)
        address_and_link = pars_addres(url)

        number_position = 0
        markup = InlineKeyboardMarkup(row_width=3)
        back = InlineKeyboardButton('Предыдущая аптека', callback_data='back')
        position = InlineKeyboardButton(f'{number_position+1}/{len(list_name_address)}', callback_data=' ')
        next = InlineKeyboardButton('Следующая аптека', callback_data='next')
        MM = InlineKeyboardButton('Вернуться в начало', callback_data='menu')
        markup.add(back, position, next, MM)

        text = f'''Местоположение аптеки: {list_name_address[number_position]}\n
Точный адрес: {list_address[number_position]}\n
Цена при заказе на АптекаМос: {list_cost[number_position]}\n
Ссылка для заказа: {list_link[number_position]}'''

        await bot.send_message(call.message.chat.id, text, reply_markup=markup)

    elif call.data == 'back':
        number_position -= 1

        if number_position == -1:
            number_position = len(list_name_address) - 1

        markup = InlineKeyboardMarkup(row_width=3)
        back = InlineKeyboardButton('Предыдущая аптека', callback_data='back')
        position = InlineKeyboardButton(f'{number_position+1}/{len(list_name_address)}', callback_data=' ')
        next = InlineKeyboardButton('Следующая аптека', callback_data='next')
        MM = InlineKeyboardButton('Вернуться в начало', callback_data='menu')
        markup.add(back, position, next, MM)

        text = f'''Местоположение аптеки: {list_name_address[number_position]}\n
Точный адрес: {list_address[number_position]}\n
Цена при заказе на АптекаМос: {list_cost[number_position]}\n
Ссылка для заказа: {list_link[0]}'''

        await bot.edit_message_text(chat_id=call.message.chat.id,
                                    message_id=call.message.message_id,
                                    text=text,
                                    inline_message_id=call.inline_message_id,
                                    reply_markup=markup)

    elif call.data == 'next':
        number_position += 1

        if number_position == len(list_name_address):
            number_position = 0

        markup = InlineKeyboardMarkup(row_width=3)
        back = InlineKeyboardButton('Предыдущая аптека', callback_data='back')
        position = InlineKeyboardButton(f'{number_position+1}/{len(list_name_address)}', callback_data=' ')
        next = InlineKeyboardButton('Следующая аптека', callback_data='next')
        MM = InlineKeyboardButton('Вернуться в начало', callback_data='menu')
        markup.add(back, position, next, MM)

        text = f'''Местоположение аптеки: {list_name_address[number_position]}\n
Точный адрес: {list_address[number_position]}\n
Цена при заказе на АптекаМос: {list_cost[number_position]}\n
Ссылка для заказа: {list_link[0]}'''

        await bot.edit_message_text(chat_id=call.message.chat.id,
                                    message_id=call.message.message_id,
                                    text=text,
                                    inline_message_id=call.inline_message_id,
                                    reply_markup=markup)

    elif call.data == 'back_address':
        number_position -= 1

        if number_position == -1:
            number_position = len(all_medicine) - 1

        markup = InlineKeyboardMarkup(row_width=3)
        back = InlineKeyboardButton('Предыдущее лекарство', callback_data='back_address')
        position = InlineKeyboardButton(f'{number_position + 1}/{len(all_medicine)}', callback_data=' ')
        next = InlineKeyboardButton('Следующее лекарство', callback_data='next_address')
        choice = InlineKeyboardButton('Показать адреса с этим лекарством', callback_data=f'Medicine_{number_position}')
        MM = InlineKeyboardButton('Вернуться в начало', callback_data='menu')
        markup.add(back, position, next)
        markup.row_width = 1
        markup.add(choice, MM)

        text = f'''Выберите наименование из предложенных:\n
{all_medicine[number_position]}'''

        await bot.edit_message_text(chat_id=call.message.chat.id,
                                    message_id=call.message.message_id,
                                    text=text,
                                    inline_message_id=call.inline_message_id,
                                    reply_markup=markup)

    elif call.data == 'next_address':
        number_position += 1

        if number_position == len(all_medicine):
            number_position = 0

        markup = InlineKeyboardMarkup(row_width=3)
        back = InlineKeyboardButton('Предыдущее лекарство', callback_data='back_address')
        position = InlineKeyboardButton(f'{number_position + 1}/{len(all_medicine)}', callback_data=' ')
        next = InlineKeyboardButton('Следующее лекарство', callback_data='next_address')
        choice = InlineKeyboardButton('Показать адреса с этим лекарством', callback_data=f'Medicine_{number_position}')
        MM = InlineKeyboardButton('Вернуться в начало', callback_data='menu')
        markup.add(back, position, next)
        markup.row_width = 1
        markup.add(choice, MM)

        text = f'''Выберите наименование из предложенных:\n
{all_medicine[number_position]}'''

        await bot.edit_message_text(chat_id=call.message.chat.id,
                                    message_id=call.message.message_id,
                                    text=text,
                                    inline_message_id=call.inline_message_id,
                                    reply_markup=markup)

    elif call.data == 'menu':
        markup = ReplyKeyboardMarkup(row_width=1, resize_keyboard=True)
        GEO = KeyboardButton('📍По геопозиции')
        Point_on_map = KeyboardButton('🗺️ Точка на карте')
        Cheaper = KeyboardButton('💰 Просто дешевле')
        markup.add(GEO, Point_on_map, Cheaper)

        await bot.send_message(call.message.chat.id, 'Как будем искать лекарство?', reply_markup=markup)
        await Form.search_methood.set()

if __name__ == '__main__':
    executor.start_polling(dp, skip_updates=True)
