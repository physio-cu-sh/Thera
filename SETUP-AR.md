# إعداد موقع PTCU Topic Assignment

**مشروع Supabase:** `PTCU-Topic-Booking`  
**Dashboard:** https://supabase.com/dashboard/project/wauycitelklrfxyebdgf  
**رابط API:** `https://wauycitelklrfxyebdgf.supabase.co`

## قاعدة البيانات
قاعدة البيانات أُنشئت بالفعل وزُرع فيها 34 اسم طالب و31 Topic. الجداول: `ptcu_students`, `ptcu_topics`, `ptcu_bookings`, `ptcu_admins`، وسياسات RLS مفعّلة.

## تجهيز حساب الإدارة (لازم مرة واحدة)
1. افتح [لوحة المشروع](https://supabase.com/dashboard/project/wauycitelklrfxyebdgf).
2. افتح **Authentication → Users → Add user**.
3. أنشئ حسابًا بالبريد `mahmoudshamroukh90@gmail.com` وحدد كلمة مرور جديدة قوية، وأكّد البريد من لوحة التحكم إذا لزم. لا تستخدم كلمة مرور سبق كتابتها في المحادثة؛ اعتبرها مكشوفة.
4. افتح **SQL Editor** والصق وشغّل:

```sql
insert into public.ptcu_admins (user_id, email)
select id, email
from auth.users
where lower(email) = lower('mahmoudshamroukh90@gmail.com')
on conflict (user_id) do update set email = excluded.email;
```

5. افتح `index.html` واضغط «لوحة الإدارة» وسجّل الدخول بالحساب الذي أنشأته. لوحة الإدارة تعرض الحجوزات وتتيح إلغاء أي حجز.

## تشغيل ونشر الموقع
- فك ضغط `PTCU-Topic-Booking.zip`.
- جرّب `index.html` في متصفح متصل بالإنترنت. إذا واجه المتصفح قيودًا عند فتح الملف مباشرة، ارفع الملف إلى استضافة static مثل Netlify أو GitHub Pages.
- بعد النشر، افتح الرابط المنشور على الهاتف والكمبيوتر وجرّب الحجز والإلغاء.

## خصائص
- عرض Upper limb وLower limb.
- البحث باسم التوبيك أو الطالب، والتصفية حسب القسم وحالة الحجز.
- اسم الطالب يظهر تحت التوبيك المحجوز.
- كل طالب له حجز واحد، وكل Topic يحجزه طالب واحد؛ قاعدة البيانات تمنع التكرار المتزامن.
- لوحة إدارة لتسجيل الدخول وإلغاء الحجوزات.

## ملاحظة أمان مهمة
الموقع يتيح اختيار اسم الطالب من قائمة، ولا يتحقق من هوية الطالب الفعلية. لذلك لا تعتبر الاسم المختار إثباتًا للهوية. لو محتاج منع انتحال الأسماء، نحتاج تسجيل دخول منفصل للطلاب أو رمز تحقق لكل طالب. لا تضع أبدًا `service_role` أو أي secret key داخل `index.html`؛ المفتاح العام الموجود فيه مخصص للاستخدام في المتصفح.
