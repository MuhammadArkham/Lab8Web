
Nama : Muhammad Arkhamullah Rifai Asshidiq

Nim : 312410545

Kelas : TI.24.A.5

## 📋 Deskripsi

Repository ini berisi implementasi aplikasi CRUD (Create, Read, Update, Delete) sederhana menggunakan PHP dan MySQL untuk manajemen data barang. 

##  Tujuan Pembelajaran

1. Memahami konsep dasar Database MySQL
2. Memahami konsep dasar operasi CRUD menggunakan PHP
3. Mampu membuat aplikasi CRUD sederhana dengan PHP dan MySQL


## 📁 Struktur Direktori

```
lab8_php_database/
├── gambar/                 # Folder untuk menyimpan gambar produk
├── foto/                   # Folder untuk screenshot dokumentasi
├── koneksi.php            # File konfigurasi koneksi database
├── index.php              # Halaman utama (Read)
├── tambah.php             # Halaman tambah data (Create)
├── ubah.php               # Halaman ubah data (Update)
├── hapus.php              # Halaman hapus data (Delete)
├── style.css              # File styling CSS
└── README.md              # Dokumentasi proyek
```

##  Instalasi dan Konfigurasi


install :
- XAMPP (Apache & MySQL)
- Web Browser 
- Text Editor (VS Code, Sublime, dll)

### Langkah Instalasi

1. **Clone atau Download Repository**
   ```bash
   git clone https://github.com/username/Lab8Web.git
   ```

2. **Pindahkan ke Direktori XAMPP**
   ```
   Copy folder ke: C:\xampp\htdocs\lab8_php_database\
   ```

3. **Jalankan XAMPP**
   - Buka XAMPP Control Panel
   - Start Apache dan MySQL
   
   ![XAMPP Control](https://github.com/MuhammadArkham/Lab8Web/blob/main/Screenshot/Screenshot%202025-11-25%20170111.png?raw=true)

4. **Buat Database**
   - Akses phpMyAdmin: `http://localhost/phpmyadmin/`
   - Buat database baru dengan nama `latihan1`
   
5. **Import Tabel**
   
   Jalankan query SQL berikut:
   
   ```sql
   CREATE TABLE data_barang (
       id_barang int(10) auto_increment Primary Key,
       kategori varchar(30),
       nama varchar(30),
       gambar varchar(100),
       harga_beli decimal(10,0),
       harga_jual decimal(10,0),
       stok int(4)
   );
   ```
   
   ![Create Table](https://github.com/MuhammadArkham/Lab8Web/blob/main/Screenshot/Screenshot%202025-11-25%20170343.png?raw=true)

6. **Insert Data Awal**
   
   ```sql
   INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
   VALUES 
   ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
   ('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
   ('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
   ```
   
   ![Insert Data](https://github.com/MuhammadArkham/Lab8Web/blob/main/Screenshot/Screenshot%202025-11-25%20170343.png?raw=true)

7. **Konfigurasi Koneksi Database**
   
   Edit file `koneksi.php` sesuai konfigurasi Anda:
   
   ```php
   <?php
   $host = "localhost";
   $user = "root";
   $pass = "";
   $db = "latihan1";
   
   $conn = mysqli_connect($host, $user, $pass, $db);
   if ($conn == false) {
       echo "Koneksi ke server gagal.";
       die();
   }
   ?>
   ```

8. **Akses Aplikasi**
   
   Buka browser dan akses: `http://localhost/lab8_php_database/`
   
   ![Directory Access](https://github.com/MuhammadArkham/Lab8Web/blob/main/Screenshot/Screenshot%202025-11-25%20170407.png?raw=true)

##  Dokumentasi Fitur

### 1. Koneksi Database

File `koneksi.php` berfungsi untuk menghubungkan aplikasi dengan database MySQL.


**Kode:**
```php
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihan1";
$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false) {
    echo "Koneksi ke server gagal.";
    die();
}
?>
```

### 2. READ - Menampilkan Data (index.php)

Halaman utama untuk menampilkan seluruh data barang dalam bentuk tabel.

![Index Page](https://github.com/MuhammadArkham/Lab8Web/blob/main/Screenshot/Screenshot%202025-11-25%20170407.png?raw=true)

**Fitur:**
- Menampilkan semua data barang
- Gambar produk
- Tombol aksi (Ubah & Hapus)
- Link ke halaman Tambah Barang

**Kode Utama:**
```php
<?php
include("koneksi.php");

$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);
?>
```

### 3. CREATE - Menambah Data (tambah.php)

Formulir untuk menambahkan data barang baru ke database.

![Tambah Barang](https://github.com/MuhammadArkham/Lab8Web/blob/main/Screenshot/Screenshot%202025-11-25%20170530.png?raw=true)

**Fitur:**
- Form input data lengkap
- Upload gambar produk
- Validasi data
- Redirect ke halaman utama setelah berhasil

**Kode Utama:**
```php
if (isset($_POST['submit'])) {
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    // ... kode lainnya
    
    $sql = 'INSERT INTO data_barang (nama, kategori, harga_jual, harga_beli, stok, gambar)';
    $sql .= "VALUE ('{$nama}', '{$kategori}', '{$harga_jual}', '{$harga_beli}', '{$stok}', '{$gambar}')";
    $result = mysqli_query($conn, $sql);
    header('location: index.php');
}
```

### 4. UPDATE - Mengubah Data (ubah.php)

Halaman untuk mengedit data barang yang sudah ada.

![Ubah Barang](https://github.com/MuhammadArkham/Lab8Web/blob/main/Screenshot/Screenshot%202025-11-25%20170628.png?raw=true)

**Fitur:**
- Form terisi otomatis dengan data existing
- Update gambar (opsional)
- Validasi data
- Redirect setelah update berhasil

**Kode Utama:**
```php
$id = $_GET['id'];
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
$data = mysqli_fetch_array($result);

// Update query
$sql = 'UPDATE data_barang SET ';
$sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
$sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok = '{$stok}' ";
$sql .= "WHERE id_barang = '{$id}'";
```

### 5. DELETE - Menghapus Data (hapus.php)
![Hapus Barang](https://github.com/MuhammadArkham/Lab8Web/blob/main/Screenshot/Screenshot%202025-11-25%20170639.png?raw=true)
Script untuk menghapus data barang dari database.

**Fitur:**
- Konfirmasi sebelum menghapus
- Delete berdasarkan ID
- Redirect otomatis

**Kode:**
```php
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```

## 🎨 Styling (style.css)

Aplikasi menggunakan CSS custom untuk tampilan yang lebih menarik:

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, sans-serif;
}

body {
    background: #f8f8f8;
    padding: 20px;
}

table {
    border-collapse: collapse;
    width: 100%;
    margin-top: 15px;
    background: #fff;
}

table th, table td {
    border: 1px solid #bbb;
    padding: 10px;
    text-align: left;
}

```


