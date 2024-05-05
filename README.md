# Advance Programming Tutorial 10
Tutorial for Advanced Programming 2024 Module 10 - Faculty of Computer Science, Universitas Indonesia

---
## Reflection - Timer Future

---

### 1.2. Understanding How It Works

![](image/1.2.png)

Dalam kode Rust ini, Rust mengimplementasi executor sederhana untuk menjalankan task-task asynchronous. Struktur data yang dimaanfaatkan contohnya `Task`, `Spawner`, dan `Executor` dengan memanfaatkan komunikasi antara spawner dan executor. Urutan output yang dihasilkan adalah "hey hey", "howdy!", dan kemudian "done!" yang dipengaruhi oleh executor yang menjalankan task asynchronous.

Output pertama yaitu "hey hey" dicetak pertama kali karena ada di dalam block `main()` yang dieksekusi sebelum task asynchronous. Kemudian task asynchronous "howdy!" dieksekusi, dan sebelum eksekusinya selesai "done!" belum dapat dihasilkan. Executor akan menunggu dengan `TimerFuture` (dalam kode ini 2 detik) sampai task asynchronous selesai baru kemudian mencetak "done!".

### 1.3. Multiple Spawn and removing drop

Multiple Spawn and Removing Drop:
![](image/1.3.(Multiple%20Without%20Drop).png)

Multiple Spawn and Put Drop:
![](image/1.3.(Multiple%20Drop).png)

Function `spawner.spawn(async { ... })` berfungsi untuk memicu task-task asynchronous yang nantinya akan dieksekusi oleh Executor. Sedangkan function `drop(spawner)` berfungsi untuk menandakan bahwa tidak ada lagi task yang masuk ke dalam queue task yang akan dieksekusi.

Output yang dihasilkan ketika ada `drop(spawner)` dan ketika function tersebut dihapus berbeda. Perbedaan tersebut terjadi karena tanpa `drop(spawner)`, executor akan terus menunggu task baru untuk dieksekusi, sehingga tasks yang telah siap akan langsung dieksekusi. Namun, dengan `drop(spawner)`, executor mengetahui bahwa tidak akan ada task baru yang masuk ke dalam queue, sehingga akan diselesaikan dulu task yang masih ada di dalam queue sebelum mengakhiri eksekusi. Hal ini yang menyebabkan adanya perbedaan urutan output pada bagian "done".