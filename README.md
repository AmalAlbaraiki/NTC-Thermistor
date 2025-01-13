# NTC-Thermistor
# قياس درجة الحرارة باستخدام حساس NTC والأردوينو

## الهدف
قياس درجة الحرارة باستخدام حساس مقاوم NTC Thermistor عبر الأردوينو، وعرض النتائج على شاشة المراقبة التسلسلية.

## الأدوات المستخدمة
- Arduino Uno
- NTC Thermistor (10kΩ)
- مقاومة ثابتة (10kΩ ±5%)
- بريدبورد وأسلاك توصيل

## خطوات التجربة
1. توصيل حساس NTC مع المقاومة في دائرة مجزئ الجهد.
2. قراءة الجهد الناتج عند النقطة المشتركة بين الحساس والمقاومة.
3. تحويل الجهد إلى مقاومة، ثم إلى درجة حرارة باستخدام معادلة بيتا.
4. عرض النتائج على الشاشة التسلسلية.

## النتائج
- مثال على النتائج:
  - المقاومة: 9117.77 Ω
  - درجة الحرارة: 26.37°C (79.47°F)

## الملاحظات
- تحسين الدقة باستخدام مقاومات ذات نسبة تفاوت أقل.
- اختبار الحساس في بيئات مختلفة للحصول على قراءات دقيقة.

![صورة الدائرة]


## الملفات
- [كود الأردوينو][Uploading NTC_THERMISTOR.ino…]()const int ntcPin = A0;    // المدخل التناظري الموصل بالـ NTC
const int resistor = 10000; // قيمة المقاومة الثابتة (10kΩ)
const float beta = 3950;   // قيمة بيتا الخاصة بالـ NTC
const float t0 = 298.15;   // درجة الحرارة المرجعية (25°C بالكلفن)
const int r25 = 10000;     // المقاومة عند 25°C

void setup() {
  Serial.begin(9600); // تشغيل الاتصال التسلسلي لعرض القيم
  Serial.println("Reading NTC Temperature...");
}

void loop() {
  int analogValue = analogRead(ntcPin);          // قراءة الجهد من المدخل التناظري
  float voltage = analogValue * (5.0 / 1023.0);  // تحويل القراءة إلى فولت
  float ntcResistance = (5.0 - voltage) * resistor / voltage; // حساب مقاومة الـ NTC

  // حساب درجة الحرارة باستخدام معادلة بيتا
  float tempK = 1 / ((1 / t0) + (log(ntcResistance / r25) / beta)); // بالكلفن
  float tempC = tempK - 273.15; // تحويل الكلفن إلى مئوي
  float tempF = (tempC * 9.0 / 5.0) + 32.0; // تحويل المئوي إلى فهرنهايت

  // عرض القيم في شاشة السيريال
  Serial.print("Resistance: ");
  Serial.print(ntcResistance);
  Serial.print(" Ohms, Temperature: ");
  Serial.print(tempC);
  Serial.print(" °C, ");
  Serial.print(tempF);
  Serial.println(" °F");

  delay(1000); // تأخير لمدة ثانية
}


- 
