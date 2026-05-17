# Roll Dice App - Flutter Learning Project

Aplikasi Flutter sederhana untuk meroll dadu. Project ini digunakan untuk mempelajari konsep-konsep dasar Flutter dan widget architecture.

## 📱 Fitur Aplikasi

- Menampilkan gambar dadu yang sesuai dengan hasil roll
- Tombol untuk meroll dadu
- Background dengan gradient warna ungu
- UI yang responsif dan menarik

---

## 🎓 Konsep Flutter yang Dipelajari

### 1. **Widget dan Widget Tree**

Flutter membangun UI menggunakan widget. Semua elemen di Flutter adalah widget, dan mereka bersusun membentuk widget tree (pohon widget).

```
MaterialApp
  └── Scaffold
      └── GradientContainer
          └── Column
              ├── Image
              └── TextButton
```

**File: [main.dart](lib/main.dart)**
```dart
runApp(
  const MaterialApp(
    home: Scaffold(
      body: GradientContainer(...),
    ),
  ),
);
```

### 2. **Stateless Widget vs Stateful Widget**

#### **Stateless Widget** - Widget yang tidak berubah
- UI statis dan tidak berubah setelah dirender
- Tidak memiliki state
- Contoh: `GradientContainer`, `StyledText`

**File: [gradient_container.dart](lib/gradient_container.dart)**
```dart
class GradientContainer extends StatelessWidget {
  const GradientContainer(this.color1, this.color2, {super.key});
  
  final Color color1;
  final Color color2;
  
  @override
  Widget build(context) {
    return Container(...);
  }
}
```

#### **Stateful Widget** - Widget yang dapat berubah
- UI dapat berubah berdasarkan perubahan state
- Memiliki state yang dapat diupdate
- Contoh: `DiceRoller`

**File: [dice_roller.dart](lib/dice_roller.dart)**
```dart
class DiceRoller extends StatefulWidget {
  const DiceRoller({super.key});
  
  @override
  State<DiceRoller> createState() {
    return _DiceRollerState();
  }
}

class _DiceRollerState extends State<DiceRoller> {
  var currentDiceRoll = randomizer.nextInt(6) + 1;
  
  @override
  Widget build(context) { ... }
}
```

### 3. **State Management dengan setState()**

`setState()` digunakan untuk memberitahu Flutter bahwa state telah berubah dan UI perlu di-rebuild.

```dart
void rollDice() {
  setState(() {
    currentDiceRoll = randomizer.nextInt(6) + 1;
  });
}
```

Ketika `setState()` dipanggil, Flutter memanggil method `build()` lagi untuk mengupdate UI dengan state yang baru.

### 4. **MaterialApp dan Scaffold**

- **MaterialApp**: Widget root yang mengonfigurasi aplikasi dengan Material Design
- **Scaffold**: Menyediakan struktur dasar untuk halaman dengan app bar, floating button, drawer, dll.

```dart
MaterialApp(
  home: Scaffold(
    body: GradientContainer(...),
  ),
)
```

### 5. **Layout Widgets**

#### **Container**
Widget untuk memberi background, padding, margin, dan dekorasi.

```dart
Container(
  decoration: BoxDecoration(
    gradient: LinearGradient(...),
  ),
  child: Center(...),
)
```

#### **Column**
Widget untuk menyusun children secara vertikal (dari atas ke bawah).

```dart
Column(
  mainAxisSize: MainAxisSize.min,
  children: [
    Image.asset(...),
    const SizedBox(height: 20),
    TextButton(...),
  ],
)
```

#### **Center**
Widget untuk menempatkan child di tengah-tengah.

```dart
Center(
  child: DiceRoller(),
)
```

#### **SizedBox**
Widget untuk memberikan spasi atau mengatur ukuran.

```dart
const SizedBox(height: 20)  // Spasi vertikal 20 pixel
```

### 6. **LinearGradient**

Untuk membuat background dengan warna gradient (perubahan warna bertahap).

```dart
BoxDecoration(
  gradient: LinearGradient(
    colors: [color1, color2],
    begin: Alignment.topLeft,
    end: Alignment.bottomRight,
  ),
)
```

**Colors**: `Color.fromARGB(255, 33, 5, 109)` hingga `Color.fromARGB(255, 68, 21, 149)`

### 7. **Image Widget**

Menampilkan gambar dari assets.

```dart
Image.asset(
  'assets/images/dice-$currentDiceRoll.png',
  width: 200,
)
```

Gambar disimpan di folder `assets/images/` dan direferensikan di `pubspec.yaml`.

### 8. **TextButton**

Tombol dengan teks yang dapat diklik.

```dart
TextButton(
  onPressed: rollDice,
  style: TextButton.styleFrom(
    foregroundColor: Colors.white,
    textStyle: const TextStyle(fontSize: 28),
  ),
  child: const Text('Roll Dice'),
)
```

- **onPressed**: Fungsi yang dijalankan saat tombol diklik
- **style**: Styling untuk tombol
- **child**: Widget yang ditampilkan di dalam tombol

### 9. **Text dan TextStyle**

Menampilkan teks dengan styling.

```dart
Text(
  'Hello',
  style: TextStyle(
    color: Colors.white,
    fontSize: 28,
  ),
)
```

### 10. **const dan Constructor**

#### **const** - Optimisasi performa
Keyword `const` membuat widget immutable dan dapat di-reuse tanpa rebuild.

```dart
const SizedBox(height: 20)
const MaterialApp(...)
```

#### **Constructor Parameter**
Cara mengirim data ke widget.

```dart
class GradientContainer extends StatelessWidget {
  const GradientContainer(this.color1, this.color2, {super.key});
  
  final Color color1;
  final Color color2;
}
```

Penggunaan:
```dart
GradientContainer(
  Color.fromARGB(255, 33, 5, 109),
  Color.fromARGB(255, 68, 21, 149),
)
```

### 11. **Random Number Generation**

Menggunakan library `dart:math` untuk generate random number.

```dart
import 'dart:math';

var randomizer = Random();

var currentDiceRoll = randomizer.nextInt(6) + 1;  // Generate 1-6
```

- `nextInt(6)` menghasilkan 0-5
- `+ 1` untuk mendapatkan range 1-6

### 12. **Package Import dan Project Structure**

```dart
import 'package:flutter/material.dart';
import 'package:roll_dice_app/gradient_container.dart';
import 'package:roll_dice_app/dice_roller.dart';
```

Struktur project:
```
lib/
  ├── main.dart                 (Entry point)
  ├── gradient_container.dart   (Custom widget)
  ├── dice_roller.dart          (Custom stateful widget)
  └── styled_text.dart          (Custom widget)
```

### 13. **CustomWidget / Widget Composition**

Membuat widget custom untuk reusability dan clean code.

**File: [styled_text.dart](lib/styled_text.dart)**
```dart
class StyledText extends StatelessWidget {
  StyledText(this.text, {super.key});
  
  final String text;
  
  @override
  Widget build(context) {
    return Text(
      text,
      style: TextStyle(
        color: Colors.white,
        fontSize: 28,
      ),
    );
  }
}
```

---

## 🔄 Flow Aplikasi

1. **Aplikasi Dimulai**: `main()` dipanggil
2. **BuildContext**: Flutter membuat widget tree dari `MaterialApp`
3. **Initial Render**: `GradientContainer` di-render dengan gradient background
4. **User Interaction**: User klik tombol "Roll Dice"
5. **setState() Called**: `rollDice()` memanggil `setState()`
6. **Rebuild**: Flutter memanggil `build()` lagi dengan `currentDiceRoll` yang baru
7. **UI Updated**: Gambar dadu dan widget lainnya diperbarui

---

## 🛠 Tools dan Dependencies

- **Flutter SDK**: Framework untuk mobile development
- **Dart**: Bahasa pemrograman yang digunakan Flutter
- **pub.dev**: Package manager untuk Flutter/Dart
- **Android Studio / VS Code**: IDE untuk development

---

## 📚 Resources Pembelajaran

- [Flutter Documentation](https://docs.flutter.dev/)
- [Dart Language Tour](https://dart.dev/guides/language/language-tour)
- [Flutter Widgets Catalog](https://docs.flutter.dev/development/ui/widgets)
- [Material Design](https://material.io/)

- **Udemy Course (sumber belajar utama):** https://www.udemy.com/share/101rfI3@wYbVOUydNrf5xM4r9AM_2DpToW6AeeM8g7wSTVRQsuqbu5OUovFWllKOqNAATDF3Nw==/  
  Catatan: Saya mengikuti kursus Udemy ini sebagai panduan praktis. Materi yang dipelajari dari kursus(App pertama yaitu ROLL DICE APP) ini meliputi:
  - Pengenalan Flutter dan Dart
  - Struktur project dan manajemen asset
  - Perbedaan `StatelessWidget` dan `StatefulWidget`
  - Pengelolaan state sederhana dengan `setState()`
  - Layout dasar menggunakan `Column`, `Row`, `Container`, dan `Center`
  - Styling teks dan tombol, serta penggunaan `TextButton` dan `TextStyle`
  - Menampilkan gambar dari `assets` dan konfigurasi `pubspec.yaml`
  - Penggunaan `Random` untuk logika sederhana (mis. roll dadu)
  - Membuat widget custom untuk meningkatkan reusability
  - Tips debugging dan best practices dalam pengembangan Flutter

---

## ✨ Key Takeaways

✅ **Widget-based UI**: Semua adalah widget di Flutter  
✅ **State Management**: `setState()` untuk update UI  
✅ **Component Reusability**: Buat custom widget untuk kode yang lebih clean  
✅ **Layout Flexibility**: Use Column, Row, Center, Container untuk layout  
✅ **Immutability**: Gunakan `const` untuk optimisasi performa  
✅ **Composition over Inheritance**: Compose widgets daripada inherit
