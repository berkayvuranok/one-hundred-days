# ⚡ Dart Asynchronous Programming (Asenkron Programlama)

## 🎯 Asenkron Programlama Nedir?

**Amaç:** Uzun süren işlemleri (API çağrıları, dosya okuma, veritabanı sorguları) beklerken uygulamanın donmasını engellemek.

### 🧩 Senkron vs Asenkron

**Senkron (Synchronous):** Kodlar sırayla çalışır, bir işlem bitene kadar diğeri başlamaz.
```dart
// ❌ SENKRON - Uygulama 3 saniye boyunca donar
void main() {
  print('Başladı');
  sleep(Duration(seconds: 3)); // 3 saniye bekle
  print('Bitti');
}
```

**Asenkron (Asynchronous):** İşlemler paralel çalışır, beklerken diğer kodlar çalışmaya devam eder.
```dart
// ✅ ASENKRON - Uygulama donmaz
void main() async {
  print('Başladı');
  await Future.delayed(Duration(seconds: 3)); // Arka planda bekle
  print('Bitti');
}
```

---

## 1️⃣ Future (Gelecekteki Değer)

**Amaç:** Gelecekte tamamlanacak bir işlemin sonucunu temsil eder.

### ✔️ Temel Future Kullanımı
```dart
// Future döndüren bir fonksiyon
Future<String> fetchUserData() {
  return Future.delayed(
    Duration(seconds: 2),
    () => 'Kullanıcı Verisi',
  );
}

void main() {
  print('Veri istendi');
  
  fetchUserData().then((data) {
    print('Gelen veri: $data');
  });
  
  print('Diğer işlemler devam ediyor');
}

// Çıktı sırası:
// Veri istendi
// Diğer işlemler devam ediyor
// Gelen veri: Kullanıcı Verisi (2 saniye sonra)
```

### ✔️ async / await Kullanımı
```dart
// async: Bu fonksiyon asenkron çalışır
// await: Bu satırda bekle, sonuç gelene kadar devam etme
Future<void> getUserData() async {
  print('Kullanıcı verisi yükleniyor...');
  
  String data = await fetchUserData(); // Burada bekle
  
  print('Gelen veri: $data');
  print('İşlem tamamlandı');
}

void main() async {
  await getUserData();
}
```

### ✔️ Hata Yakalama (Error Handling)
```dart
Future<String> fetchData() async {
  await Future.delayed(Duration(seconds: 1));
  throw Exception('Sunucu hatası!');
}

// 1. Yöntem: try-catch
Future<void> loadData() async {
  try {
    String data = await fetchData();
    print(data);
  } catch (e) {
    print('Hata: $e');
  } finally {
    print('İşlem tamamlandı (başarılı ya da hatalı)');
  }
}

// 2. Yöntem: then() ve catchError()
void loadDataWithThen() {
  fetchData()
    .then((data) => print(data))
    .catchError((error) => print('Hata: $error'))
    .whenComplete(() => print('İşlem tamamlandı'));
}
```

---

## 2️⃣ Multiple Futures (Çoklu İşlemler)

**Amaç:** Birden fazla asenkron işlemi yönetmek.

### ✔️ Future.wait (Tümünü Bekle)
```dart
Future<String> fetchUser() async {
  await Future.delayed(Duration(seconds: 2));
  return 'Kullanıcı Bilgisi';
}

Future<String> fetchPosts() async {
  await Future.delayed(Duration(seconds: 3));
  return 'Gönderiler';
}

Future<String> fetchComments() async {
  await Future.delayed(Duration(seconds: 1));
  return 'Yorumlar';
}

// ✅ Paralel çalıştır, hepsini bekle (3 saniye sürer, 6 değil!)
Future<void> loadAllData() async {
  print('Veriler yükleniyor...');
  
  List<String> results = await Future.wait([
    fetchUser(),
    fetchPosts(),
    fetchComments(),
  ]);
  
  print('Kullanıcı: ${results[0]}');
  print('Gönderiler: ${results[1]}');
  print('Yorumlar: ${results[2]}');
}
```

### ✔️ Future.any (İlk Tamamlananı Al)
```dart
Future<void> getFastestServer() async {
  String result = await Future.any([
    fetchFromServer1(), // 5 saniye
    fetchFromServer2(), // 2 saniye ← Bu kazanır
    fetchFromServer3(), // 4 saniye
  ]);
  
  print('En hızlı sunucudan gelen: $result');
}
```

---

## 3️⃣ Stream (Veri Akışı)

**Amaç:** Zamanla gelen çoklu veriyi işlemek (Future: tek veri, Stream: sürekli veri).

### 🧩 Future vs Stream

| Özellik | **Future** | **Stream** |
|---------|-----------|-----------|
| **Veri Sayısı** | Tek (single value) | Çoklu (multiple values) |
| **Kullanım** | Bir kez sonuç döner | Sürekli veri akışı |
| **Örnek** | API'den kullanıcı bilgisi çekme | Canlı konum takibi, chat mesajları |

### ✔️ Temel Stream Kullanımı
```dart
// Stream oluşturma
Stream<int> countStream() async* {
  for (int i = 1; i <= 5; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i; // Her saniye bir sayı gönder
  }
}

// Stream dinleme
void main() async {
  await for (int value in countStream()) {
    print('Gelen değer: $value');
  }
}

// Çıktı (her biri 1 saniye arayla):
// Gelen değer: 1
// Gelen değer: 2
// Gelen değer: 3
// Gelen değer: 4
// Gelen değer: 5
```

### ✔️ Stream Controller (Manuel Kontrol)
```dart
import 'dart:async';

void main() {
  // StreamController oluştur
  final controller = StreamController<String>();
  
  // Stream'i dinle
  controller.stream.listen(
    (data) => print('Gelen mesaj: $data'),
    onError: (error) => print('Hata: $error'),
    onDone: () => print('Stream kapandı'),
  );
  
  // Veri gönder
  controller.sink.add('Merhaba');
  controller.sink.add('Nasılsın?');
  controller.sink.add('Hoşçakal');
  
  // Stream'i kapat
  controller.close();
}
```

### ✔️ Stream Dönüşümleri
```dart
Stream<int> numberStream() async* {
  for (int i = 1; i <= 5; i++) {
    await Future.delayed(Duration(milliseconds: 500));
    yield i;
  }
}

void main() async {
  // map: Değerleri dönüştür
  await for (var doubled in numberStream().map((n) => n * 2)) {
    print('İkiye katlanmış: $doubled');
  }
  
  // where: Filtrele
  await for (var even in numberStream().where((n) => n % 2 == 0)) {
    print('Çift sayı: $even');
  }
  
  // take: İlk N elemanı al
  await for (var first in numberStream().take(3)) {
    print('İlk 3 eleman: $first');
  }
}
```

---

## 4️⃣ Broadcast Stream (Çoklu Dinleyici)

**Amaç:** Aynı stream'i birden fazla yerde dinlemek.

### 🧩 Single vs Broadcast Stream
```dart
// ❌ Single Subscription Stream - Sadece 1 dinleyici
Stream<int> singleStream() async* {
  yield 1;
  yield 2;
  yield 3;
}

void main() {
  var stream = singleStream();
  stream.listen((data) => print('Dinleyici 1: $data'));
  stream.listen((data) => print('Dinleyici 2: $data')); // ❌ HATA!
}

// ✅ Broadcast Stream - Çoklu dinleyici
void mainBroadcast() {
  var controller = StreamController<int>.broadcast();
  
  controller.stream.listen((data) => print('Dinleyici 1: $data'));
  controller.stream.listen((data) => print('Dinleyici 2: $data'));
  
  controller.sink.add(1);
  controller.sink.add(2);
  
  controller.close();
}
```

---

## 5️⃣ Real-World Örnekler

### ✔️ API Çağrısı (HTTP Request)
```dart
import 'dart:convert';
import 'package:http/http.dart' as http;

// Model class
class User {
  final int id;
  final String name;
  final String email;
  
  User({required this.id, required this.name, required this.email});
  
  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      id: json['id'],
      name: json['name'],
      email: json['email'],
    );
  }
}

// API Service
class UserService {
  Future<User> fetchUser(int userId) async {
    try {
      final response = await http.get(
        Uri.parse('https://jsonplaceholder.typicode.com/users/$userId'),
      );
      
      if (response.statusCode == 200) {
        return User.fromJson(jsonDecode(response.body));
      } else {
        throw Exception('Kullanıcı bulunamadı');
      }
    } catch (e) {
      throw Exception('Bağlantı hatası: $e');
    }
  }
  
  // Birden fazla kullanıcı çek
  Future<List<User>> fetchMultipleUsers(List<int> userIds) async {
    List<Future<User>> futures = userIds.map((id) => fetchUser(id)).toList();
    return await Future.wait(futures);
  }
}

// Kullanım
void main() async {
  var service = UserService();
  
  // Tek kullanıcı
  try {
    User user = await service.fetchUser(1);
    print('${user.name} - ${user.email}');
  } catch (e) {
    print('Hata: $e');
  }
  
  // Çoklu kullanıcı
  try {
    List<User> users = await service.fetchMultipleUsers([1, 2, 3]);
    users.forEach((user) => print(user.name));
  } catch (e) {
    print('Hata: $e');
  }
}
```

### ✔️ Timeout (Zaman Aşımı)
```dart
Future<String> fetchDataWithTimeout() async {
  try {
    return await fetchData().timeout(
      Duration(seconds: 5),
      onTimeout: () {
        throw Exception('İstek zaman aşımına uğradı');
      },
    );
  } catch (e) {
    return 'Hata: $e';
  }
}
```

### ✔️ Retry Mechanism (Yeniden Deneme)
```dart
Future<String> fetchWithRetry({int maxAttempts = 3}) async {
  int attempts = 0;
  
  while (attempts < maxAttempts) {
    try {
      return await fetchData();
    } catch (e) {
      attempts++;
      if (attempts >= maxAttempts) {
        throw Exception('$maxAttempts deneme sonrası başarısız: $e');
      }
      await Future.delayed(Duration(seconds: 2)); // 2 saniye bekle
      print('Yeniden deneniyor... ($attempts/$maxAttempts)');
    }
  }
  
  throw Exception('Beklenmeyen hata');
}
```

### ✔️ Debounce (Arama Kutusu Optimizasyonu)
```dart
import 'dart:async';

class SearchService {
  Timer? _debounce;
  
  void onSearchChanged(String query) {
    // Önceki timer'ı iptal et
    if (_debounce?.isActive ?? false) _debounce!.cancel();
    
    // 500ms bekle, eğer kullanıcı yazmaya devam ederse yeniden başlat
    _debounce = Timer(Duration(milliseconds: 500), () {
      performSearch(query);
    });
  }
  
  Future<void> performSearch(String query) async {
    print('Arama yapılıyor: $query');
    // API çağrısı buraya
  }
  
  void dispose() {
    _debounce?.cancel();
  }
}
```

---

## 💡 Özet Tablo

| Konu | Amaç | Anahtar Kelimeler | Kullanım Alanı |
|------|------|-------------------|----------------|
| **Future** | Tek seferlik asenkron işlem | `async`, `await`, `then`, `catchError` | API çağrıları, dosya okuma |
| **Multiple Futures** | Çoklu işlemleri yönetme | `Future.wait`, `Future.any` | Paralel API çağrıları |
| **Stream** | Sürekli veri akışı | `Stream`, `async*`, `yield`, `await for` | Canlı konum, chat, sensör verileri |
| **StreamController** | Manuel stream kontrolü | `StreamController`, `sink`, `listen` | Özel veri akışları |
| **Error Handling** | Hata yönetimi | `try-catch`, `timeout`, `retry` | Güvenli asenkron kod |

---

## 🎯 Best Practices (En İyi Uygulamalar)

### ✅ Yapılması Gerekenler
```dart
// 1. async fonksiyonlarda await kullan
Future<void> goodExample() async {
  var data = await fetchData(); // ✅ Doğru
  print(data);
}

// 2. Hata yakalama ekle
Future<void> safeExample() async {
  try {
    var data = await fetchData();
  } catch (e) {
    print('Hata: $e');
  }
}

// 3. StreamController'ı kapat
void streamExample() {
  var controller = StreamController();
  // ... kullanım
  controller.close(); // ✅ Mutlaka kapat
}
```

### ❌ Yapılmaması Gerekenler
```dart
// 1. async olmadan await kullanma
void badExample() {
  var data = await fetchData(); // ❌ HATA: async yok
}

// 2. await olmadan Future beklemek
Future<void> forgotAwait() async {
  fetchData(); // ❌ Sonucu beklemeden devam eder
  print('Bu hemen çalışır');
}

// 3. Gereksiz await kullanımı
Future<String> unnecessaryAwait() async {
  return await fetchData(); // ❌ Gereksiz await
}

// Doğrusu:
Future<String> better() {
  return fetchData(); // ✅ Zaten Future döndürüyor
}
```

---

## 🔄 Async/Await Akış Şeması
```
1. Fonksiyon async ile işaretlenir
   ↓
2. await ile asenkron işlem başlatılır
   ↓
3. İşlem tamamlanana kadar BU FONKSİYON bekler
   ↓
4. Diğer kodlar (main, UI) çalışmaya DEVAM eder
   ↓
5. İşlem bitince sonraki satır çalışır
```

---

## 🚀 Flutter'da Kullanım Örneği
```dart
class UserPage extends StatefulWidget {
  @override
  _UserPageState createState() => _UserPageState();
}

class _UserPageState extends State<UserPage> {
  User? user;
  bool isLoading = true;
  String? error;
  
  @override
  void initState() {
    super.initState();
    loadUser();
  }
  
  Future<void> loadUser() async {
    setState(() {
      isLoading = true;
      error = null;
    });
    
    try {
      User fetchedUser = await UserService().fetchUser(1);
      setState(() {
        user = fetchedUser;
        isLoading = false;
      });
    } catch (e) {
      setState(() {
        error = e.toString();
        isLoading = false;
      });
    }
  }
  
  @override
  Widget build(BuildContext context) {
    if (isLoading) {
      return CircularProgressIndicator();
    }
    
    if (error != null) {
      return Text('Hata: $error');
    }
    
    return Text('Kullanıcı: ${user?.name}');
  }
}
```