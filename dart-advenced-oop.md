# 🚀 Dart Advanced & OOP

## 1️⃣ Classes (Sınıflar)

**Amaç:** Nesne yönelimli programlamada veri ve davranışları bir araya getirmek.

### ✔️ Temel Class Tanımı
```dart
class Person {
  String name;
  int age;
  
  // Constructor (Yapıcı Metod)
  Person(this.name, this.age);
  
  // Method (Metod)
  void introduce() {
    print('Merhaba, ben $name, $age yaşındayım.');
  }
}

// Kullanım
var person = Person('Ali', 25);
person.introduce(); // Çıktı: Merhaba, ben Ali, 25 yaşındayım.
```

### ✔️ Named Constructor (İsimlendirilmiş Yapıcı)
```dart
class User {
  String name;
  String email;
  
  User(this.name, this.email);
  
  // Named constructor
  User.guest() : name = 'Misafir', email = 'guest@example.com';
}

// Kullanım
var guest = User.guest();
```

### ✔️ Private Members (Gizli Üyeler)
```dart
class BankAccount {
  String _accountNumber; // Alt çizgi (_) ile başlarsa private olur
  double _balance = 0.0;
  
  BankAccount(this._accountNumber);
  
  // Getter
  double get balance => _balance;
  
  // Setter
  set deposit(double amount) {
    if (amount > 0) _balance += amount;
  }
}

// Kullanım
var account = BankAccount('123456');
account.deposit = 1000;
print(account.balance); // 1000.0
```

---

## 2️⃣ Inheritance (Kalıtım)

**Amaç:** Mevcut bir sınıfın özelliklerini ve davranışlarını yeni bir sınıfa aktarmak.

### ✔️ Temel Kalıtım
```dart
// Parent class (Üst Sınıf)
class Animal {
  String name;
  
  Animal(this.name);
  
  void makeSound() {
    print('$name bir ses çıkarıyor');
  }
}

// Child class (Alt Sınıf)
class Dog extends Animal {
  String breed;
  
  Dog(String name, this.breed) : super(name);
  
  // Override (Üzerine Yazma)
  @override
  void makeSound() {
    print('$name havlıyor: Hav hav!');
  }
  
  void fetch() {
    print('$name topu getiriyor');
  }
}

// Kullanım
var dog = Dog('Karabaş', 'Golden Retriever');
dog.makeSound(); // Karabaş havlıyor: Hav hav!
dog.fetch(); // Karabaş topu getiriyor
```

### ✔️ super Anahtar Kelimesi
```dart
class Employee {
  String name;
  double salary;
  
  Employee(this.name, this.salary);
  
  void work() {
    print('$name çalışıyor');
  }
}

class Manager extends Employee {
  List<String> team;
  
  Manager(String name, double salary, this.team) : super(name, salary);
  
  @override
  void work() {
    super.work(); // Parent class metodunu çağırır
    print('$name ayrıca takımını yönetiyor');
  }
}
```

---

## 3️⃣ Abstract Classes (Soyut Sınıflar)

**Amaç:** Bir şablon oluşturmak, doğrudan nesne üretilmesini engellemek.

### ✔️ Abstract Class Tanımı
```dart
// Abstract class - direkt olarak örneklenemez
abstract class Shape {
  String color;
  
  Shape(this.color);
  
  // Abstract method - alt sınıflarda mutlaka implement edilmeli
  double calculateArea();
  
  // Normal method
  void display() {
    print('Bu şeklin rengi: $color');
  }
}

class Circle extends Shape {
  double radius;
  
  Circle(String color, this.radius) : super(color);
  
  @override
  double calculateArea() {
    return 3.14 * radius * radius;
  }
}

class Rectangle extends Shape {
  double width;
  double height;
  
  Rectangle(String color, this.width, this.height) : super(color);
  
  @override
  double calculateArea() {
    return width * height;
  }
}

// Kullanım
var circle = Circle('Kırmızı', 5);
print(circle.calculateArea()); // 78.5
circle.display(); // Bu şeklin rengi: Kırmızı
```

---

## 4️⃣ Interfaces (Arayüzler)

**Amaç:** Bir sınıfın hangi metodları içermesi gerektiğini belirlemek.

### 🧩 Not: 
Dart'ta `interface` anahtar kelimesi yoktur. Her class otomatik olarak bir interface olarak kullanılabilir.

### ✔️ Interface Kullanımı (implements)
```dart
class Printable {
  void printDocument() {
    print('Belge yazdırılıyor');
  }
}

class Scannable {
  void scanDocument() {
    print('Belge taranıyor');
  }
}

// Birden fazla interface implement edilebilir
class Printer implements Printable, Scannable {
  @override
  void printDocument() {
    print('Yazıcı: Belge yazdırılıyor');
  }
  
  @override
  void scanDocument() {
    print('Yazıcı: Belge taranıyor');
  }
}

// Kullanım
var printer = Printer();
printer.printDocument(); // Yazıcı: Belge yazdırılıyor
printer.scanDocument(); // Yazıcı: Belge taranıyor
```

### ✔️ extends vs implements Farkı
```dart
// extends: Parent class'ın tüm özelliklerini miras alır
class Cat extends Animal {
  // Animal'ın name ve makeSound özelliklerini miras alır
}

// implements: Sadece contract (sözleşme) alır, tüm metodları yeniden yazmak zorundasın
class Printer implements Printable {
  // Printable'daki tüm metodları override etmek ZORUNLU
  @override
  void printDocument() {
    // Yeni implementasyon
  }
}
```

---

## 5️⃣ Mixins (Karışımlar)

**Amaç:** Kod tekrarını önlemek için davranışları farklı sınıflara eklemek.

### 🧩 Not:
- Mixin'ler constructor (yapıcı metod) içeremez
- Birden fazla mixin kullanılabilir
- `with` anahtar kelimesi ile kullanılır

### ✔️ Mixin Tanımı ve Kullanımı
```dart
// Mixin tanımı
mixin Swimming {
  void swim() {
    print('Yüzüyor');
  }
}

mixin Flying {
  void fly() {
    print('Uçuyor');
  }
}

mixin Walking {
  void walk() {
    print('Yürüyor');
  }
}

// Sadece yürüyen hayvan
class Human with Walking {
  String name;
  Human(this.name);
}

// Yürüyen ve yüzen hayvan
class Duck with Walking, Swimming, Flying {
  String name;
  Duck(this.name);
}

// Sadece yüzen hayvan
class Fish with Swimming {
  String name;
  Fish(this.name);
}

// Kullanım
var human = Human('Ali');
human.walk(); // Yürüyor

var duck = Duck('Donald');
duck.walk(); // Yürüyor
duck.swim(); // Yüzüyor
duck.fly(); // Uçuyor

var fish = Fish('Nemo');
fish.swim(); // Yüzüyor
```

### ✔️ extends, with, implements Birlikte Kullanımı
```dart
abstract class Animal {
  String name;
  Animal(this.name);
  void makeSound();
}

mixin Running {
  void run() => print('Koşuyor');
}

mixin Jumping {
  void jump() => print('Zıplıyor');
}

// Sıralama önemli: extends -> with -> implements
class Kangaroo extends Animal with Running, Jumping {
  Kangaroo(String name) : super(name);
  
  @override
  void makeSound() {
    print('$name ses çıkarıyor');
  }
}

// Kullanım
var kangaroo = Kangaroo('Skippy');
kangaroo.makeSound(); // Skippy ses çıkarıyor
kangaroo.run(); // Koşuyor
kangaroo.jump(); // Zıplıyor
```

---

## 6️⃣ Collections (Koleksiyonlar)

**Amaç:** Birden fazla veriyi organize bir şekilde saklamak ve yönetmek.

### ✔️ List (Liste - Sıralı Koleksiyon)
```dart
// Temel liste
List<String> fruits = ['Elma', 'Armut', 'Muz'];

// Eleman ekleme
fruits.add('Çilek');
fruits.addAll(['Kiraz', 'Üzüm']);

// Eleman silme
fruits.remove('Elma');
fruits.removeAt(0);

// Liste metodları
print(fruits.length); // Uzunluk
print(fruits.first); // İlk eleman
print(fruits.last); // Son eleman
print(fruits.isEmpty); // Boş mu?
print(fruits.contains('Muz')); // İçeriyor mu?

// Liste döngüsü
for (var fruit in fruits) {
  print(fruit);
}

// forEach
fruits.forEach((fruit) => print(fruit));

// map (Dönüştürme)
var upperFruits = fruits.map((f) => f.toUpperCase()).toList();

// where (Filtreleme)
var longNames = fruits.where((f) => f.length > 4).toList();

// any (Herhangi biri koşula uyuyor mu?)
bool hasLongName = fruits.any((f) => f.length > 5);

// every (Hepsi koşula uyuyor mu?)
bool allShort = fruits.every((f) => f.length < 10);
```

### ✔️ Set (Benzersiz Koleksiyon)
```dart
// Set - Tekrar eden elemanları kabul etmez
Set<int> numbers = {1, 2, 3, 4, 5};

// Eleman ekleme (tekrar ekleme yapılmaz)
numbers.add(3); // Eklenmez, zaten var
numbers.add(6); // Eklenir

// Set işlemleri
Set<int> evenNumbers = {2, 4, 6, 8};
Set<int> oddNumbers = {1, 3, 5, 7};

// Birleşim (Union)
var allNumbers = evenNumbers.union(oddNumbers); // {1, 2, 3, 4, 5, 6, 7, 8}

// Kesişim (Intersection)
var common = evenNumbers.intersection({2, 3, 4}); // {2, 4}

// Fark (Difference)
var difference = evenNumbers.difference({2, 4}); // {6, 8}
```

### ✔️ Map (Anahtar-Değer Çiftleri)
```dart
// Map tanımı
Map<String, dynamic> user = {
  'name': 'Ayşe',
  'age': 28,
  'email': 'ayse@example.com',
  'isActive': true,
};

// Değer ekleme/güncelleme
user['phone'] = '555-1234';
user['age'] = 29;

// Değer okuma
print(user['name']); // Ayşe

// Güvenli okuma (null safety)
print(user['address'] ?? 'Adres yok');

// Map metodları
print(user.keys); // Tüm anahtarlar
print(user.values); // Tüm değerler
print(user.length); // Eleman sayısı
print(user.containsKey('email')); // Anahtar var mı?
print(user.containsValue(28)); // Değer var mı?

// Map döngüsü
user.forEach((key, value) {
  print('$key: $value');
});

// Entries ile döngü
for (var entry in user.entries) {
  print('${entry.key}: ${entry.value}');
}

// Map dönüştürme
var names = user.map((key, value) => MapEntry(key, value.toString()));
```

### ✔️ İleri Seviye Collection İşlemleri
```dart
// Liste içinde Map
List<Map<String, dynamic>> users = [
  {'name': 'Ali', 'age': 25},
  {'name': 'Veli', 'age': 30},
  {'name': 'Ayşe', 'age': 28},
];

// Filtreleme ve Sıralama
var adults = users.where((user) => user['age'] >= 18).toList();
users.sort((a, b) => a['age'].compareTo(b['age']));

// reduce (Toplama)
List<int> numbers = [1, 2, 3, 4, 5];
var sum = numbers.reduce((a, b) => a + b); // 15

// fold (Başlangıç değeri ile toplama)
var total = numbers.fold(10, (prev, element) => prev + element); // 25

// Spread operator (...)
List<int> list1 = [1, 2, 3];
List<int> list2 = [4, 5, 6];
List<int> combined = [...list1, ...list2]; // [1, 2, 3, 4, 5, 6]

// Collection if
bool includeZero = true;
List<int> nums = [
  if (includeZero) 0,
  1,
  2,
  3,
];

// Collection for
List<int> doubled = [
  for (var i in numbers) i * 2
]; // [2, 4, 6, 8, 10]
```

---

## 💡 Özet Tablo

| Konu | Amaç | Anahtar Kelimeler | Kullanım Alanı |
|------|------|-------------------|----------------|
| **Classes** | Nesne oluşturmak | `class`, `constructor`, `getter`, `setter` | Veri modelleme, nesne yönelimli tasarım |
| **Inheritance** | Kod tekrarını önlemek | `extends`, `super`, `@override` | Ortak özellikleri paylaşma |
| **Abstract Classes** | Şablon oluşturmak | `abstract`, `abstract method` | Sözleşme tanımlama, polimorfizm |
| **Interfaces** | Sözleşme tanımlamak | `implements` | Çoklu davranış tanımlama |
| **Mixins** | Davranış ekleme | `mixin`, `with` | Çoklu kalıtım alternatifi, kod yeniden kullanımı |
| **Collections** | Veri yapıları | `List`, `Set`, `Map` | Veri saklama ve yönetme |

---

## 🎯 extends vs implements vs with Farkları

| Özellik | `extends` | `implements` | `with` |
|---------|-----------|--------------|--------|
| **Amaç** | Kalıtım (miras alma) | Sözleşme (contract) | Davranış ekleme |
| **Sayı** | Tek (single inheritance) | Çoklu (multiple) | Çoklu (multiple) |
| **Override Zorunluluğu** | İsteğe bağlı | Zorunlu | İsteğe bağlı |
| **Constructor** | Var | Yok (göz ardı edilir) | Olamaz |
| **Kullanım** | `class Dog extends Animal` | `class Printer implements Printable` | `class Duck with Flying` |