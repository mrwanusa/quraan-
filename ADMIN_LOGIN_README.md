# نظام تسجيل الدخول الإداري

تم إنشاء نظام تسجيل دخول آمن للوصول إلى لوحة التحكم الإدارية.

## بيانات الدخول

- **البريد الإلكتروني**: `admin@gmail.com`
- **كلمة المرور**: `admin`

## الوصول إلى النظام

### 1. صفحة تسجيل الدخول
- الرابط المباشر: `/admin/login`
- تحتوي على نموذج تسجيل دخول آمن
- عرض بيانات تجريبية للمساعدة
- إعادة توجيه تلقائية إذا كان المستخدم مسجل دخول بالفعل

### 2. الوصول للإدارة
- عند محاولة الوصول لـ `/admin` بدون تسجيل دخول، سيتم توجيهك لصفحة تسجيل الدخول
- بعد تسجيل الدخول بنجاح، ستتم إعادة التوجيه للوحة التحكم

## الميزات الأمنية

### 1. الحماية (Protected Routes)
- جميع صفحات الإدارة محمية بنظام المصادقة
- إعادة توجيه تلقائية لصفحة تسجيل الدخول للمستخدمين غير المسجلين

### 2. إدارة الجلسة
- حفظ بيانات المستخدم في `localStorage`
- التحقق من صحة الجلسة عند كل وصول
- تنظيف البيانات التالفة تلقائياً

### 3. واجهة المستخدم
- عرض معلومات المستخدم المسجل في الداشبورد
- زر تسجيل خروج واضح ومتاح
- رسائل تأكيد لعمليات تسجيل الدخول والخروج

## الصفحات المحمية

جميع الصفحات التالية تتطلب تسجيل دخول:
- `/admin` - لوحة التحكم الرئيسية
- `/admin/hadiths` - إدارة الأحاديث
- `/admin/prophets` - إدارة قصص الأنبياء
- `/admin/adhkar` - إدارة الأذكار
- `/admin/reciters` - إدارة القراء

## قاعدة البيانات

تم إنشاء جدول `admin_users` في قاعدة البيانات:
```sql
CREATE TABLE admin_users (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role VARCHAR(50) DEFAULT 'admin',
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
```

## الملفات المضافة

### 1. صفحات جديدة
- `src/pages/admin/AdminLoginPage.tsx` - صفحة تسجيل الدخول

### 2. مكونات جديدة
- `src/components/ProtectedRoute.tsx` - حماية المسارات
- `src/components/AdminRedirect.tsx` - إعادة توجيه الإدارة

### 3. أدوات مساعدة
- `src/utils/auth.ts` - دوال المصادقة

### 4. تحديثات
- `src/App.tsx` - إضافة المسارات المحمية
- `src/pages/admin/AdminDashboard.tsx` - إضافة معلومات المستخدم وزر الخروج

## كيفية الاستخدام

1. **الوصول لصفحة تسجيل الدخول**:
   ```
   http://localhost:5173/admin/login
   ```

2. **إدخال بيانات الدخول**:
   - البريد: admin@gmail.com
   - كلمة المرور: admin

3. **الوصول للوحة التحكم**:
   - سيتم توجيهك تلقائياً لـ `/admin`
   - ستظهر معلومات المستخدم في الأعلى
   - يمكن تسجيل الخروج من زر "خروج"

## الأمان

⚠️ **ملاحظة مهمة**: في البيئة الإنتاجية يجب:
- تشفير كلمات المرور باستخدام bcrypt
- استخدام JWT أو session tokens
- إضافة CSRF protection
- تطبيق rate limiting
- استخدام HTTPS

## التطوير المستقبلي

يمكن إضافة الميزات التالية:
- إدارة متعددة المستخدمين
- أدوار وصلاحيات مختلفة
- إعادة تعيين كلمة المرور
- تسجيل العمليات (Logging)
- تنبيهات الأمان 