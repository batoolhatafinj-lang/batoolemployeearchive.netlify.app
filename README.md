# أرشيف شعبة تكنولوجيا المعلومات

تطبيق ASP.NET Core MVC بـ Razor Views، وقاعدة PostgreSQL مع EF Core وNpgsql وMigrations.

## المتطلبات

- .NET SDK 10.0
- PostgreSQL 17 أو أحدث
- قاعدة بيانات PostgreSQL ومستخدم يملك صلاحية إنشاء الجداول

## إعداد قاعدة البيانات

عدّل `ConnectionStrings:DefaultConnection` في `EmployeeArchive/appsettings.json` (يوجد مثال في `EmployeeArchive/appsettings.Example.json`) أو عيّن متغير البيئة `ConnectionStrings__DefaultConnection`، ثم من مجلد `EmployeeArchive`:

```powershell
dotnet tool restore
dotnet ef database update
```

ملفات الـ Migration موجودة في `EmployeeArchive/Data/Migrations`. لتطبيق التحديثات تلقائياً عند التشغيل، عيّن `Database__ApplyMigrationsOnStartup=true`.

## التشغيل

```powershell
dotnet run --project EmployeeArchive/EmployeeArchive.csproj
```

حساب مسؤول التهيئة محلياً: `admin` / `12345`. غيّر `Admin:Password` أو عيّن `Admin__Password` قبل تشغيل التطبيق في بيئة غير تطويرية؛ التطبيق يرفض كلمة المرور الافتراضية في الإنتاج. الحسابات التي ينشئها المسؤول للموظفين تُخزّن كلمات مرورها باستخدام BCrypt.

## الاستخدام

1. سجّل دخول المسؤول، ثم أضف الموظفين مع بياناتهم واسم المستخدم وكلمة مرور أولية.
2. يفتح كل موظف حسابه ليشاهد بياناته وأقسام الأرشيف الأربعة.
3. يرفع الموظف ملفاً رئيسياً بصيغة PDF أو صورة، ويمكنه إرفاق أربع صور إضافية اختيارية عند اختيار صورة رئيسية. الحد الأعلى 15 ميغابايت لكل ملف.

يمكن للمسؤول استيراد بيانات الموظفين من صفحة **استيراد من Excel**. استخدم قالب `.xlsx` المتاح فيها، وأدخل كلمة مرور أولية لا تقل عن 8 أحرف أو أرقام لكل موظف. يعرض النظام معاينة قبل الحفظ ويتجاوز الصفوف الناقصة أو أسماء المستخدمين المكررة؛ كلمات المرور تُحفظ مجزأة باستخدام BCrypt ولا تظهر في المعاينة. احذف نسخة ملف Excel التي تحتوي كلمات المرور بعد الاستيراد.

تُخزّن الملفات في `EmployeeArchive/App_Data` افتراضياً. يمكن نقلها إلى مجلد دائم خارج مجلد النشر بتعيين `FileStorage__RootPath`. تنزيل الكتب وصور الموظفين يمر عبر نقاط محمية بتسجيل الدخول.

## إصدارات الحزم

المشروع يستهدف .NET 10 LTS، ويستخدم إصدارات 10.0 متوافقة من ASP.NET Core Authentication وEF Core وNpgsql. Bootstrap RTL مضمّن محلياً ضمن `wwwroot/lib`.
