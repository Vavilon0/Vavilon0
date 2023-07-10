import telebot
from telebot import types
import random
import sqlite3
from rand import rand
from mem import mem


TOKEN = ""
bot = telebot.TeleBot(TOKEN, parse_mode=None)


conn = sqlite3.connect('db.db', check_same_thread=False)
cursor = conn.cursor()

def db_table_val(user_id: int, user_name: str, user_surname: str, username: str):
	cursor.execute('INSERT INTO users (user_id, user_name, user_surname, username) VALUES (?, ?, ?, ?)', (user_id, user_name, user_surname, username,))
	conn.commit()


@bot.message_handler(commands=['start'])
def start(message):

    markup = types.ReplyKeyboardMarkup(row_width=2, resize_keyboard=True)
    btn1 = types.KeyboardButton('🔍Поиск по коду')
    btn2 = types.KeyboardButton('🎲Рандом🎲')
    btn4 = types.KeyboardButton('📈Соц-Сети📉')
    btn3 = types.KeyboardButton('💢Другое💢')
    btn5 = types.KeyboardButton('➡Скрить меню⬅')
    markup.add(btn1, btn2,btn3,btn4, btn5)
    bot.send_message(chat_id=message.chat.id, text='✋Привет {0.first_name}\n\nВыбери кнопку и нажми ее!!\nПриятного просмотра фильма или аниме мой друг.'.format(message.from_user), reply_markup=markup)

    us_id = message.from_user.id
    cursor.execute("SELECT user_id FROM users WHERE user_id = ?", (us_id, ))
    data = cursor.fetchone()
    if data is None:
        if message.text.lower() == '/start':
            us_id = message.from_user.id
            us_name = message.from_user.first_name
            us_sname = message.from_user.last_name
            username = message.from_user.username
      
            db_table_val(user_id=us_id, user_name=us_name, user_surname=us_sname, username=username)


@bot.message_handler(content_types=['text'])
def text(message):
    if message.text == '🔍Поиск по коду':
        markup = types.ReplyKeyboardMarkup(row_width=2, resize_keyboard=True)
        btn1 = types.KeyboardButton('☣Главное меню☣')
        markup.add(btn1,)
        bot.send_message(chat_id=message.chat.id, text='❤Веди номер фильма или аниmе❤', reply_markup=markup)
    elif message.text == '🎲Рандом🎲':
            bot.send_message(message.chat.id, '' + random.choice(rand))
    elif message.text == ('😍Поддержать создателя денюшкой😍'):
        markup = types.InlineKeyboardMarkup()
        btn2 = types.InlineKeyboardButton("Закинуть денюшку💲", url="")
        markup.add(btn2 )
        bot.send_message(chat_id=message.chat.id, text='Этим вы поможете создателю и попадете в таблицу данатеров.\nВ коментарии к донату напишите свой ник в телеграмме.', reply_markup=markup)
    elif message.text == ('📈Соц-Сети📉'):
        markup = types.InlineKeyboardMarkup()
        btn1 = types.InlineKeyboardButton('👨‍💻Тик-Ток👨‍💻', url="")
        btn2 = types.InlineKeyboardButton('👯‍♀️Группа👯‍♀️', url="")
        btn3 = types.InlineKeyboardButton('💤Чат💤', url="")
        markup.add(btn1,btn2,btn3)
        bot.send_message(chat_id=message.chat.id, text='Друг, я буду ждать тебя во всех соц-сетях👄', reply_markup=markup)
    elif message.text == ('💢Другое💢'):
        markup = types.ReplyKeyboardMarkup(row_width=2, resize_keyboard=True)
        btn1 = types.KeyboardButton('😅Мемы😅')
        btn2 = types.KeyboardButton('😎Донатеры😎')
        btn3 = types.KeyboardButton('😍Поддержать создателя денюшкой😍')
        btn4 = types.KeyboardButton('☣Главное меню☣')
        btn5 = types.KeyboardButton('➡Скрить меню⬅')
        markup.add(btn1, btn2,btn3,btn4, btn5)
        bot.send_message(chat_id=message.chat.id, text='Выбери пункт действия', reply_markup=markup)
    elif message.text == ('😎Донатеры😎'):
        bot.send_message(chat_id=message.chat.id, text='Тут будут все , кто поможет денюшкой💲💲')
    elif message.text == ('😅Мемы😅'):
        bot.send_message(message.chat.id, '' + random.choice(mem))
    elif message.text == ('➡Скрить меню⬅'):
        markup = types.ReplyKeyboardRemove()
        bot.send_message(chat_id=message.chat.id, text='Меню скрыто ', reply_markup=markup)
    elif message.text == '☣Главное меню☣':
        markup = types.ReplyKeyboardMarkup(row_width=2, resize_keyboard=True)
        btn1 = types.KeyboardButton('🔍Поиск по коду')
        btn2 = types.KeyboardButton('🎲Рандом🎲')
        btn4 = types.KeyboardButton('📈Соц-Сети📉')
        btn3 = types.KeyboardButton('💢Другое💢')
        btn5 = types.KeyboardButton('➡Скрить меню⬅')
        markup.add(btn1,btn2,btn3,btn4,btn5)
        bot.send_message(chat_id=message.chat.id, text='✋Привет {0.first_name}\n\nВыбери кнопку и нажми ее!!\nПриятного просмотра фильма или аниме мой друг.'.format(message.from_user), reply_markup=markup)


    elif message.text == '1':
        markup = types.InlineKeyboardMarkup()
        bb1 = types.InlineKeyboardButton("Смотреть фильм/аниме", url="")
        markup.add(bb1)
        bot.send_message(chat_id=message.chat.id, text='' , reply_markup=markup)
    elif message.text == '2':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '3':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '4':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text=' ', reply_markup=markup)
    elif message.text == '5':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '6':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '7':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=''))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '8':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '9':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '10':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '11':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='Криминальное чтиво (1994)\n\n\nНесколько связанных историй из жизни бандитов. Шедевр Квентина Тарантино, который изменил мировое кино.Двое бандитов Винсент Вега и Джулс Винфилд ведут философские беседы в перерывах между разборками и решением проблем с должниками криминального босса Марселласа Уоллеса.В первой истории Винсент проводит незабываемый вечер с женой Марселласа Мией. Во второй рассказывается о боксёре Бутче Кулидже, купленном Уоллесом, чтобы сдать бой. В третьей истории Винсент и Джулс по нелепой случайности попадают в неприятности.', reply_markup=markup)
    elif message.text == '12':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url="https://shre.su/QXME"))
        bot.send_message(chat_id=message.chat.id, text='Иван Васильевич меняет профессию (1973)\n\n\nИван Грозный отвечает на телефон, пока его тезка-пенсионер сидит на троне. Советский хит о липовом государе.Инженер-изобретатель Тимофеев сконструировал машину времени, которая соединила его квартиру с далеким шестнадцатым веком - точнее, с палатами государя Ивана Грозного. Туда-то и попадают тезка царя пенсионер-общественник Иван Васильевич Бунша и квартирный вор Жорж Милославский.На их место в двадцатом веке «переселяется» великий государь. Поломка машины приводит ко множеству неожиданных и забавных событий...', reply_markup=markup)
    elif message.text == '13':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url="https://shre.su/YH8X"))
        bot.send_message(chat_id=message.chat.id, text='1+1 (2011)\n\n\nПострадав в результате несчастного случая, богатый аристократ Филипп нанимает в помощники человека, который менее всего подходит для этой работы, – молодого жителя предместья Дрисса, только что освободившегося из тюрьмы. Несмотря на то, что Филипп прикован к инвалидному креслу, Дриссу удается привнести в размеренную жизнь аристократа дух приключений.', reply_markup=markup)
    elif message.text == '14':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url="https://shre.su/KEFX"))
        bot.send_message(chat_id=message.chat.id, text='Король Лев (1994)\n\n\nУ величественного Короля-Льва Муфасы рождается наследник по имени Симба. Уже в детстве любознательный малыш становится жертвой интриг своего завистливого дяди Шрама, мечтающего о власти.Симба познаёт горе утраты, предательство и изгнание, но в конце концов обретает верных друзей и находит любимую. Закалённый испытаниями, он в нелёгкой борьбе отвоёвывает своё законное место в «Круге жизни», осознав, что значит быть настоящим Королём. ', reply_markup=markup)
    elif message.text == '15':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url="https://shre.su/T8HU"))
        bot.send_message(chat_id=message.chat.id, text='Темный рыцарь (2008)\n\n\n У Бэтмена появляется новый враг — философ-террорист Джокер. Кинокомикс, который вывел жанр на новый уровень.Бэтмен поднимает ставки в войне с криминалом. С помощью лейтенанта Джима Гордона и прокурора Харви Дента он намерен очистить улицы Готэма от преступности. Сотрудничество оказывается эффективным, но скоро они обнаружат себя посреди хаоса, развязанного восходящим криминальным гением, известным напуганным горожанам под именем Джокер.', reply_markup=markup)
    elif message.text == '16':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url="https://shre.su/HNJU"))
        bot.send_message(chat_id=message.chat.id, text='Бойцовский клуб (1999)\n\n\n Страховой работник разрушает рутину своей благополучной жизни. Культовая драма по книге Чака Паланика.Сотрудник страховой компании страдает хронической бессонницей и отчаянно пытается вырваться из мучительно скучной жизни. Однажды в очередной командировке он встречает некоего Тайлера Дёрдена — харизматического торговца мылом с извращенной философией. Тайлер уверен, что самосовершенствование — удел слабых, а единственное, ради чего стоит жить, — саморазрушение.Проходит немного времени, и вот уже новые друзья лупят друг друга почем зря на стоянке перед баром, и очищающий мордобой доставляет им высшее блаженство. Приобщая других мужчин к простым радостям физической жестокости, они основывают тайный Бойцовский клуб, который начинает пользоваться невероятной популярностью.', reply_markup=markup)
    elif message.text == '17':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url="https://shre.su/ARI5"))
        bot.send_message(chat_id=message.chat.id, text='Атака титанов (7 апреля 2013) \n\n\n Последние 100 лет люди живут в страхе и однажды случилось то, чего многие боялись. Вместо прогресса, часть человечества оказалась отброшена назад, что породило новую ветвь в эволюции, которых стали называть титанами. Они не обладали большим интеллектом, но вырастали до гигантских размеров и сила, с которой они рождались, была невероятно огромной и с лихвой компенсировала недостаток ума. Эти существа пожирали людей ради забавы, а самое высокотехнологичное оружие XIX века, которым пытались с ними бороться, оказалось абсолютно бессильно. Те, кому удалось выжить, перепробовали множество методов борьбы, и вскоре, решили возвести высокие и крепкие стены, назвав их Марией, Розой и Сина, через которые не в силах было пройти даже морскому урагану.С того времени прошел целый век. Люди мирно жили под защитой возведенного строения, пока не догадываясь, что их спокойствию вскоре настанет конец. Супертитан, ростом выше стены, набрел на неё и сумел разрушить. Свидетелями этого момента стал мальчик Эрен и его приемная сестра Микаса. Дети с ужасом наблюдали за тем, как монстры хлынули потоком сквозь образовавшуюся расщелину и принялись крушить все вокруг. Их мать погибла, а отец бесследно пропал. А, Эрен, видевший жестокость, с какой титаны пожирали его близких и родственников, поклялся, что отомстит. Захватывающая интрига заставит зрителей смотреть онлайн «Атака титанов» затаив дыхание на протяжении всего аниме сериала.', reply_markup=markup)
    elif message.text == '18':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url="https://shre.su/ETW9"))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '19':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '20':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '21':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '22':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '23':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '24':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '25':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '26':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '27':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '28':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '29':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '30':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '31':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '32':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url="'"))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '33':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '34':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '35':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '36':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '37':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '38':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '39':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '40':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '41':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '42':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '43':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '44':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '45':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '46':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '47':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '48':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '49':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '50':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '50':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='', reply_markup=markup)
    elif message.text == '51':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '52':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '53':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '54':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '55':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '56':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '57':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '58':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '59':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '60':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '61':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '62':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '63':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '64':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '65':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '66':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '67':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '68':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '69':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '70':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '71':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '72':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '73':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '74':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '75':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '76':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '77':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '78':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '79':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '80':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '81':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '82':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '83':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '84':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '85':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '86':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '87':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '88':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '89':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '90':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '91':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '92':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '93':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '94':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '95':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '96':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '97':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '98':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '99':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)
    elif message.text == '100':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton("Смотреть фильм/аниме", url=""))
        bot.send_message(chat_id=message.chat.id, text='\n\n\n', reply_markup=markup)



    elif message.text == 'рассылка':
        if message.chat.id == 1428212393:
            cursor.execute('SELECT user_id FROM users')
            result = cursor.fetchall()
            msg = 'тут будет ваша реклама =) ' 
            for x in result:
                bot.send_message(x[0], str(msg))
    else:
        markup = types.ReplyKeyboardMarkup(row_width=2, resize_keyboard=True)
        btn1 = types.KeyboardButton('🔍Поиск по коду')
        btn2 = types.KeyboardButton('🎲Рандом🎲')
        btn4 = types.KeyboardButton('📈Соц-Сети📉')
        btn3 = types.KeyboardButton('💢Другое💢')
        btn5 = types.KeyboardButton('➡Скрить меню⬅')
        markup.add(btn1, btn2, btn3,btn4,btn5)
        bot.send_message(chat_id=message.chat.id, text='✖ Нету такого дейтвия брат... ✖', reply_markup=markup)

bot.polling(non_stop=True)
