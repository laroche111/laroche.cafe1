# Laroche Cafe — QR Table & Waiter Call System

هاد الريبو فيه 3 مشاريع منفصلة، كل وحدة لازم تترفع/تُنشر لحالها:

```
laroche-repo/
├── backend/          → السيرفر المشترك (API)
├── customer-app/     → تطبيق الزبون (اللي بيفتحه الزبون من QR)
└── waiter-app/       → لوحة النادل + صفحة طباعة الـ QR
```

كل مجلد لازم يترفع/ينشر بشكل مستقل (3 روابط مختلفة بالنهاية).

---

## 1) نشر الـ backend أولاً

- ادخل ع [Render](https://render.com) (أو Railway/Fly.io، أي شي بيضل شغال 24/7).
- اعمل **New Web Service** واربطه بمجلد `backend/` بالريبو.
- Build command: `npm install`
- Start command: `npm start`
- (اختياري بس مستحسن) ضيف Environment Variable:
  - `ALLOWED_ORIGINS` = روابط الـ customer-app و الـ waiter-app بعد ما تنشرهم، مفصولين بفاصلة، مثلاً:
    `https://laroche-customer.netlify.app,https://laroche-waiter.netlify.app`
- بعد النشر رح ياخد رابط شكله متل: `https://laroche-backend.onrender.com`

⚠️ ملاحظة: التخزين هون عبارة عن ملف `data.json` بسيط — كافي للبداية، بس إذا السيرفر (متل Render Free) بيعيد تشغيل نفسه بيضيع الملف أحياناً. إذا بدك تخزين دائم 100%، لاحقاً منقدر نبدله بقاعدة بيانات حقيقية (Postgres مثلاً) بدون ما نلمس الفرونت إند.

---

## 2) نشر customer-app و waiter-app

كل وحدة فيها ملف `config.js` — افتحه وحط فيه رابط الـ backend يلي طلع من الخطوة 1:

```js
window.LAROCHE_API_BASE = "https://laroche-backend.onrender.com";
```

بعدين انشر كل مجلد كموقع static لحاله (Netlify، Vercel، GitHub Pages، أو أي استضافة static):
- انشر `customer-app/` → هيدا الرابط يلي بتحطه بالـ QR كودات
- انشر `waiter-app/` → هيدا الرابط بيتعطى للطاقم بس (ما ينشر ع مواقع التواصل)

التنقل جوا التطبيقين شغال بنظام `#/...` (hash routing)، يعني **ما في داعي لأي إعداد rewrite/redirect** عالسيرفر — رابط `/table/7` عندك رح يصير مثلاً:
`https://laroche-customer.netlify.app/#/table/7`

---

## 3) بعد ما كل شي منشور

1. افتح waiter-app → روح ع "صفحة الطباعة" (`#/admin-print`)
2. حط رابط الـ customer-app جوا الخانة
3. رح تطلع 25 QR كود جاهزين للطباعة، كل وحدة بتودي مباشرة لطاولتها

---

## شنو لازم ترفعه ع GitHub بالظبط

كل الملفات يلي بالريبو **ما عدا**:
- `backend/node_modules/` (منتج تلقائياً، ما بيترفع — موجود بـ `.gitignore`)
- `backend/data.json` (بيانات وقت التشغيل، مش كود — موجود بـ `.gitignore`)
- أي ملف `.env` إذا ضفته لاحقاً لأسرار زي مفاتيح API

يعني عملياً بترفع:
```
backend/server.js
backend/package.json
backend/.gitignore
customer-app/index.html
customer-app/config.js
waiter-app/index.html
waiter-app/config.js
README.md
```

`config.js` بترفعه بالقيمة الافتراضية (placeholder)، وبعد ما تعرف رابط الـ backend الحقيقي بتحدثه مباشرة بالريبو (أو محلياً وبعدين commit).
