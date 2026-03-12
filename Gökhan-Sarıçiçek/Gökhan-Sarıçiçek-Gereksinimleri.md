# Kidigo Fonksiyonel Gereksinimler Dokümanı

## 1. Kullanıcı ve Profil Yönetimi

### FR-1: Kullanıcı Kaydı
- **API Metodu:** POST /auth/register  
- **Açıklama:** Yeni bir öğrencinin sisteme kayıt olmasını sağlar. Kullanıcı adı, e-posta ve parola bilgileri alınarak sistemde yeni bir kullanıcı hesabı oluşturulur.  
- **Ön Koşul:** Kullanıcının daha önce kayıtlı olmaması gerekir.  
- **Sonuç:** Yeni kullanıcı hesabı oluşturulur ve kullanıcı sisteme giriş yapabilir.

### FR-2: Kullanıcı Giriş
- **API Metodu:** POST /auth/login  
- **Açıklama:** Kayıtlı kullanıcıların sisteme giriş yapmasını sağlar. Başarılı giriş sonrası kullanıcıya kimlik doğrulama için JWT token üretilir.  
- **Ön Koşul:** Kullanıcı sistemde kayıtlı olmalıdır.  
- **Sonuç:** Kullanıcı doğrulanır ve sisteme erişim sağlanır.

### FR-3: Profil Verilerini Görüntüleme
- **API Metodu:** GET /users/profile  
- **Açıklama:** Giriş yapmış kullanıcının profil bilgilerini görüntülemesini sağlar.  
- **Ön Koşul:** Kullanıcının sisteme giriş yapmış olması gerekir.  
- **Sonuç:** Kullanıcıya ait profil bilgileri görüntülenir.

### FR-4: Profil Güncelleme
- **API Metodu:** PUT /users/profile  
- **Açıklama:** Kullanıcının profil bilgilerini güncellemesini sağlar. Kullanıcı adı, hedeflenen sınav veya sınıf bilgileri değiştirilebilir.  
- **Ön Koşul:** Kullanıcının sisteme giriş yapmış olması gerekir.  
- **Sonuç:** Kullanıcı bilgileri güncellenir.

### FR-5: Hesap Silme
- **API Metodu:** DELETE /users/profile  
- **Açıklama:** Kullanıcının hesabını ve tüm verilerini sistemden kalıcı olarak silmesini sağlar.  
- **Ön Koşul:** Kullanıcının sisteme giriş yapmış olması gerekir.  
- **Sonuç:** Kullanıcı hesabı ve ilişkili veriler silinir.

---

## 2. Çalışma ve Pomodoro Yönetimi

### FR-6: Çalışma Seansı Başlatma
- **API Metodu:** POST /study-sessions  
- **Açıklama:** Kullanıcının yeni bir çalışma seansı başlatmasını sağlar. Seans türü Pomodoro veya serbest çalışma olarak belirlenebilir.  
- **Ön Koşul:** Kullanıcının sisteme giriş yapmış olması gerekir.  
- **Sonuç:** Yeni bir çalışma seansı başlatılır ve sistemde kayıt altına alınır.

### FR-7: Aktif Seansı Görüntüleme
- **API Metodu:** GET /study-sessions/active  
- **Açıklama:** Kullanıcının o anda aktif bir çalışma seansı olup olmadığını kontrol eder.  
- **Ön Koşul:** Kullanıcı giriş yapmış olmalıdır.  
- **Sonuç:** Aktif seans varsa bilgileri döndürülür.

### FR-8: Çalışma Süresi Güncelleme
- **API Metodu:** PATCH /study-sessions/{sessionId}  
- **Açıklama:** Tamamlanan veya durdurulan bir çalışma seansının süresini günceller.  
- **Ön Koşul:** İlgili çalışma seansı mevcut olmalıdır.  
- **Sonuç:** Seans süresi güncellenir.

### FR-9: Manuel Çalışma Ekleme
- **API Metodu:** POST /study-sessions/manual  
- **Açıklama:** Kullanıcının geçmişte yaptığı bir çalışmayı manuel olarak sisteme eklemesini sağlar.  
- **Ön Koşul:** Kullanıcı giriş yapmış olmalıdır.  
- **Sonuç:** Yeni bir çalışma kaydı oluşturulur.

### FR-10: Çalışma Kaydını Silme
- **API Metodu:** DELETE /study-sessions/{sessionId}  
- **Açıklama:** Yanlış girilen veya gereksiz bir çalışma kaydını sistemden siler.  
- **Ön Koşul:** İlgili çalışma kaydı mevcut olmalıdır.  
- **Sonuç:** Çalışma kaydı sistemden kaldırılır.

---

## 3. Hedef ve Kategori Yönetimi

### FR-11: Yeni Hedef Belirleme
- **API Metodu:** POST /goals  
- **Açıklama:** Kullanıcının günlük veya haftalık çalışma hedefi oluşturmasını sağlar.  
- **Ön Koşul:** Kullanıcı giriş yapmış olmalıdır.  
- **Sonuç:** Yeni hedef kaydı oluşturulur.

### FR-12: Hedef Güncelleme
- **API Metodu:** PATCH /goals/{goalId}  
- **Açıklama:** Mevcut hedeflerin güncellenmesini sağlar.  
- **Ön Koşul:** İlgili hedef mevcut olmalıdır.  
- **Sonuç:** Hedef bilgileri güncellenir.

### FR-13: Ders/Kategori Ekleme
- **API Metodu:** POST /subjects  
- **Açıklama:** Kullanıcının kendi ders veya kategori listesini oluşturmasını sağlar.  
- **Ön Koşul:** Kullanıcı giriş yapmış olmalıdır.  
- **Sonuç:** Yeni ders veya kategori sisteme eklenir.

### FR-14: Ders Listesini Görüntüleme
- **API Metodu:** GET /subjects  
- **Açıklama:** Kullanıcının tanımladığı tüm ders veya kategorileri listeler.  
- **Ön Koşul:** Kullanıcı giriş yapmış olmalıdır.  
- **Sonuç:** Ders listesi döndürülür.

### FR-15: Ders Güncelleme
- **API Metodu:** PUT /subjects/{subjectId}  
- **Açıklama:** Mevcut bir dersin adını veya bilgilerini düzenler.  
- **Ön Koşul:** İlgili ders mevcut olmalıdır.  
- **Sonuç:** Ders bilgileri güncellenir.

### FR-16: Ders Silme
- **API Metodu:** DELETE /subjects/{subjectId}  
- **Açıklama:** Kullanıcının tanımladığı bir dersi sistemden kaldırmasını sağlar.  
- **Ön Koşul:** Ders sistemde mevcut olmalıdır.  
- **Sonuç:** Ders kaydı silinir.

---

## 4. İstatistik ve Analiz (Dashboard)

### FR-17: Genel Performans Özeti
- **API Metodu:** GET /statistics/summary  
- **Açıklama:** Kullanıcının günlük veya haftalık toplam çalışma sürelerini getirir.  
- **Sonuç:** Kullanıcıya performans özeti gösterilir.

### FR-18: Ders Bazlı Dağılım
- **API Metodu:** GET /statistics/subjects  
- **Açıklama:** Kullanıcının ders bazında çalışma sürelerini getirir.  
- **Sonuç:** Grafik oluşturmak için veri sağlanır.
