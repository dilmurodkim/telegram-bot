# telegram-bot
# main.py
from aiogram import Bot, Dispatcher, types
from aiogram.utils import executor
from aiogram.types import InlineKeyboardMarkup, InlineKeyboardButton, ReplyKeyboardMarkup, KeyboardButton
import logging
import os

# TOKEN bu yerga qo'shiladi
API_TOKEN = os.getenv("7752977498:AAE7b885o9hw10KcnJEDclLal1XiX9mGpLk") or "7752977498:AAE7b885o9hw10KcnJEDclLal1XiX9mGpLk"

logging.basicConfig(level=logging.INFO)

bot = Bot(token=API_TOKEN)
dp = Dispatcher(bot)

# Asosiy menyu tugmalari
main_menu = ReplyKeyboardMarkup(resize_keyboard=True)
main_menu.add(
    KeyboardButton("📚 TOPIK 1"),
    KeyboardButton("📖 서울대 한국어 1A/1B"),
    KeyboardButton("🌤 Harflar")
)

# Harflar tugmalari
hangeul_letters_data= {
    "ㄱ": "talaffuz: ㄱ (g/k):\ngap boshida k\nichida g kabi\n\nmisol: 고기\ntalaffuzi: gogi\ntarjima: go‘sht",
    "ㄲ": "talaffuz: ㄲ (kk):\nkuchli k tovushi\n\nmisol: 끼다\ntalaffuzi: kkida\ntarjima: kiymoq",
    "ㄴ": "talaffuz: ㄴ (n):\ndoimo n kabi\n\nmisol: 누구\ntalaffuzi: dugu\ntarjima: kim?",
    "ㄷ": "talaffuz: ㄷ (d/t):\nboshida t\nichida d\n\nmisol: 다리\ntalaffuzi: dari\ntarjima: oyoq / ko‘prik",
    "ㄸ": "talaffuz: ㄸ (tt):\nkuchli t tovushi\n\nmisol: 땅\ntalaffuzi: ttang\ntarjima: yer",
    "ㄹ": "talaffuz: ㄹ (r/l):\nboshida r\noxirida yoki undoshdan keyin l\n\nmisol: 사람\ntalaffuzi: saram\ntarjima: inson",
    "ㅁ": "talaffuz: ㅁ (m):\nm tovushi\n\nmisol: 머리\ntalaffuzi: mori\ntarjima: bosh",
    "ㅂ": "talaffuz: ㅂ (b/p):\nboshida p\nichida b\n\nmisol: 바지\ntalaffuzi: paji\ntarjima: shim",
    "ㅃ": "talaffuz: ㅃ (pp):\nkuchli p tovushi\n\nmisol: 빵\ntalaffuzi: ppang\ntarjima: non",
    "ㅅ": "talaffuz: ㅅ (s):\ni bilan yumshoq eshitiladi\n\nmisol: 사과\ntalaffuzi: sagwa\ntarjima: olma",
    "ㅆ": "talaffuz: ㅆ (ss):\nkuchli s tovushi\n\nmisol: 쌀\ntalaffuzi: ssal\ntarjima: guruch",
    "ㅇ": "talaffuz: ㅇ (ng):\nboshida aytilmaydi\noxirida ng sifatida\n\nmisol: 아이\ntalaffuzi: ai\ntarjima: bola",
    "ㅈ": "talaffuz: ㅈ (j):\nj tovushi\n\nmisol: 자전거\ntalaffuzi: jajŏngŏ\ntarjima: velosiped",
    "ㅉ": "talaffuz: ㅉ (jj):\nkuchli j tovushi\n\nmisol: 짜다\ntalaffuzi: jjada\ntarjima: sho‘r",
    "ㅊ": "talaffuz: ㅊ (ch):\nch tovushi\n\nmisol: 친구\ntalaffuzi: chinggu\ntarjima: do‘st",
    "ㅋ": "talaffuz: ㅋ (k):\nkuchli k\n\nmisol: 코\ntalaffuzi: ko\ntarjima: burun",
    "ㅌ": "talaffuz: ㅌ (t):\nkuchli t\n\nmisol: 토끼\ntalaffuzi: tokki\ntarjima: quyon",
    "ㅍ": "talaffuz: ㅍ (p):\nkuchli p\n\nmisol: 포도\ntalaffuzi: podo\ntarjima: uzum",
    "ㅎ": "talaffuz: ㅎ (h):\nh tovushi\n\nmisol: 하나\ntalaffuzi: hana\ntarjima: bir",
    "ㅏ": "talaffuz: ㅏ (a):\nog‘iz katta ochiladi\n\nmisol: 아빠\ntalaffuzi: appa\ntarjima: dada",
    "ㅐ": "talaffuz: ㅐ (ae):\ne ga o‘xshash\n\nmisol: 개\ntalaffuzi: ke\ntarjima: it",
    "ㅑ": "talaffuz: ㅑ (ya):\nya tovushi\n\nmisol: 야채\ntalaffuzi: yachae\ntarjima: sabzavot",
    "ㅒ": "talaffuz: ㅒ (yae):\nya + e\n\nmisol: 얘기\ntalaffuzi: yaegi\ntarjima: suhbat",
    "ㅓ": "talaffuz: ㅓ (eo):\no ga o‘xshash\norqadan chiqadi\n\nmisol: 어머니\ntalaffuzi: ŏmŏni\ntarjima: ona",
    "ㅔ": "talaffuz: ㅔ (e):\ninglizcha e kabi\n\nmisol: 네\ntalaffuzi: ne\ntarjima: ha",
    "ㅕ": "talaffuz: ㅕ (yeo):\nyo ga o‘xshash\n\nmisol: 여자\ntalaffuzi: yŏja\ntarjima: ayol",
    "ㅖ": "talaffuz: ㅖ (ye):\nye tovushi\n\nmisol: 예\ntalaffuzi: ye\ntarjima: ha (hurmatli)",
    "ㅗ": "talaffuz: ㅗ (o):\nyuqoriga qarab o\n\nmisol: 오이\ntalaffuzi: oi\ntarjima: bodring",
    "ㅘ": "talaffuz: ㅘ (wa):\no + a\n\nmisol: 사과\ntalaffuzi: sagwa\ntarjima: olma",
    "ㅙ": "talaffuz: ㅙ (wae):\no + ae\n\nmisol: 왜\ntalaffuzi: wae\ntarjima: nega",
    "ㅚ": "talaffuz: ㅚ (oe):\nwe yoki ö kabi\n\nmisol: 외국\ntalaffuzi: oeguk\ntarjima: chet el",
    "ㅛ": "talaffuz: ㅛ (yo):\nyuqoriga qarab yo\n\nmisol: 요리\ntalaffuzi: yori\ntarjima: taom",
    "ㅜ": "talaffuz: ㅜ (u):\npastga qarab u\n\nmisol: 우유\ntalaffuzi: uyu\ntarjima: sut",
    "ㅝ": "talaffuz: ㅝ (wo):\nu + eo\n\nmisol: 워터\ntalaffuzi: wŏtŏ\ntarjima: suv",
    "ㅞ": "talaffuz: ㅞ (we):\nu + e\n\nmisol: 웨딩\ntalaffuzi: weding\ntarjima: to‘y",
    "ㅟ": "talaffuz: ㅟ (wi):\nu + i\n\nmisol: 위\ntalaffuzi: wi\ntarjima: usti",
    "ㅠ": "talaffuz: ㅠ (yu):\npastga qarab yu\n\nmisol: 유리\ntalaffuzi: yuri\ntarjima: oynavand",
    "ㅡ": "talaffuz: ㅡ (eu):\nog‘iz tekis\n\nmisol: 으깨다\ntalaffuzi: ukkæda\ntarjima: ezmoq",
    "ㅢ": "talaffuz: ㅢ (ui):\neu + i\n\nmisol: 의사\ntalaffuzi: ŭisa\ntarjima: shifokor",
    "ㅣ": "talaffuz: ㅣ (i):\ni tovushi\n\nmisol: 이름\ntalaffuzi: irŭm\ntarjima: ism"
}







letter_menu = InlineKeyboardMarkup(row_width=5)
for letter in hangeul_letters_data:
    letter_menu.insert(InlineKeyboardButton(letter, callback_data=f"letter_{letter}"))
letter_menu.add(InlineKeyboardButton("🔙 Orqaga", callback_data="back_main"))


# Grammatik ma'lumotlar 1A
grammar_1A = {
    "1A_1": "N+은/는::Urg'u va yuklama vafifasini bajaradi...",
    "1A_2": "N+입니까? / N+입니다::So‘roq va darak gap tuzishda ishlatiladi.\nMasalan: 학생입니까?\nO'quvchimi?\n네, 학생입니다.\nHa o'quvchidir.",
    "1A_3": "N+이/가 아닙니다::Inkor shaklida ishlatiladi\n'이 아닙니다' Undosh bilan tugasa\n '가 아닙니다'Unli bilan tugasa\n Masalan:저는 학생이 아닙니다.\n Men o'quvchi emasman.",
    "1A_4": "N+이/가 있어요 / 없어요::Bor / yo‘qni bildiradi.\nMasalan: 책이 있어요.\nKitob bor.\n컴퓨터가 없어요.\nKompyuter yo'q.",
    "1A_5": "이것은 / 그것은 / 저것은::Bu / u / anavi narsani bildiradi. Masalan: 이것은 책입니다.\nBu narsa kitob dir.",
    "1A_6": "주세요 ::Biror narsani iltimos qilish. Masalan: 물 좀 주세요.\nBiroz suv bering.",
    "1A_7": "N+하고 / 과/와 / (이)랑::\"...bilan\" ma'nosini beradi. Masalan: 친구하고 같이 갔어요.\nDo'stim bilan birga bordik.",
    "1A_8": "A/V + 아요 / 어요 / 해요::Fe’l yoki sifatga qo‘shilib, hozirgi yoki hozirgi zamonga yaqin ish-harakatni bildiradi.\n\n✅ Qoidalar:\n1) ㅏ/ㅗ bilan tugasa → 아요: 가다 → 가요 (boraman)\n2) boshqa unlilar bilan → 어요: 먹다 → 먹어요 (yeyman)\n3) 하다 bilan → 해요: 공부하다 → 공부해요 (dars qilaman)\n\n📌 Misollar:\n- 가요 (boraman)\n- 와요 (kelaman)\n- 먹어요 (yeyman)\n- 공부해요 (dars qilaman)",
    "1A_9": "N+을/를::-ni qo'shimchasi\nMasalan: 밥을 먹어요.\nOvqatNI yeyapman.",
    "1A_10": "N+에서::Harakat yoki holat sodir bo‘ladigan joylarga ishlatiladi. Masalan: 집에서 공부해요.\nUyda o'qiyapman.",
    "1A_11": "안 A/V::tarjimasi:-mayman\n-maydi\nInkor shakli. Masalan: 안 가요,\nBormayman\n안 먹어요.\nYemayman.",
    "1A_12": "N+에 있어요 / 없어요::Joy nomlariga nisbatan\nTarjimasi:-da bor\n-da yo'q\nMasalan: 교실에 책이 있어요.\nSinfxonada kitob bor.",
    "1A_13": "N+에 가요 / 와요::Yo‘nalish bildiradi.\nTarjimasi:-ga bormoq\n-dan kelmoq\nMasalan: 학교에 가요.\nMaktabga borayapman.",
    "1A_14": "앞 / 옆 / 뒤::Joylashuvlarga nisbatan ishlatiladi.\nTarjimasi:oldi, yon, orqa. Masalan: 집 앞에 있어요.\nUy yonida bor.",
    "1A_15": "요일::Haftaning kunlari: 월요일 (Dushanba),\n화요일 (Seshanba),\n수요일 (Chorshanba),\n목요일 (Payshanba),\n금요일 (Juma)\n토요일 (Shanba)\n일요일 (Yakshanba)",
    "1A_16": "N+에::Vaqt yoki joy bildiradi.\nTarjimasi:-da yoki -ga\nMasalan: 오전 9시에 학교에 가요.\nErtalab 9soatda maktabga boraman.",
    "1A_17": "A/V + 았/었/했어요::O‘tgan zamon.\n\n✅ Qoidalar:\n1) Fe’lning oxirgi bo‘g‘inida ㅏ yoki ㅗ bo‘lsa → 았어요: 가다 → 갔어요 (bordim)\n2) Boshqa unlilar bo‘lsa → 었어요: 먹다 → 먹었어요 (yedim)\n3) 하다 fe’li → 했어요: 공부하다 → 공부했어요 (o‘qidim)\n\n📌 Misollar:\n- 갔어요 (bordim)\n- 왔어요 (keldim)\n- 먹었어요 (yedim)\n- 공부했어요 (o‘qidim)",
    "1A_18": "A/V + 지만::Qarama-qarshi fikr.\nTarjimasi:ammo yoki lekin\nMasalan: 맛있지만 비싸요,\nMazzali lekin qimmat.",
    "1A_19": "V + 고::Ikki harakatlarni bir-biriga bog‘laydi.\nTarjimasi:-b yoki -ib \nMasalan: 밥을 먹고 공부해요,\nOvqatni yeb tahsil olyapman.",
    "1A_20": "V + (으)세요::Hurmat shakli.\nTarjimasi:-ing\nMasalan: 들어오세요,\nKiring.",
    "1A_21": "A/V + ㅂ/습니까?, ㅂ/습니다::Rasmiy so‘zlashuv uslubi. Masalan: 갑니까?\nBorasizmi,\n갑니다,\nBoraman.",
    "1A_22": "V + 을/ㄹ까요?::Taklif yoki mulohaza.\nTarjimasi:-mizmi\nMasalan: 갈까요?\nboramizmi.",
    "1A_23": "이/그/저::Ko‘rsatish olmoshlari: bu, u, anavi. Masalan: 이 사람,\nBu odam.",
    "1A_24": "A/V + 네요::Tarjimasi: ekan\nHayronlanish, ajablanish bildiradi. Masalan: 날씨가 좋네요!. Havo yaxshi ekan!"
}

# /start
@dp.message_handler(commands=['start'])
async def send_welcome(message: types.Message):
    await message.answer("한국어 o'rgatuvchi botga xush kelibsiz!\n\nMenyudan birini tanlang:", reply_markup=main_menu)

# Harflar tugmasi
@dp.message_handler(lambda message: message.text == "🌤 Harflar")
async def show_letters(message: types.Message):
    await message.answer("Quyidagi harflardan birini tanlang:", reply_markup=letter_menu)

@dp.callback_query_handler(lambda c: c.data.startswith("letter_"))
async def explain_letter(call: types.CallbackQuery):
    letter = call.data.split("_")[1]
    explanation = hangeul_letters_data.get(letter, "Izoh topilmadi")
    await call.message.edit_text(f"<b>{letter}</b> harfi: {explanation}", parse_mode="HTML", reply_markup=letter_menu)


# TOPIK 1 tugmasi
@dp.message_handler(lambda message: message.text == "📚 TOPIK 1")
async def topik_handler(message: types.Message):
    await message.answer("📚 TOPIK 1 kanali: [Kanalga o‘tish](https://t.me/koreystili_birgalikda)", parse_mode="Markdown", reply_markup=main_menu)

# 1A/1B menyusi
@dp.message_handler(lambda message: message.text == "📖 서울대 한국어 1A/1B")
async def show_books(message: types.Message):
    markup = InlineKeyboardMarkup(row_width=2)
    markup.add(
        InlineKeyboardButton("🅰️ 서울대 한국어 1A", callback_data="book_1A"),
        InlineKeyboardButton("🅱️ 서울대 한국어 1B", callback_data="book_1B")
    )
    await message.answer("Sizga qaysi kitob kerak:", reply_markup=markup)

@dp.callback_query_handler(lambda c: c.data == "book_1A")
async def handle_1A(call: types.CallbackQuery):
    markup = InlineKeyboardMarkup(row_width=2)
    for i in range(1, 25):
        key = f"1A_{i}"
        grammar_title = grammar_1A.get(key, f"1A_{i}::Nomlanmagan")
        markup.insert(InlineKeyboardButton(grammar_title.split("::")[0], callback_data=key))
    markup.add(InlineKeyboardButton("🔙 Orqaga", callback_data="back_main"))
    await call.message.edit_text("🅰️ 1A grammatikalari:", reply_markup=markup)

@dp.callback_query_handler(lambda c: c.data == "book_1B")
async def handle_1B(call: types.CallbackQuery):
    await call.message.edit_text("1B grammatikalari hozircha tayyor emas.", reply_markup=main_menu)

@dp.callback_query_handler(lambda c: c.data.startswith("1A_"))
async def send_grammar(call: types.CallbackQuery):
    title, content = grammar_1A[call.data].split("::")
    await call.message.answer(f"<b>{title}</b>\n\n{content}", parse_mode="HTML", reply_markup=main_menu)

@dp.callback_query_handler(lambda c: c.data == "back_main")
async def go_back(call: types.CallbackQuery):
    await call.message.answer("🔙 Asosiy menyu:", reply_markup=main_menu)

# === Ishga tushirish ===
if __name__ == '__main__':
    executor.start_polling(dp, skip_updates=True)
