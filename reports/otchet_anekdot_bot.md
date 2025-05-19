
# 📄 Отчёт о разработке Telegram-бота «АнекдотБот»

## 🔧 Цель проекта

Разработка Telegram-бота, который отправляет пользователю случайный анекдот из заранее подготовленного списка по команде или при нажатии кнопки.

---

## 🛠 Используемые технологии

- Язык программирования: **Python 3.10**
- Среда выполнения: локальная машина
- Telegram Bot API

---

## 📌 Этапы реализации

### 1. Регистрация Telegram-бота

- Через [@BotFather](https://t.me/BotFather) создан новый бот
- Получен API-токен для взаимодействия с Telegram

### 2. Установка зависимостей

```bash
pip install python-telegram-bot==20.0
```

### 3. Подготовка анекдотов

- Собрано 100 анекдотов с сайта [anekdot.ru](https://anekdot.ru/)
- Анекдоты сохранены в Python-списке `anekdots`

```python
anekdots = [
    "— Доктор, у меня провалы в памяти. — И с каких это пор? — С каких это пор что?",
    "Программист — это машина, превращающая кофе в баги.",
    "— Вовочка, почему у тебя домашка пустая? — Это спецэффект: невидимые чернила.",
    # ... (ещё 97 анекдотов)
]
```

### 4. Реализация логики бота

- Команда `/start` выводит инлайн-кнопки
- При нажатии кнопки отправляется случайный анекдот

#### Пример кода:

```python
from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import ApplicationBuilder, CommandHandler, CallbackQueryHandler, ContextTypes
import random

TOKEN = "ВАШ_ТОКЕН"

anekdots = [ ... ]  # список из 100 анекдотов

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    keyboard = [[InlineKeyboardButton("Получить анекдот", callback_data='get_anekdot')]]
    reply_markup = InlineKeyboardMarkup(keyboard)
    await update.message.reply_text("Нажми кнопку, чтобы получить анекдот", reply_markup=reply_markup)

async def button_handler(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    await query.answer()
    joke = random.choice(anekdots)
    await query.edit_message_text(text=joke)

app = ApplicationBuilder().token(TOKEN).build()
app.add_handler(CommandHandler("start", start))
app.add_handler(CallbackQueryHandler(button_handler))
app.run_polling()
```

---

## ✅ Результат

- Бот успешно запускается и работает в Telegram
- Пользователь может нажать кнопку и получить случайный анекдот
- Интерфейс интуитивно понятный

---

## 📌 Возможные улучшения

- Добавить категории анекдотов (программисты, школа, семья и т.д.)
- Интеграция с API (если появится официальный источник)
- Кнопка «ещё анекдот» после каждого ответа
- Подключение базы данных для хранения любимых анекдотов

---
