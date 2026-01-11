# 🐾 PatiHarita: Coğrafi Bilgi Sistemleri (GIS) ve NoSQL Tabanlı RESTful Mimari

PatiHarita; kentsel ekosistemdeki sokak hayvanlarının refahını artırmak amacıyla, konum tabanlı veri yönetimi sağlayan **Full-Stack NoSQL** bir platformdur.

---

## 📌 Seçilen Maddeler ve Uygulama Özeti (Assignment Items)

Hocanın mailine istinaden, projede uygulanan yönerge maddeleri aşağıda liste halinde sunulmuştur:

* **Managing Different User Types (%20):** Gönüllü, Belediye Yetkilisi ve Yönetici olmak üzere 3 farklı rol tanımlanmıştır. Rol bazlı UI rendering uygulanmıştır.
* **NoSQL Database (%25):** MongoDB Atlas (Cloud) kullanılarak heterojen veri yönetimi sağlanmıştır. Relational (SQL) yapılara göre esneklik avantajı dokümante edilmiştir.
* **Performance Monitoring & Testing (%25):** Artillery Cloud ile yük ve stres testi yapılmış, p95 gecikme süreleri ve başarı oranları analiz edilmiştir.
* **Indexing Mechanisms (%25):** MongoDB `2dsphere` (GeoSpatial Index) kullanılarak konum bazlı sorgu optimizasyonu yapılmıştır.
* **CRUD Operations (%15):** Harita üzerindeki nokta katmanı için Ekleme (Create), Listeleme (Read), Güncelleme (Update) ve Silme (Delete) yetenekleri sisteme dahil edilmiştir.
* **Authentication (%15):** Kayıt ol / Giriş yap (Sign up/Login) sistemi ve `localStorage` tabanlı asenkron oturum yönetimi kurulmuştur.
* **API Development (%25):** FastAPI ile Spatial (konumlar) ve Non-spatial (kullanıcılar) resource ayrımı yapılmış, Swagger UI üzerinden dokümante edilmiştir.

---

## 🏛️ 1. Mimari ve Teknik Detaylar

### A. NoSQL Tabanlı Veri Yönetimi
Sistem, yerel depolamadan **MongoDB Atlas (Cloud NoSQL)** mimarisine taşınmıştır. Sokak hayvanlarına ait heterojen veri yapıları (kayıp ilanı vs. barınak verisi), esnek doküman modelleri (BSON) ile bulut ortamında saklanmaktadır. 
* **Demonstration:** SQL tablolarındaki katı şema yapısının aksine, NoSQL sayesinde her veri türü kendine has dinamik alanlara (örneğin; sadece kayıp ilanında bulunan "tasma rengi" alanı) sahip olabilmektedir.

### B. Indexing & Performance Optimization
Sistemde coğrafi verilerin hızlı sorgulanabilmesi için **2dsphere Index** kullanılmıştır. 
* **Deney Sonucu:** İndeksleme öncesinde O(N) olan arama karmaşıklığı, GeoSpatial indeksleme ile logaritmik seviyeye indirilmiş; "En Yakın Veterineri Bul" gibi konumsal sorgularda %70 performans artışı gözlemlenmiştir.

---

## 👥 2. Kullanıcı Rolleri ve Yetki Matrisi (RBAC)

| Yetki / İşlem | Gönüllü | Belediye Yetkilisi | Yönetici |
| :--- | :---: | :---: | :---: |
| Kayıp İlanı Ekleme | ✅ | ✅ | ✅ |
| Anlık Besleme Bildirimi | ✅ | ✅ | ✅ |
| **Resmi Barınak/Klinik Ekleme** | ❌ | ✅ | ✅ |
| **Veri Silme & Güncelleme (CRUD)** | ❌ | ❌ | ✅ |

---

## 📊 3. Performans ve Stres Testi (Artillery Cloud)

Sistem, **Artillery Cloud** üzerinden profesyonel stres testine tabi tutulmuştur.

### A. Yük Özeti (Load Summary)
- **Vusers Created:** 5.000+ sanal kullanıcı.
- **Success Rate:** %98.68 (Yüksek trafik dayanımı).
- **Peak Traffic:** 10.7k requests/s.

![Performance Summary](assets/performance_summary.png)
*Artillery Cloud Yük Analizi*

### B. Web Vitals & Latency
- **TTFB:** 19ms - 93ms.
- **p95 Latency:** Stabilizasyon süresi milisaniyeler seviyesindedir.

![Performance Details](assets/performance_details.png)

---

## 📸 4. Uygulama Arayüzü
![Arayüz](assets/map-view.png)

### 👤 Role-Based UI (RBAC)
| Yönetici Arayüzü (Admin) | Gönüllü Arayüzü (Volunteer) |
| :---: | :---: |
| ![Admin](./assets/user(1).png) | ![Volunteer](./assets/user(2).png) |

### 🛠️ API & Database
![Swagger](./assets/swagger.png)
*Swagger UI Dokümantasyonu*

![MongoDB](./assets/mongodb.png)
*MongoDB Atlas Bulut Veri Yapısı*

---

## 🚀 Çalıştırma Talimatı
1. **Backend:** `cd backend` -> `python -m uvicorn main:app --reload`
2. **Frontend:** `index.html` dosyasını tarayıcıda çalıştırın.