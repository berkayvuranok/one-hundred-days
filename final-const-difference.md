# Dart dilinde `final` ve `const` Farkları

Dart dilinde `final` ve `const` birbirine çok karıştırılır çünkü ikisi de değişkenin değerinin sonradan değiştirilemeyeceğini (immutable) garanti eder.

Ancak aralarındaki temel fark **"zamanlama"** (değerin ne zaman bilindiği) ile ilgilidir.

İşte detaylı ayrım:

---

## ⚔️ `final` vs `const`

### 1. Temel Fark: Zamanlama (Timing)

| Özellik | `const` (Constant) | `final` |
|---------|-------------------|---------|
| **Zaman** | Compile-time (Derleme Zamanı) | Run-time (Çalışma Zamanı) |
| **Anlamı** | Değer, kod daha çalışmadan yazıldığı anda kesinlikle bilinmelidir. | Değer, kod çalışırken (örneğin bir kullanıcı butona bastığında) hesaplanıp atanabilir. |
| **Bellek** | Aynı değer bellekte sadece bir kere oluşturulur (Canonicalized). | Her kullanımda bellekte yeni yer ayrılabilir. |

---

### 2. `final` (Nihai Karar)

`final`, "değeri bir kez atayacağım ve bir daha değiştirmeyeceğim" demektir. Ancak bu değeri atamak için programın çalışmasını bekleyebilirsin.

**Ne zaman kullanılır?** Değer veritabanından, internetten, kullanıcı girdisinden veya `DateTime.now()` gibi o anki zamandan geliyorsa.

**Kural:** Atama yapıldıktan sonra `set` edilemez.
```dart
// ✅ DOĞRU: Şu anki zaman kod çalışınca belli olur.
final timeNow = DateTime.now(); 

// ✅ DOĞRU: Kullanıcıdan gelen input run-time'da belli olur.
final username = getUserInput(); 

// ❌ YANLIŞ: Değeri sonradan değiştiremezsin.
timeNow = DateTime.now(); // Hata!
```

---

### 3. `const` (Sabit Değer)

`const`, "bu değer evrensel bir sabittir, program çalışmadan önce bile bellidir" demektir. Bilgisayar bu değeri kod derlenirken (compile) yerine koyar.

**Ne zaman kullanılır?** Pi sayısı, sabit renk kodları, sabit API url'leri, matematiksel sabitler.

**Kural:** Değer tamamen "hard-coded" (elle yazılmış) veya başka `const` değerlerden oluşmalıdır.
```dart
// ✅ DOĞRU: Pi sayısı derleme zamanında bellidir.
const pi = 3.14159;

// ✅ DOĞRU: İki sabit sayının toplamı da sabittir.
const area = pi * 10 * 10; 

// ❌ YANLIŞ: DateTime.now() çalışma zamanında değişir, const olamaz.
const time = DateTime.now(); // Hata!
```

---

### 4. Kritik Detay: Listeler ve Nesneler (Collections)

Bu kısım mülakatlarda sıkça sorulur. `final` bir listenin içeriği değişebilir, `const` bir listenin içeriği değişemez.

#### A) `final` Liste

Kutu kilitlidir (başka liste atayamazsın), ama kutunun içindeki eşyaları değiştirebilirsin.
```dart
final List<String> names = ['Ali', 'Veli'];

names.add('Ayşe'); // ✅ ÇALIŞIR: İçerik değişebilir.
names[0] = 'Mehmet'; // ✅ ÇALIŞIR: Eleman değişebilir.

names = ['Zeynep']; // ❌ HATA: names değişkenine YENİ bir liste atayamazsın.
```

#### B) `const` Liste

Kutu kilitlidir VE kutunun içindeki eşyalar dondurulmuştur (Frozen).
```dart
const List<String> names = ['Ali', 'Veli'];

names.add('Ayşe'); // ❌ HATA: Runtime'da "Unsupported operation" hatası alırsın.
names[0] = 'Mehmet'; // ❌ HATA: İçerik değiştirilemez.
```

---

### 5. Flutter İçin Neden Önemli?

Flutter'da `const` kullanımı performans için kritiktir.

- Eğer bir Widget'ı `const` ile tanımlarsan (Örn: `const Text('Merhaba')`), Flutter bu widget'ı bellekte sadece bir kez oluşturur.
- Ekran her yenilendiğinde (setState olduğunda), Flutter `const` olan widget'ları yeniden çizmez (rebuild etmez). Olduğu gibi kullanır. Bu da FPS artışı sağlar.
```dart
// Build methodu her tetiklendiğinde:
return Column(
  children: [
    const Text("Başlık"), // ✅ Bellekte 1 kere üretilir, tekrar tekrar kullanılır.
    Text(degiskenBaslik), // ⚠️ Her build işleminde yeniden üretilir.
  ],
);
```

---

## 📊 Özet Karar Tablosu

| Durum | Hangi Anahtar Kelime? |
|-------|----------------------|
| Değer kod yazılırken belliyse (Pi sayısı, Renkler) | `const` |
| Değer program çalışınca belli olacaksa (Saat, API verisi) | `final` |
| Bir listenin içeriği asla değişmesin istiyorsan | `const List` |
| Değişkenin referansı değişmesin ama içi değişebilsin | `final List` |
| Widget'ın durumu hiç değişmeyecekse (Statik UI) | `const Widget` |