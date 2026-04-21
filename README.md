import os
import threading
from flask import Flask
from telegram.ext import Updater, CommandHandler, MessageHandler, Filters

# --- سيرفر الويب (للحماية من الإيقاف) ---
app = Flask('')
@app.route('/')
def home(): return "Uploader Bot is Online!"

def run_web():
    # بورت 10000 هو المطلوب في Render
    app.run(host='0.0.0.0', port=10000)

# --- معلومات البوت ---
TOKEN = "8156108151:AAFlF86m84n8-r99_E69fBqG1z8eZ_M9R_M"

def start(update, context):
    update.message.reply_text("👑 أهلاً بك يا إمبراطور.\nبوت الرفع شغال الآن على الاستضافة.")

def handle_docs(update, context):
    # هنا يستلم الملف ويأكد لك الاستلام
    update.message.reply_text("✅ استلمت الملف، جاري الحفظ على السيرفر...")

if __name__ == "__main__":
    # تشغيل الويب بخيط منفصل
    threading.Thread(target=run_web).start()

    # تشغيل البوت
    updater = Updater(TOKEN, use_context=True)
    dp = updater.dispatcher
    dp.add_handler(CommandHandler("start", start))
    dp.add_handler(MessageHandler(Filters.document, handle_docs))

    print("🚀 المحرك انطلق...")
    updater.start_polling()
