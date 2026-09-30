# Incident Diagnosis (Olay Teşhisi): YerimVar Vakası Analizi

YerimVar vakasındaki kırılmalar kod hatalarından ziyade mühendislik süreçlerindeki eksikliklerden kaynaklanmıştır. Sunulan vaka zaman çizelgesi (YerimVar Incident Timeline) incelenerek 3 kritik kopma noktası (breakpoint) belirlenmiş, süreç boşlukları ve eksik mühendislik kanıtları tanımlanmıştır.

## Breakpoint 1: İki Farklı Gerçeklik ve Durum (State) Yönetiminin Çökmesi (Two Realities)

### What happened? (Ne oldu?)

Deniz ve Ege'nin ekranlarında aynı sınıf (Sınıf 204) için farklı durumlar gösterilmiştir. Deniz'in ekranında sınıf "DOLU", Ege'nin ekranında ise "BOŞ" olarak görünmüştür.

Bu durum, sistemdeki senkronizasyon ve durum (state) yönetiminin düzgün çalışmadığını göstermektedir.

### Process Gap (Süreç Boşluğu)

- **Eşzamanlılık (Concurrency) ve Senkronizasyon Yönetimi Eksikliği:** Birden fazla kullanıcının aynı kaynağa erişimini ve durum güncellemelerini eşzamanlı olarak yöneten merkezi bir durum yönetimi mimarisi tasarlanmamıştır.
- **Gerçek Zamanlı Veri Doğrulama Süreç Boşluğu:** İstemciler (client) arasındaki durum senkronizasyonunu kontrol eden ve doğrulayan entegrasyon testleri yapılmamıştır.

### Missing Evidence (Eksik Kanıt)

- Sınıf rezervasyon durumlarının eşzamanlı güncellendiğini doğrulayan entegrasyon test raporları (Concurrency Test Logs).
- Durum yönetimi ve senkronizasyon mimarisini açıklayan teknik tasarım dokümanı (State Management Architecture Document).

---

## Breakpoint 2: Ortam (Environment) Bağımlılığı ve "Benim Bilgisayarımda Çalışıyordu" Yanılgısı (Environment Failure & Localhost)

### What happened? (Ne oldu?)

Uygulama çalıştırılmak istendiğinde `ModuleNotFoundError: No module named 'flask'` hatası alınmıştır.

Bu durum, ürünün yalnızca Deniz'in bilgisayarında çalıştığını ve proje bağımlılıklarının ve çalışma ortamının yeterince yönetilmediğini göstermiştir.

Ayrıca canlı sunucu yerine `localhost:8000` adresi üzerinden dağıtım yapılmaya çalışılmıştır.

### Process Gap (Süreç Boşluğu)

- **Bağımlılık ve Ortam Yönetimi (Dependency Management) Eksikliği:** Proje bağımlılıklarını izole eden ve standartlaştıran yapılandırma dosyaları (`requirements.txt`, `Dockerfile` vb.) ve ortam yönetimi disiplini uygulanmamıştır.
- **Sürekli Entegrasyon ve Dağıtım (CI/CD) Süreç Boşluğu:** Kodun farklı bilgisayarlarda veya canlı sunucuda tutarlı şekilde çalışmasını sağlayacak otomatik build, test ve deployment süreçleri kurulmamıştır.

### Missing Evidence (Eksik Kanıt)

- Sürüm bağımlılıklarını sabitleyen konfigürasyon dosyaları (`requirements.txt` / `Dockerfile`).
- Otomatik derleme, test ve yayınlama sürecini doğrulayan CI/CD pipeline çalışma ve test logları.

---

## Breakpoint 3: Yapay Zeka Kodunun Sahiplenilememesi ve Açıklanamaması (Unexplained AI Code)

### What happened? (Ne oldu?)

Kod içerisindeki kritik algoritmaya (`optimize_reservation`) "// YZ yazdı. Çalışır gibi. Dokunmayın." şeklinde bir yorum eklenmiştir.

Bu durum, kodun neden bu şekilde tasarlandığının ekip tarafından yeterince anlaşılmadığını ve kodun mühendislik açısından sahiplenilmediğini göstermektedir.

### Process Gap (Süreç Boşluğu)

- **Kod İnceleme (Code Review) ve Sahiplik Süreç Eksikliği:** Üretilen kodun takımdaki mühendisler tarafından anlaşıldığını, doğrulandığını ve sahiplenildiğini garanti eden peer-review süreçleri işletilmemiştir.
- **Mimari Karar Kaydı (ADR) Eksikliği:** Algoritmanın neden bu şekilde tasarlandığını ve kararların gerekçesini açıklayan dokümantasyon (Context, Alternatives, Decision, Rationale) tutulmamıştır.

### Missing Evidence (Eksik Kanıt)

- Kodun insan tarafından incelendiğini ve onaylandığını gösteren Pull Request (PR) Code Review kayıtları.
- Algoritma tasarım kararlarını ve gerekçelerini açıklayan Mimari Karar Kaydı (Architecture Decision Record - ADR) dokümanı.

---

## AI Usage Record (AI Kullanım Kaydı)

- **AI Tool(s) (AI Aracı):** ChatGPT / Gemini
- **AI Role (AI Rolü):** Drafting / Structuring / Reviewing (Taslak oluşturma / Yapılandırma / İnceleme)
- **Human Review (İnsan İncelemesi):** Completed (Tamamlandı)
- **Final Decision (Son Karar):** Team (Takım)
