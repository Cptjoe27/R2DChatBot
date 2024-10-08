from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import Application, CallbackContext, CallbackQueryHandler
import requests

# Токен второго бота (бот2)
BOT2_TOKEN = '7937356562:AAGC2oM143YTxacQW_vhgqUCZ-O4HQbMtg4'

# Токен основного бота (бот1) для отправки команд
BOT1_TOKEN = 'YOUR_BOT1_TOKEN'  # Замените на токен первого бота

# URL основного бота (бот1) для отправки команды
BOT1_API_URL = f'https://api.telegram.org/bot{BOT1_TOKEN}/sendMessage'

# Функция для обработки нажатий кнопок "Принять" и "Отклонить"
async def handle_decision(update: Update, context: CallbackContext) -> None:
    query = update.callback_query
    query_data = query.data  # Данные кнопки

    action, user_id = query_data.split('_')

    if action == 'accept':
        # Если заявка принята, просто уведомляем менеджера
        await query.edit_message_text(text="Заявка принята.")
    elif action == 'decline':
        # Если заявка отклонена, отправляем команду первому боту, чтобы уведомить пользователя
        await decline_request(user_id)

        # Уведомляем менеджера, что заявка отклонена
        await query.edit_message_text(text="Заявка отклонена. Пользователь уведомлен.")

# Функция для отправки команды основному боту (бот1) на отклонение заявки
async def decline_request(user_id: str):
    # Формируем команду для отправки боту1
    data = {
        'chat_id': user_id,  # ID пользователя, чтобы бот1 знал, кому отправлять сообщение
        'text': f'/decline {user_id}'
    }

    # Отправляем запрос боту1 через его API
    response = requests.post(BOT1_API_URL, data=data)

    if response.status_code == 200:
        print(f"Уведомление об отклонении отправлено для пользователя {user_id}.")
    else:
        print(f"Ошибка при отправке уведомления: {response.text}")

# Функция запуска бота
def main():
    # Создаем приложение для второго бота
    application = Application.builder().token(BOT2_TOKEN).build()

    # Обработчик нажатий кнопок "Принять" и "Отклонить"
    application.add_handler(CallbackQueryHandler(handle_decision))

    # Запускаем бота
    application.run_polling()

if __name__ == '__main__':
    main()
