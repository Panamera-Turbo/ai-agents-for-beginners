<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "8164484c16b1ed3287ef9dae9fc437c1",
  "translation_date": "2025-07-23T08:15:02+00:00",
  "source_file": "10-ai-agents-production/README.md",
  "language_code": "ur"
}
-->
# پروڈکشن میں AI ایجنٹس: مشاہدہ اور جانچ

جب AI ایجنٹس تجرباتی پروٹوٹائپس سے حقیقی دنیا کی ایپلیکیشنز میں منتقل ہوتے ہیں، تو ان کے رویے کو سمجھنے، ان کی کارکردگی کی نگرانی کرنے، اور ان کے نتائج کا منظم طریقے سے جائزہ لینے کی صلاحیت اہم ہو جاتی ہے۔

## سیکھنے کے مقاصد

اس سبق کو مکمل کرنے کے بعد، آپ درج ذیل کو جان سکیں گے/سمجھ سکیں گے:
- ایجنٹ کے مشاہدے اور جانچ کے بنیادی تصورات
- ایجنٹس کی کارکردگی، لاگت، اور مؤثریت کو بہتر بنانے کی تکنیکیں
- اپنے AI ایجنٹس کا منظم طریقے سے جائزہ کیسے لیں اور کیا جانچیں
- پروڈکشن میں AI ایجنٹس کو تعینات کرتے وقت لاگت کو کیسے کنٹرول کریں
- AutoGen کے ساتھ بنائے گئے ایجنٹس کو کیسے انسٹرومنٹ کریں

مقصد یہ ہے کہ آپ کو وہ علم فراہم کیا جائے جو آپ کے "بلیک باکس" ایجنٹس کو شفاف، قابل انتظام، اور قابل اعتماد نظاموں میں تبدیل کر سکے۔

_**نوٹ:** محفوظ اور قابل اعتماد AI ایجنٹس کو تعینات کرنا ضروری ہے۔ [قابل اعتماد AI ایجنٹس بنانا](./06-building-trustworthy-agents/README.md) سبق کو بھی دیکھیں۔_

## ٹریسز اور اسپینز

مشاہدے کے اوزار جیسے [Langfuse](https://langfuse.com/) یا [Azure AI Foundry](https://learn.microsoft.com/en-us/azure/ai-foundry/what-is-azure-ai-foundry) عام طور پر ایجنٹ کے رنز کو ٹریسز اور اسپینز کے طور پر ظاہر کرتے ہیں۔

- **ٹریس** ایک مکمل ایجنٹ کے کام کو شروع سے آخر تک ظاہر کرتا ہے (جیسے صارف کی درخواست کو ہینڈل کرنا)۔
- **اسپینز** ٹریس کے اندر انفرادی مراحل ہوتے ہیں (جیسے لینگویج ماڈل کو کال کرنا یا ڈیٹا حاصل کرنا)۔

![Langfuse میں ٹریس درخت](https://langfuse.com/images/cookbook/example-autogen-evaluation/trace-tree.png)

مشاہدے کے بغیر، ایک AI ایجنٹ "بلیک باکس" کی طرح محسوس ہو سکتا ہے - اس کی اندرونی حالت اور استدلال غیر واضح ہوتے ہیں، جس سے مسائل کی تشخیص یا کارکردگی کو بہتر بنانا مشکل ہو جاتا ہے۔ مشاہدے کے ساتھ، ایجنٹس "گلاس باکسز" بن جاتے ہیں، جو شفافیت فراہم کرتے ہیں جو اعتماد پیدا کرنے اور یہ یقینی بنانے کے لیے ضروری ہے کہ وہ مطلوبہ طریقے سے کام کریں۔

## پروڈکشن ماحول میں مشاہدے کی اہمیت

AI ایجنٹس کو پروڈکشن ماحول میں منتقل کرنا نئے چیلنجز اور ضروریات کو متعارف کراتا ہے۔ مشاہدہ اب "اچھا ہو تو بہتر" کی بجائے ایک اہم صلاحیت بن جاتا ہے:

*   **ڈی بگنگ اور جڑ کی وجہ کا تجزیہ:** جب کوئی ایجنٹ ناکام ہو یا غیر متوقع نتیجہ پیدا کرے، تو مشاہدے کے اوزار ان غلطیوں کے ذرائع کی نشاندہی کے لیے ضروری ٹریسز فراہم کرتے ہیں۔ یہ خاص طور پر پیچیدہ ایجنٹس میں اہم ہے جو متعدد LLM کالز، ٹول تعاملات، اور مشروط منطق شامل کر سکتے ہیں۔
*   **لیٹنسی اور لاگت کا انتظام:** AI ایجنٹس اکثر LLMs اور دیگر بیرونی APIs پر انحصار کرتے ہیں جو فی ٹوکن یا فی کال بل کیے جاتے ہیں۔ مشاہدہ ان کالز کو درست طریقے سے ٹریک کرنے کی اجازت دیتا ہے، جس سے ان آپریشنز کی نشاندہی ہوتی ہے جو غیر ضروری طور پر سست یا مہنگے ہیں۔ یہ ٹیموں کو پرامپٹس کو بہتر بنانے، زیادہ مؤثر ماڈلز منتخب کرنے، یا ورک فلو کو دوبارہ ڈیزائن کرنے کے قابل بناتا ہے تاکہ آپریشنل لاگت کا انتظام کیا جا سکے اور صارف کے تجربے کو یقینی بنایا جا سکے۔
*   **اعتماد، حفاظت، اور تعمیل:** بہت سی ایپلیکیشنز میں، یہ یقینی بنانا ضروری ہے کہ ایجنٹس محفوظ اور اخلاقی طور پر کام کریں۔ مشاہدہ ایجنٹ کے اعمال اور فیصلوں کا آڈٹ ٹریل فراہم کرتا ہے۔ یہ پرامپٹ انجیکشن، نقصان دہ مواد کی تخلیق، یا ذاتی طور پر قابل شناخت معلومات (PII) کے غلط استعمال جیسے مسائل کا پتہ لگانے اور ان کو کم کرنے کے لیے استعمال کیا جا سکتا ہے۔ مثال کے طور پر، آپ یہ سمجھنے کے لیے ٹریسز کا جائزہ لے سکتے ہیں کہ ایجنٹ نے کسی خاص جواب یا ٹول کا استعمال کیوں کیا۔
*   **مسلسل بہتری کے لوپس:** مشاہدے کا ڈیٹا ایک تکراری ترقیاتی عمل کی بنیاد ہے۔ یہ دیکھ کر کہ ایجنٹس حقیقی دنیا میں کیسے کام کرتے ہیں، ٹیمیں بہتری کے شعبوں کی نشاندہی کر سکتی ہیں، ماڈلز کو بہتر بنانے کے لیے ڈیٹا اکٹھا کر سکتی ہیں، اور تبدیلیوں کے اثرات کی توثیق کر سکتی ہیں۔ یہ ایک فیڈ بیک لوپ بناتا ہے جہاں آن لائن جانچ سے حاصل کردہ پروڈکشن بصیرتیں آف لائن تجربات اور بہتری کو مطلع کرتی ہیں، جس سے ایجنٹ کی کارکردگی بتدریج بہتر ہوتی ہے۔

## کلیدی میٹرکس جو ٹریک کیے جانے چاہئیں

ایجنٹ کے رویے کی نگرانی اور سمجھنے کے لیے، مختلف میٹرکس اور سگنلز کو ٹریک کیا جانا چاہیے۔ اگرچہ مخصوص میٹرکس ایجنٹ کے مقصد پر منحصر ہو سکتے ہیں، کچھ عالمگیر اہمیت رکھتے ہیں۔

یہاں کچھ عام میٹرکس ہیں جن کی مشاہدے کے اوزار نگرانی کرتے ہیں:

**لیٹنسی:** ایجنٹ کتنی جلدی جواب دیتا ہے؟ طویل انتظار کے اوقات صارف کے تجربے پر منفی اثر ڈالتے ہیں۔ آپ کو ایجنٹ کے رنز کو ٹریس کرکے کاموں اور انفرادی مراحل کے لیے لیٹنسی کی پیمائش کرنی چاہیے۔ مثال کے طور پر، ایک ایجنٹ جو تمام ماڈل کالز کے لیے 20 سیکنڈ لیتا ہے، اسے تیز ماڈل استعمال کرکے یا ماڈل کالز کو متوازی طور پر چلا کر تیز کیا جا سکتا ہے۔

**لاگت:** فی ایجنٹ رن خرچ کیا ہے؟ AI ایجنٹس LLM کالز پر انحصار کرتے ہیں جو فی ٹوکن یا بیرونی APIs پر بل کیے جاتے ہیں۔ بار بار ٹول کا استعمال یا متعدد پرامپٹس تیزی سے لاگت میں اضافہ کر سکتے ہیں۔ مثال کے طور پر، اگر ایک ایجنٹ معمولی معیار کی بہتری کے لیے پانچ بار LLM کو کال کرتا ہے، تو آپ کو یہ اندازہ لگانا چاہیے کہ آیا لاگت جائز ہے یا آپ کالز کی تعداد کو کم کر سکتے ہیں یا سستا ماڈل استعمال کر سکتے ہیں۔ حقیقی وقت کی نگرانی غیر متوقع اسپائکس کی نشاندہی کرنے میں بھی مدد کر سکتی ہے (مثلاً، بگز جو ضرورت سے زیادہ API لوپس کا سبب بنتے ہیں)۔

**درخواست کی غلطیاں:** ایجنٹ نے کتنی درخواستیں ناکام کیں؟ اس میں API کی غلطیاں یا ناکام ٹول کالز شامل ہو سکتی ہیں۔ پروڈکشن میں ان کے خلاف اپنے ایجنٹ کو مزید مضبوط بنانے کے لیے، آپ فال بیکس یا ری ٹرائیز ترتیب دے سکتے ہیں۔ مثلاً، اگر LLM فراہم کنندہ A بند ہو، تو آپ بیک اپ کے طور پر LLM فراہم کنندہ B پر سوئچ کر سکتے ہیں۔

**صارف کی رائے:** براہ راست صارف کی تشخیصات قیمتی بصیرت فراہم کرتی ہیں۔ اس میں واضح درجہ بندی (👍تھمز اپ/👎ڈاؤن، ⭐1-5 ستارے) یا متنی تبصرے شامل ہو سکتے ہیں۔ مستقل منفی رائے آپ کو الرٹ کرنی چاہیے کیونکہ یہ اس بات کی علامت ہے کہ ایجنٹ توقع کے مطابق کام نہیں کر رہا۔

**غیر واضح صارف کی رائے:** صارف کے رویے بالواسطہ رائے فراہم کرتے ہیں، چاہے واضح درجہ بندی نہ ہو۔ اس میں فوری سوال کی دوبارہ ترتیب، بار بار کی گئی درخواستیں، یا ری ٹرائی بٹن پر کلک کرنا شامل ہو سکتا ہے۔ مثلاً، اگر آپ دیکھتے ہیں کہ صارفین بار بار ایک ہی سوال پوچھ رہے ہیں، تو یہ اس بات کی علامت ہے کہ ایجنٹ توقع کے مطابق کام نہیں کر رہا۔

**درستگی:** ایجنٹ کتنی بار درست یا مطلوبہ نتائج پیدا کرتا ہے؟ درستگی کی تعریف مختلف ہو سکتی ہے (مثلاً، مسئلہ حل کرنے کی درستگی، معلومات کی بازیافت کی درستگی، صارف کی اطمینان)۔ پہلا قدم یہ ہے کہ آپ کے ایجنٹ کے لیے کامیابی کی تعریف کریں۔ آپ خودکار چیکس، تشخیصی اسکورز، یا کام کی تکمیل کے لیبلز کے ذریعے درستگی کو ٹریک کر سکتے ہیں۔ مثال کے طور پر، ٹریسز کو "کامیاب" یا "ناکام" کے طور پر نشان زد کرنا۔

**خودکار تشخیصی میٹرکس:** آپ خودکار تشخیصات بھی ترتیب دے سکتے ہیں۔ مثلاً، آپ ایجنٹ کے آؤٹ پٹ کو اسکور کرنے کے لیے LLM استعمال کر سکتے ہیں، جیسے کہ آیا یہ مددگار، درست، یا نہیں۔ کئی اوپن سورس لائبریریاں بھی دستیاب ہیں جو ایجنٹ کے مختلف پہلوؤں کو اسکور کرنے میں مدد دیتی ہیں۔ مثلاً، [RAGAS](https://docs.ragas.io/) RAG ایجنٹس کے لیے یا [LLM Guard](https://llm-guard.com/) نقصان دہ زبان یا پرامپٹ انجیکشن کا پتہ لگانے کے لیے۔

عملی طور پر، ان میٹرکس کا مجموعہ AI ایجنٹ کی صحت کی بہترین کوریج فراہم کرتا ہے۔ اس باب کے [مثال نوٹ بک](../../../10-ai-agents-production/code_samples/10_autogen_evaluation.ipynb) میں، ہم آپ کو دکھائیں گے کہ یہ میٹرکس حقیقی مثالوں میں کیسے نظر آتے ہیں، لیکن پہلے ہم سیکھیں گے کہ ایک عام تشخیصی ورک فلو کیسا لگتا ہے۔

## اپنے ایجنٹ کو انسٹرومنٹ کریں

ٹریسنگ ڈیٹا اکٹھا کرنے کے لیے، آپ کو اپنے کوڈ کو انسٹرومنٹ کرنے کی ضرورت ہوگی۔ مقصد یہ ہے کہ ایجنٹ کے کوڈ کو انسٹرومنٹ کیا جائے تاکہ وہ ٹریسز اور میٹرکس پیدا کرے جنہیں مشاہدے کے پلیٹ فارم کے ذریعے پکڑا، پروسیس، اور بصری بنایا جا سکے۔

**اوپن ٹیلیمیٹری (OTel):** [اوپن ٹیلیمیٹری](https://opentelemetry.io/) LLM مشاہدے کے لیے ایک انڈسٹری اسٹینڈرڈ کے طور پر ابھرا ہے۔ یہ APIs، SDKs، اور ٹولز کا ایک سیٹ فراہم کرتا ہے جو ٹیلیمیٹری ڈیٹا کو پیدا کرنے، اکٹھا کرنے، اور ایکسپورٹ کرنے کے لیے استعمال ہوتا ہے۔

بہت سی انسٹرومنٹیشن لائبریریاں موجود ہیں جو موجودہ ایجنٹ فریم ورکس کو ریپ کرتی ہیں اور اوپن ٹیلیمیٹری اسپینز کو مشاہدے کے ٹول میں ایکسپورٹ کرنا آسان بناتی ہیں۔ نیچے ایک مثال دی گئی ہے کہ کیسے AutoGen ایجنٹ کو [OpenLit انسٹرومنٹیشن لائبریری](https://github.com/openlit/openlit) کے ساتھ انسٹرومنٹ کیا جائے:

```python
import openlit

openlit.init(tracer = langfuse._otel_tracer, disable_batch = True)
```

اس باب کے [مثال نوٹ بک](../../../10-ai-agents-production/code_samples/10_autogen_evaluation.ipynb) میں آپ دیکھیں گے کہ اپنے AutoGen ایجنٹ کو کیسے انسٹرومنٹ کریں۔

**دستی اسپین تخلیق:** اگرچہ انسٹرومنٹیشن لائبریریاں ایک اچھا بنیادی ڈھانچہ فراہم کرتی ہیں، لیکن اکثر ایسے کیسز ہوتے ہیں جہاں زیادہ تفصیلی یا حسب ضرورت معلومات کی ضرورت ہوتی ہے۔ آپ دستی طور پر اسپینز تخلیق کر سکتے ہیں تاکہ حسب ضرورت ایپلیکیشن منطق شامل کی جا سکے۔ مزید اہم بات یہ ہے کہ آپ خودکار یا دستی طور پر تخلیق کردہ اسپینز کو حسب ضرورت خصوصیات (جنہیں ٹیگز یا میٹا ڈیٹا بھی کہا جاتا ہے) کے ساتھ افزودہ کر سکتے ہیں۔ ان خصوصیات میں کاروباری مخصوص ڈیٹا، درمیانی حسابات، یا کوئی بھی سیاق و سباق شامل ہو سکتا ہے جو ڈی بگنگ یا تجزیہ کے لیے مفید ہو، جیسے `user_id`، `session_id`، یا `model_version`۔

[Langfuse Python SDK](https://langfuse.com/docs/sdk/python/sdk-v3) کے ساتھ دستی طور پر ٹریسز اور اسپینز تخلیق کرنے کی مثال:

```python
from langfuse import get_client
 
langfuse = get_client()
 
span = langfuse.start_span(name="my-span")
 
span.end()
```

## ایجنٹ کی جانچ

مشاہدہ ہمیں میٹرکس فراہم کرتا ہے، لیکن جانچ وہ عمل ہے جس میں ان ڈیٹا کا تجزیہ کیا جاتا ہے (اور ٹیسٹ کیے جاتے ہیں) تاکہ یہ معلوم کیا جا سکے کہ AI ایجنٹ کتنی اچھی کارکردگی کا مظاہرہ کر رہا ہے اور اسے کیسے بہتر بنایا جا سکتا ہے۔ دوسرے الفاظ میں، ایک بار جب آپ کے پاس وہ ٹریسز اور میٹرکس ہوں، تو آپ ان کا استعمال ایجنٹ کا جائزہ لینے اور فیصلے کرنے کے لیے کیسے کرتے ہیں؟

باقاعدہ جانچ ضروری ہے کیونکہ AI ایجنٹس اکثر غیر متعین ہوتے ہیں اور وقت کے ساتھ تبدیل ہو سکتے ہیں (اپ ڈیٹس یا ماڈل کے رویے میں تبدیلی کے ذریعے) – بغیر جانچ کے، آپ کو معلوم نہیں ہوگا کہ آپ کا "سمارٹ ایجنٹ" واقعی اپنا کام اچھے طریقے سے کر رہا ہے یا اس کی کارکردگی خراب ہو گئی ہے۔

AI ایجنٹس کے لیے دو قسم کی جانچ ہوتی ہے: **آن لائن جانچ** اور **آف لائن جانچ**۔ دونوں قیمتی ہیں اور ایک دوسرے کی تکمیل کرتے ہیں۔ ہم عام طور پر آف لائن جانچ سے شروع کرتے ہیں، کیونکہ یہ کسی بھی ایجنٹ کو تعینات کرنے سے پہلے کم از کم ضروری قدم ہے۔

### آف لائن جانچ

![Langfuse میں ڈیٹاسیٹ آئٹمز](https://langfuse.com/images/cookbook/example-autogen-evaluation/example-dataset.png)

یہ ایجنٹ کی جانچ ایک کنٹرول شدہ ماحول میں شامل ہے، عام طور پر ٹیسٹ ڈیٹاسیٹس کا استعمال کرتے ہوئے، نہ کہ لائیو صارف کی درخواستوں کا۔ آپ ایسے منتخب کردہ ڈیٹاسیٹس استعمال کرتے ہیں جہاں آپ کو معلوم ہو کہ متوقع نتیجہ یا درست رویہ کیا ہے، اور پھر اپنے ایجنٹ کو ان پر چلاتے ہیں۔

مثال کے طور پر، اگر آپ نے ایک ریاضی کے لفظی مسئلے کے ایجنٹ کو بنایا ہے، تو آپ کے پاس 100 مسائل کے [ٹیسٹ ڈیٹاسیٹ](https://huggingface.co/datasets/gsm8k) ہو سکتے ہیں جن کے جوابات معلوم ہیں۔ آف لائن جانچ اکثر ترقی کے دوران کی جاتی ہے (اور CI/CD پائپ لائنز کا حصہ ہو سکتی ہے) تاکہ بہتریوں کی جانچ کی جا سکے یا کارکردگی کی خرابیوں کے خلاف حفاظت کی جا سکے۔ فائدہ یہ ہے کہ یہ **دہرائی جانے والی ہے اور آپ کو واضح درستگی کے میٹرکس مل سکتے ہیں کیونکہ آپ کے پاس گراؤنڈ ٹروتھ ہے**۔ آپ صارف کی درخواستوں کی نقل بھی کر سکتے ہیں اور ایجنٹ کے جوابات کو مثالی جوابات کے خلاف ماپ سکتے ہیں یا اوپر بیان کردہ خودکار میٹرکس کا استعمال کر سکتے ہیں۔

آف لائن جانچ کے ساتھ کلیدی چیلنج یہ ہے کہ یہ یقینی بنانا کہ آپ کا ٹیسٹ ڈیٹاسیٹ جامع اور متعلقہ رہے – ایجنٹ ایک مقررہ ٹیسٹ سیٹ پر اچھی کارکردگی کا مظاہرہ کر سکتا ہے لیکن پروڈکشن میں بہت مختلف درخواستوں کا سامنا کر سکتا ہے۔ لہذا، آپ کو نئے ایج کیسز اور حقیقی دنیا کے منظرناموں کی عکاسی کرنے والی مثالوں کے ساتھ ٹیسٹ سیٹس کو اپ ڈیٹ رکھنا چاہیے۔ چھوٹے "اسموک ٹیسٹ" کیسز اور بڑے جانچ سیٹس کا امتزاج مفید ہے: فوری چیک کے لیے چھوٹے سیٹس اور وسیع کارکردگی کے میٹرکس کے لیے بڑے سیٹس۔

### آن لائن جانچ

![مشاہدے کے میٹرکس کا جائزہ](https://langfuse.com/images/cookbook/example-autogen-evaluation/dashboard.png)

یہ ایجنٹ کی جانچ ایک لائیو، حقیقی دنیا کے ماحول میں شامل ہے، یعنی پروڈکشن میں اصل استعمال کے دوران۔ آن لائن جانچ میں ایجنٹ کی کارکردگی کو حقیقی صارف کی تعاملات پر مسلسل نگرانی کرنا اور نتائج کا تجزیہ شامل ہے۔

مثال کے طور پر، آپ کامیابی کی شرح، صارف کی اطمینان کے اسکورز، یا لائیو ٹریفک پر دیگر میٹرکس کو ٹریک کر سکتے ہیں۔ آن لائن جانچ کا فائدہ یہ ہے کہ یہ **ایسی چیزوں کو پکڑتی ہے جن کی آپ لیب کے ماحول میں توقع نہیں کر سکتے** – آپ وقت کے ساتھ ماڈل کی کارکردگی میں کمی کا مشاہدہ کر سکتے ہیں (اگر ایجنٹ کی مؤثریت ان پٹ پیٹرنز کے بدلنے کے ساتھ کم ہو جائے) اور غیر متوقع درخواستوں یا حالات کو پکڑ سکتے ہیں جو آپ کے ٹیسٹ ڈیٹا میں نہیں تھے۔ یہ ایجنٹ کے جنگلی ماحول میں رویے کی حقیقی تصویر فراہم کرتا ہے۔

آن لائن جانچ میں اکثر بالواسطہ اور براہ راست صارف کی رائے اکٹھا کرنا شامل ہوتا ہے، جیسا کہ پہلے ذکر کیا گیا، اور ممکنہ طور پر شیڈو ٹیسٹس یا A/B ٹیسٹس چلانا (جہاں ایجنٹ کا نیا ورژن پرانے کے ساتھ موازنہ کرنے کے لیے متوازی طور پر چلتا ہے)۔ چیلنج یہ ہے کہ لائیو تعاملات کے لیے قابل اعتماد لیبلز یا اسکورز حاصل کرنا مشکل ہو سکتا ہے – آپ صارف کی رائے یا نیچے کی طرف میٹرکس پر انحصار کر سکتے ہیں (جیسے کیا صارف نے نتیجہ پر کلک کیا)۔

### دونوں کا امتزاج

آن لائن اور آف لائن جانچ ایک دوسرے کے متضاد

- متعین کردہ پیرامیٹرز، پرامپٹس، اور ٹولز کے ناموں کو بہتر بنائیں۔  
| ملٹی ایجنٹ سسٹم مستقل طور پر کام نہیں کر رہا | - ہر ایجنٹ کو دیے گئے پرامپٹس کو بہتر بنائیں تاکہ وہ مخصوص اور ایک دوسرے سے مختلف ہوں۔<br>- ایک درجہ بندی والا نظام بنائیں جو "روٹنگ" یا کنٹرولر ایجنٹ کا استعمال کرے تاکہ یہ طے کیا جا سکے کہ کون سا ایجنٹ درست ہے۔ |

ان مسائل کی نشاندہی زیادہ مؤثر طریقے سے کی جا سکتی ہے اگر مشاہداتی نظام موجود ہو۔ وہ ٹریسز اور میٹرکس جن پر ہم نے پہلے بات کی تھی، یہ جاننے میں مدد دیتے ہیں کہ ایجنٹ کے ورک فلو میں مسائل کہاں پیش آ رہے ہیں، جس سے ڈیبگنگ اور آپٹیمائزیشن زیادہ مؤثر ہو جاتی ہے۔

## اخراجات کا انتظام

یہاں کچھ حکمت عملیاں دی گئی ہیں جو AI ایجنٹس کو پروڈکشن میں تعینات کرنے کے اخراجات کو کم کرنے میں مددگار ثابت ہو سکتی ہیں:

**چھوٹے ماڈلز کا استعمال:** چھوٹے لینگویج ماڈلز (SLMs) کچھ ایجنٹک استعمال کے کیسز میں اچھی کارکردگی دکھا سکتے ہیں اور اخراجات کو نمایاں طور پر کم کر سکتے ہیں۔ جیسا کہ پہلے ذکر کیا گیا، ایک تشخیصی نظام بنانا جو کارکردگی کا موازنہ بڑے ماڈلز سے کرے، یہ سمجھنے کا بہترین طریقہ ہے کہ SLM آپ کے استعمال کے کیس میں کتنی اچھی کارکردگی دکھائے گا۔ SLMs کو آسان کاموں جیسے ارادے کی درجہ بندی یا پیرامیٹر نکالنے کے لیے استعمال کرنے پر غور کریں، جبکہ پیچیدہ استدلال کے لیے بڑے ماڈلز کو محفوظ رکھیں۔

**روٹر ماڈل کا استعمال:** ایک مشابہ حکمت عملی یہ ہے کہ مختلف سائز اور ماڈلز کا استعمال کریں۔ آپ ایک LLM/SLM یا سرور لیس فنکشن استعمال کر سکتے ہیں تاکہ پیچیدگی کی بنیاد پر درخواستوں کو بہترین ماڈلز کی طرف بھیجا جا سکے۔ یہ اخراجات کو کم کرنے میں مدد دے گا اور ساتھ ہی صحیح کاموں پر کارکردگی کو یقینی بنائے گا۔ مثال کے طور پر، آسان سوالات کو چھوٹے، تیز ماڈلز کی طرف بھیجیں، اور صرف پیچیدہ استدلال کے کاموں کے لیے مہنگے بڑے ماڈلز کا استعمال کریں۔

**جوابات کو کیش کرنا:** عام درخواستوں اور کاموں کی نشاندہی کرنا اور ان کے جوابات کو پہلے سے فراہم کرنا، اس سے پہلے کہ وہ آپ کے ایجنٹک نظام سے گزریں، ایک اچھا طریقہ ہے تاکہ مشابہ درخواستوں کی تعداد کم ہو۔ آپ ایک ایسا فلو بھی نافذ کر سکتے ہیں جو یہ طے کرے کہ کوئی درخواست آپ کے کیش شدہ درخواستوں سے کتنی مشابہ ہے، اور اس کے لیے زیادہ بنیادی AI ماڈلز کا استعمال کریں۔ یہ حکمت عملی اکثر پوچھے جانے والے سوالات یا عام ورک فلو کے لیے اخراجات کو نمایاں طور پر کم کر سکتی ہے۔

## آئیے دیکھتے ہیں کہ یہ عملی طور پر کیسے کام کرتا ہے

اس سیکشن کے [مثالی نوٹ بک](../../../10-ai-agents-production/code_samples/10_autogen_evaluation.ipynb) میں، ہم دیکھیں گے کہ مشاہداتی ٹولز کا استعمال کرتے ہوئے اپنے ایجنٹ کی نگرانی اور تشخیص کیسے کی جا سکتی ہے۔

## پچھلا سبق

[میٹا کوگنیشن ڈیزائن پیٹرن](../09-metacognition/README.md)

## اگلا سبق

[MCP](../11-mcp/README.md)

**ڈسکلیمر**:  
یہ دستاویز AI ترجمہ سروس [Co-op Translator](https://github.com/Azure/co-op-translator) کا استعمال کرتے ہوئے ترجمہ کی گئی ہے۔ ہم درستگی کے لیے کوشش کرتے ہیں، لیکن براہ کرم آگاہ رہیں کہ خودکار ترجمے میں غلطیاں یا غیر درستیاں ہو سکتی ہیں۔ اصل دستاویز کو اس کی اصل زبان میں مستند ذریعہ سمجھا جانا چاہیے۔ اہم معلومات کے لیے، پیشہ ور انسانی ترجمہ کی سفارش کی جاتی ہے۔ ہم اس ترجمے کے استعمال سے پیدا ہونے والی کسی بھی غلط فہمی یا غلط تشریح کے ذمہ دار نہیں ہیں۔