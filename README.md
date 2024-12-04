# Lab10Web
Lab 10 PHP OOP
## Nur Laila (312310149)

### Buat folder baru dengan nama lab10_php_oop pada docroot webserver (htdocs) 
![image](https://github.com/user-attachments/assets/1b44149f-8aa1-4a96-9a2f-db10d74be341)   

### 1. Buat file baru dengan nama mobil.php 
```sh
<?php
/**
* Program sederhana pendefinisian class dan pemanggilan class.
**/
class Mobil
{
    private $warna;
    private $merk;
    private $harga;

public function __construct()
{
    $this->warna = "Biru";
    $this->merk = "BMW";
    $this->harga = "10000000";
}
    public function gantiWarna ($warnaBaru)
    {
        $this->warna = $warnaBaru;
    }
    public function tampilWarna ()
    {
       echo "Warna mobilnya : " . $this->warna;
    }
}
// membuat objek mobil
$a = new Mobil();
$b = new Mobil();

// memanggil objek
echo "<b>Mobil pertama</b><br>";
$a->tampilWarna();
echo "<br>Mobil pertama ganti warna<br>";
$a->gantiWarna("Merah");
$a->tampilWarna();

// memanggil objek
echo "<br><b>Mobil kedua</b><br>";
$b->gantiWarna("Hijau");
$b->tampilWarna();

?>
```
![image](https://github.com/user-attachments/assets/ded33b43-33c3-4e1e-ae56-dfee1dd317b1)
Class & Object: Untuk merepresentasikan entitas mobil.   
Encapsulation: Atribut warna, merk, dan harga bersifat privat.  
Constructor: Memberikan nilai awal otomatis pada objek.   
Method: Untuk mengubah dan menampilkan data.   

### Buat file baru dengan nama form.php  
```sh
<?php
/**
* Nama Class: Form
* Deskripsi: Class untuk membuat form inputan text sederhana
**/
class Form
{
    private $fields = array();
    private $action;
    private $submit = "Submit Form";
    private $jumField = 0;

    public function __construct($action, $submit)
    {
        $this->action = $action;
        $this->submit = $submit;
    }

    public function displayForm()
    {
        echo "<form action='".$this->action."' method='POST'>";
        echo '<table width="100%" border="0">';
        for ($j = 0; $j < count($this->fields); $j++) {
            echo "<tr><td align='right'>".$this->fields[$j]['label']."</td>";
            echo "<td><input type='text' name='".$this->fields[$j]['name']."'></td></tr>";
        }
        echo "<tr><td colspan='2'>";
        echo "<input type='submit' value='".$this->submit."'></td></tr>";
        echo "</table>";
        echo "</form>";
    }

    public function addField($name, $label)
    {
        $this->fields[$this->jumField]['name'] = $name;
        $this->fields[$this->jumField]['label'] = $label;
        $this->jumField++;
    }
}

?>
```
tidak muncul file apapun karena filenya belum terisi.

### Buat file baru dengan nama form_input.php
```sh
<?php
/**
* Program memanfaatkan Program 10.2 untuk membuat form inputan sederhana.
**/
include "form.php";
echo "<html><head><title>Mahasiswa</title></head><body>";
$form = new Form("","Input Form");
$form->addField("txtnim", "Nim");
$form->addField("txtnama", "Nama");
$form->addField("txtalamat", "Alamat");
echo "<h3>Silahkan isi form berikut ini :</h3>";
$form->displayForm();
echo "</body></html>";
?>
```
![image](https://github.com/user-attachments/assets/08ab69a8-fb18-453d-95e5-09ba2672f980)  
Kode di atas adalah implementasi dari class Form yang digunakan untuk membuat form input sederhana. Program ini memanfaatkan class Form yang didefinisikan sebelumnya (di file form.php).    

### Contoh lainnya untuk database connection dan query. Buat file dengan nama database.php
```sh
<?php
class Database {
    protected $host;
    protected $user;
    protected $password;
    protected $db_name;
    protected $conn;

    public function __construct() {
        $this->getConfig();
        $this->conn = new mysqli($this->host, $this->user, $this->password, $this->db_name);

        if ($this->conn->connect_error) {
            die("Connection failed: " . $this->conn->connect_error);
        }
    }

    private function getConfig() {
        include_once("config.php");
        global $config; // Pastikan $config bisa diakses
        $this->host = $config['host'];
        $this->user = $config['username'];
        $this->password = $config['password'];
        $this->db_name = $config['db_name'];
    }

    public function query($sql) {
        $result = $this->conn->query($sql);
        if ($result === false) {
            die("SQL Error: " . $this->conn->error);
        }
        return $result;
    }

    public function get($table, $where = null) {
        $condition = $where ? " WHERE $where" : "";
        $sql = "SELECT * FROM $table$condition";
        $result = $this->query($sql);
        return $result->fetch_assoc();
    }

    public function insert($table, $data) {
        if (is_array($data)) {
            $columns = implode(", ", array_keys($data));
            $values = implode(", ", array_map(fn($val) => "'{$this->conn->real_escape_string($val)}'", $data));
            $sql = "INSERT INTO $table ($columns) VALUES ($values)";
            return $this->query($sql);
        }
        return false;
    }

    public function update($table, $data, $where) {
        if (is_array($data)) {
            $update_values = implode(", ", array_map(fn($key, $val) =>
                "$key='{$this->conn->real_escape_string($val)}'", array_keys($data), $data));
            $sql = "UPDATE $table SET $update_values WHERE $where";
            return $this->query($sql);
        }
        return false;
    }

    public function delete($table, $filter) {
        if (!empty($filter)) {
            $sql = "DELETE FROM $table WHERE $filter";
            return $this->query($sql);
        }
        return false;
    }
}
?>
```
file database masih kosong karena belum terisi, maka membuat dulu tablenya dan config.php dan test.php

### Membuat table dalam database mysql 
```sh
MariaDB [(none)]> use latihan1;
Database changed
MariaDB [latihan1]> create table users (
    -> id INT AUTO_INCREMENT PRIMARY KEY,
    -> nama VARCHAR(25) NOT NULL,
    -> email VARCHAR(30) NOT NULL);
Query OK, 0 rows affected (0.035 sec)

MariaDB [latihan1]> INSERT INTO users (nama, email) VALUES
    -> ('Nur Laila', 'nurlaila06@gmail.com'),
    -> ('Dita Tiara', 'dita123@gmail.com');
Query OK, 2 rows affected (0.079 sec)
Records: 2  Duplicates: 0  Warnings: 0
```
setelah membuat table users pada database latihan1, selanjutnya beralih ke vscode dan buat file baru dengan nama config.php

## config.php
```sh
<?php
$config = [
    'host' => 'localhost',     // Host database
    'username' => 'root',      // Username MySQL
    'password' => '',          // Password MySQL
    'db_name' => 'latihan1'    // Nama database
];
?>
```
![image](https://github.com/user-attachments/assets/89ac14ed-2142-41f3-9fdd-047a0aac1ad7)   
berfungsi untuk menyimpan informasi konfigurasi koneksi database.

## test.php
```sh
<?php
// Memasukkan file konfigurasi dan kelas Database
include_once 'config.php';
include_once 'database.php';

// Membuat objek Database dan menghubungkan ke database
try {
    $db = new Database();  // Membuat objek database
    echo "Koneksi ke database berhasil!<br>";
} catch (Exception $e) {
    echo "Koneksi gagal: " . $e->getMessage();
}

// Menguji operasi SELECT
$user = $db->get('users', 'id = 1');  // Mengambil data user dengan ID 1
if ($user) {
    echo "Data User ID 1: <br>";
    echo "Nama: " . $user['nama'] . "<br>";
    echo "Email: " . $user['email'] . "<br>";
} else {
    echo "User tidak ditemukan.<br>";
}

// Menguji operasi UPDATE
$updateData = [
    'nama' => 'Ainun',
    'email' => 'ainun112@gmail.com'
];
$updated = $db->update('users', $updateData, 'id = 1');
if ($updated) {
    echo "Data berhasil diperbarui!<br>";
} else {
    echo "Gagal memperbarui data.<br>";
}

// Menguji operasi DELETE
$deleted = $db->delete('users', 'id = 2');  // Menghapus user dengan ID 2
if ($deleted) {
    echo "Data berhasil dihapus!<br>";
} else {
    echo "Gagal menghapus data.<br>";
}
?>
```
![image](https://github.com/user-attachments/assets/e219977d-2c7c-445f-b007-2e296cf64102)  
![image](https://github.com/user-attachments/assets/3b9500fa-3e00-4297-a933-59782e4cc73d)  
Kode PHP ini menghubungkan aplikasi ke database menggunakan kelas **`Database`** dan melakukan operasi CRUD pada tabel `users`. Pertama, objek **`Database`** dibuat untuk menghubungkan ke database; jika koneksi berhasil, pesan "Koneksi ke database berhasil!" akan ditampilkan. Kemudian, data pengguna dengan `id = 1` diambil menggunakan metode **`get`** dan ditampilkan jika ditemukan. Selanjutnya, operasi **`update`** dilakukan untuk memperbarui data pengguna dengan `id = 1`, mengganti nama dan email, dan menampilkan pesan keberhasilan atau kegagalan. Terakhir, operasi **`delete`** menghapus data pengguna dengan `id = 2`, dengan pesan yang sesuai tergantung pada hasilnya.    

# TUGAS DAN JAWABAN
Implementasikan konsep modularisasi pada kode program pada praktikum sebelumnya dengan menggunakan class library untuk form dan database connection.  

## 1. Memuat file database2.php pada folder lab10_php_oop
![image](https://github.com/user-attachments/assets/802a612a-0538-48ff-83c4-bbacb763482d)
```sh
<?php
class Database {
    protected $host;
    protected $user;
    protected $password;
    protected $db_name;
    protected $conn;

    public function __construct() {
        $this->getConfig();
        // Cek koneksi sebelum melanjutkan
        $this->conn = new mysqli($this->host, $this->user, $this->password, $this->db_name);

        if ($this->conn->connect_error) {
            die("Connection failed: " . $this->conn->connect_error);
        }
    }

    private function getConfig() {
        include_once("config.php");
        global $config;
        $this->host = $config['host'];
        $this->user = $config['username'];
        $this->password = $config['password'];
        $this->db_name = $config['db_name'];
    }

    public function query($sql) {
        try {
            $result = $this->conn->query($sql);
            if ($result === false) {
                throw new Exception("SQL Error: " . $this->conn->error);
            }
            return $result;
        } catch (Exception $e) {
            die("Database Query Error: " . $e->getMessage());
        }
    }

    public function insert($table, $data) {
        if (is_array($data)) {
            $columns = implode(", ", array_keys($data)); // Kolom: nama, email
            $values = implode(", ", array_map(fn($val) => "'{$this->conn->real_escape_string($val)}'", $data));
            $sql = "INSERT INTO $table ($columns) VALUES ($values)";
            return $this->query($sql);
        }
        return false;
    }

    public function get($table, $where = null) {
        $condition = $where ? " WHERE $where" : "";
        $sql = "SELECT * FROM `$table`$condition";
        $result = $this->query($sql);
        return $result->fetch_assoc();
    }

    public function update($table, $data, $where) {
        if (is_array($data)) {
            $update_values = implode(", ", array_map(function ($key, $val) {
                return "$key = '" . $this->conn->real_escape_string($val) . "'";
            }, array_keys($data), $data));
            $sql = "UPDATE `$table` SET $update_values WHERE $where";
            return $this->query($sql);
        }
        return false;
    }

    public function delete($table, $filter) {
        if (!empty($filter)) {
            $sql = "DELETE FROM `$table` WHERE $filter";
            return $this->query($sql);
        }
        return false;
    }

    public function close() {
        $this->conn->close();
    }
}
?>
```
Kode di atas adalah kelas `Database` untuk mengelola koneksi dan operasi database MySQL secara modular. Fitur utamanya mencakup koneksi otomatis, eksekusi query, dan operasi CRUD (Create, Read, Update, Delete) dengan penanganan error. Kelas ini membaca konfigurasi dari file `config.php` dan memastikan keamanan data dengan escaping input.    

## 2. Membuat file form2.php
```sh
<?php
class Form {
    private $fields = [];
    private $action;
    private $submit;

    public function __construct($action, $submit) {
        $this->action = $action;
        $this->submit = $submit;
    }

    public function addField($name, $label) {
        $this->fields[] = ['name' => $name, 'label' => $label];
    }

    public function displayForm() {
        echo "<form action='{$this->action}' method='POST'>";
        echo "<table>";
        foreach ($this->fields as $field) {
            echo "<tr>
                    <td>{$field['label']}</td>
                    <td><input type='text' name='{$field['name']}'></td>
                  </tr>";
        }
        echo "<tr>
                <td colspan='2'><button type='submit'>{$this->submit}</button></td>
              </tr>";
        echo "</table>";
        echo "</form>";
    }
}
?>
```
Kode ini saya buat untuk mempermudah pembuatan formulir dinamis dengan PHP. Kelas `Form` memungkinkan saya menentukan *action* dan tombol submit, lalu menambahkan field input dengan label melalui metode `addField`. Formulir ditampilkan otomatis dalam format tabel dengan `displayForm`, sehingga lebih efisien dan minim kesalahan dibanding menulis HTML manual.    

## 3. Membuat form_input2.php
```sh
<?php
include_once 'form2.php';
include_once 'database2.php';

// Menampilkan form
echo "<html><head><title>Mahasiswa</title></head><body>";
$form = new Form("insert.php", "Submit Data");
$form->addField("nama", "Nama");
$form->addField("email", "Email");
echo "<h3>Silakan isi form berikut:</h3>";
$form->displayForm();
echo "</body></html>";
?>
```
Kode ini menampilkan formulir HTML untuk mengisi nama dan email mahasiswa menggunakan kelas `Form`. Formulir dibuat dinamis, diarahkan ke `insert.php` saat disubmit, dan ditampilkan dengan struktur HTML lengkap.    

## 4. config1.php
```sh
<?php
$config = [
    'host' => 'localhost',
    'username' => 'root', 
    'password' => '',    
    'db_name' => 'latihan1' 
];
?>
```

## 5. Membuat file insert.php
```sh
<?php
include_once 'config.php';
include_once 'database2.php';

// Proses data dari form
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $nama = $_POST['nama'];
    $email = $_POST['email'];

    $db = new Database();
    $data = [
        'nama' => $nama,
        'email' => $email
    ];

    // Menyimpan data ke tabel users
    if ($db->insert('users', $data)) {
        echo "Data berhasil disimpan!";
    } else {
        echo "Gagal menyimpan data!";
    }
}
?>
```
Data nama dan email yang diinput oleh pengguna disimpan ke tabel users dalam database menggunakan kelas Database. Jika penyimpanan berhasil, akan ditampilkan pesan "Data berhasil disimpan!", dan jika gagal, akan muncul pesan error.     
![image](https://github.com/user-attachments/assets/8af3f74e-354d-44af-92d1-50df4c95dc67)    
![image](https://github.com/user-attachments/assets/e6a99230-f9f6-4d39-9452-bde7935a25e3)   
![image](https://github.com/user-attachments/assets/8876906c-eca6-4deb-86bf-db69cf48f818)
![image](https://github.com/user-attachments/assets/21e7e87d-8381-4752-894f-5ff0ae343100)  
terdapat table dengan id 1 dan 3 saja karena pada test.php sebelumnya id dengan nomor 2 telah di hapus

## ujicoba.php
```sh
<?php
include_once 'config.php';
include_once 'database2.php';

// Membuat objek Database dan menghubungkan ke database
try {
    $db = new Database();  // Membuat objek database
    echo "Koneksi ke database berhasil!<br>";
} catch (Exception $e) {
    echo "Koneksi gagal: " . $e->getMessage();
}

// Menguji operasi SELECT
$user = $db->get('users', 'id = 7');  
if ($user) {
    echo "Data User ID 7: <br>";
    echo "Nama: " . $user['nama'] . "<br>";
    echo "Email: " . $user['email'] . "<br>";
} else {
    echo "User tidak ditemukan.<br>";
}

// Menguji operasi UPDATE
$updateData = [
    'nama' => 'Dita',
    'email' => 'dita123@gmail.com'
];
$updated = $db->update('users', $updateData, 'id = 1');
if ($updated) {
    echo "Data berhasil diperbarui!<br>";
} else {
    echo "Gagal memperbarui data.<br>";
}

// Menguji operasi DELETE
$deleted = $db->delete('users', 'id = 3');  
if ($deleted) {
    echo "Data berhasil dihapus!<br>";
} else {
    echo "Gagal menghapus data.<br>";
}
?>
```
![image](https://github.com/user-attachments/assets/6124e66d-2342-46b4-a9a7-ffa5d52af0d3)   
![image](https://github.com/user-attachments/assets/5e8840d4-9b91-4f9b-a220-5c1656c62e86)   



