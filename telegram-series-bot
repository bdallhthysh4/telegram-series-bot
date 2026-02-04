import os
import requests
from telegram import Bot, ParseMode
from telegram.ext import Updater, CommandHandler
from datetime import datetime

# ==============================
# 🌟 جلب التوكنات من Environment Variables
# ==============================
TELEGRAM_TOKEN = "8499348359:AAGwpynbSaeEUd0C_TEg4T-LDUT5tNESniU"
ADSTERRA_TOKEN = "29bd8a8326fc382de6c114c0b4508272"

bot = Bot(token=TELEGRAM_TOKEN)

# ==============================
# 🌟 دالة لجلب بيانات Adsterra
# ==============================
def get_adsterra_stats():
    """
    ترجع إحصائيات Adsterra بشكل مرتب
    """
    url = "https://panel.adsterra.com/api/v1/stats"  # رابط API
    headers = {"Authorization": f"Bearer {ADSTERRA_TOKEN}"}
    try:
        response = requests.get(url, headers=headers, timeout=10)
        if response.status_code == 200:
            data = response.json()
            # مثال للعرض، عدّل حسب هيكلية API عندك
            today = datetime.now().strftime("%Y-%m-%d")
            stats_text = f"""
📊 *Adsterra Stats*

🗓 *اليوم:* {today}
👁️ *الزيارات:* {data.get('impressions', 0)}
🖱 *النقرات:* {data.get('clicks', 0)}
📈 *CTR:* {data.get('ctr', 0)}%
💵 *ربح اليوم:* ${data.get('today_earning', 0)}
💰 *الرصيد الكلي:* ${data.get('balance', 0)}

⚡ _تقرير سريع من بوتك الخرافي!_
"""
            return stats_text
        else:
            return "❌ فشل جلب البيانات من Adsterra. تحقق من التوكن أو الاتصال."
    except requests.exceptions.RequestException:
        return "❌ خطأ في الاتصال بـ Adsterra API."

# ==============================
# 🌟 دالة /stats في البوت
# ==============================
def stats(update, context):
    chat_id = update.effective_chat.id
    bot.send_message(chat_id=chat_id,
                     text=get_adsterra_stats(),
                     parse_mode=ParseMode.MARKDOWN)

# ==============================
# 🌟 إعداد التحديثات
# ==============================
updater = Updater(token=TELEGRAM_TOKEN, use_context=True)
dispatcher = updater.dispatcher
dispatcher.add_handler(CommandHandler('stats', stats))

# ==============================
# 🌟 رسالة ترحيبية عند بدء البوت
# ==============================
def start(update, context):
    chat_id = update.effective_chat.id
    bot.send_message(
        chat_id=chat_id,
        text="🤖 أهلاً! أنا بوت Adsterra الأسطوري.\n"
             "استخدم /stats للحصول على إحصائياتك اليومية.",
        parse_mode=ParseMode.MARKDOWN
    )

dispatcher.add_handler(CommandHandler('start', start))

# ==============================
# 🌟 تشغيل البوت
# ==============================
print("🤖 البوت شغال! اضغط Ctrl+C لإيقافه")
updater.start_polling()
updater.idle()
