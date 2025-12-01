# 🎯 Dart Fundamentals

## 1️⃣ Variables

**Amaç:** Değerleri saklamak için değişken tanımlamak.
```dart
var name = 'John';     // Türü otomatik çıkarım
String city = 'Paris'; // Açık tür tanımı
final age = 25;        // Run-time sabit
const pi = 3.14;       // Compile-time sabit
```

### 🧩 Not: 
Değişmeme garantisi varsa `final` veya `const` kullan.
- **const:** Derleme zamanında (compile-time) sabitlenir.
- **final:** Çalışma zamanında (run-time) atanır ama sonrasında değişmez.

---

## 2️⃣ Data Types

**Amaç:** Bellekte tutulan verilerin türünü belirtmek.

### ✔️ Temel Türler
```dart
int a = 10;
double b = 12.5;
bool isActive = true;
String text = "Hello!";
```

### ✔️ List (Liste)
```dart
List<int> nums = [1, 2, 3];
```

### ✔️ Map (Anahtar-Değer)
```dart
Map<String, String> user = {
  'name': 'Alice',
  'city': 'London',
};
```

### ✔️ Set (Benzersiz Koleksiyon)
```dart
Set<String> tags = {'dev', 'mobile'};
```

---

## 3️⃣ Control Flow

**Amaç:** Programın akışını mantıksal olarak kontrol etmek.

### ✔️ if / else
```dart
if (age >= 18) {
  print("Adult");
} else {
  print("Minor");
}
```

### ✔️ switch
```dart
switch (role) {
  case 'admin':
    print("Full access");
    break;
  case 'user':
    print("Limited access");
    break;
}
```

### ✔️ Loops (Döngüler)
```dart
// For Loop
for (var i = 0; i < 5; i++) {
  print(i);
}

// While Loop
while (condition) {
  // ...
}

// Do-While Loop
do {
  // ...
} while (condition);
```

### ✔️ forEach
```dart
numbers.forEach((n) => print(n));
```

---

## 4️⃣ Null Safety

**Amaç:** Null (boş değer) kaynaklı çalışma zamanı hatalarını önlemek.

### ✔️ Null Olmayan Değişken
```dart
String name = "Mike"; // Asla null olamaz
```

### ✔️ Null Olabilir Değişken (?)
```dart
String? description; // Null değer alabilir
```

### ✔️ Null Kontrolü (??)
```dart
print(description ?? "No description"); // Eğer null ise varsayılanı yazdır
```

### ✔️ Null Assertion (!)
```dart
String text = description!; // Null olmadığından eminim, hata verirse sorumluluk bende
```

### ✔️ Late (Gecikmeli Başlatma)
```dart
late String token; // Değeri daha sonra atanacak, kullanmadan önce atanmalı
```

---

## 💡 Özet Tablo

| Konu | İşlev | Öne Çıkan Anahtar Kelimeler | Ortak Kullanım |
|------|-------|------------------------------|----------------|
| **Variables** | Veri saklamak | `var`, `final`, `const` | Temel değer tanımları |
| **Data Types** | Veri türü belirtmek | `int`, `String`, `List`, `Map` | Veri yapıları ve modelleri |
| **Control Flow** | Akış kontrolü | `if`, `switch`, loops | İş mantığı ve karar mekanizmaları |
| **Null Safety** | Null hatalarını önlemek | `?`, `!`, `late` | Daha güvenli ve hatasız kod |