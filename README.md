<table>
  <tr>
    <th colspan="2">DATA MAHASISWA</th>
  </tr>
  <tr>
    <td>Nama</td>
    <td>Muhammad Arifin</td>
  </tr>
  <tr>
    <td>NIM</td>
    <td>312210330</td>
  </tr>
  <tr>
    <td>Kelas</td>
    <td>TI.22.A3</td>
  </tr>
</table>

# <p align="center">Praktikum9 : PHP Modular</p>

# Instruksi Praktikum

1. Persiapkan text editor misalnya VSCode.
   
2. Buat folder baru dengan nama lab9_php_modular pada docroot webserver (htdocs)
   
3. Ikuti langkah-langkah praktikum yang akan dijelaskan berikutnya.

- Jalankan XAMPP dan akses http://localhost/phpmyadmin/ untuk membuat database.

![286815472](https://github.com/marifinnnnn/Lab9Web/assets/115518274/00d5dc0b-f0d2-43f5-ade3-a736b34d769b)

- Buat file lab9_php_modular pada root directory web server (c:\xampp\htdocs)

![286815830](https://github.com/marifinnnnn/Lab9Web/assets/115518274/422b20cc-047e-4ba4-9a10-b8a930f6809a)

# Langkah-langkah praktikum

- Buat file baru dengan nama <b>header.php</b>

      <!DOCTYPE html>
      <html lang="en">
      <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=`device-width`, initial-scale=1.0">
        <title>Contoh Modularisasi</title>
        <link rel="stylesheet" href="style.css">
      </head>
      <body>
        <div class="container">
            <header>
                <h1>Modularisasi Menggunakan Require</h1>
            </header>
            <nav>
                <a href="home.php">Home</a>
                <a href="about.php">Tentang</a>
                <a href="kontak.php">Kontak</a>
            </nav>

Buat file dengan nama footer.php

            <footer>
                <p>&copy; 2021, Informatika, Universitas Pelita Bangsa</p>
            </footer>
        </div>
    </body>
    </html>

Buat file nama dengan home.php

    <?php require("header.php"); ?>
    
    <div>
        <h2>Ini halaman home</h2>
        <p>Ini bagian content dari halaman</p>
    </div>
    
    <?php require("footer.php"); ?>

Buat file dengan nama about.php

    <?php require('header.php') ?>
    
    <div>
        <h2>Ini halaman about</h2>
        <p>Ini adalah bagian content dari halaman</p>
    </div>
    
    <?php require('footer.php') ?>

## Tugas
Implementasikan konsep modularisasi pada kode program praktikum 8 tentang
database, sehingga setiap halamannya memiliki template tampilan yang sama.


### Config folder
config/koneksi.php

    <?php
    $host = 'localhost';
    $user = 'root';
    $pass = '';
    $db = 'latihan1';
    
    $conn = mysqli_connect($host, $user, $pass, $db);
    
    if ($conn = false) {
        echo 'Koneksi ke server gagal';
        die();
    };
    
    ?>

config/index.php

    <?php 
    include("koneksi.php");
    
    $sql = 'SELECT * FROM data_barang';
    $conn = mysqli_connect($host, $user, $pass, $db);
    $result = mysqli_query($conn, $sql);
    ?>

config/tambah.php

    <?php
    error_reporting(E_ALL);
    include_once 'koneksi.php';
    
    if (isset($_POST['submit'])) {
        $nama = $_POST['nama'];
        $kategori = $_POST['kategori'];
        $harga_jual = $_POST['harga_jual'];
        $harga_beli = $_POST['harga_beli'];
        $stok = $_POST['stok'];
        $file_gambar = $_FILES['file_gambar'];
        $gambar = null;
        if ($file_gambar['error'] == 0) {
            $filename = str_replace(' ', '_', $file_gambar['name']);
            $destination = dirname(__FILE__) . '/gambar/' . $filename;
            if (move_uploaded_file($file_gambar['tmp_name'], $destination)) {
                $gambar = 'gambar/' . $filename;
            }
        }
        
        $sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli,
        stok, gambar) ';
        $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}',
        '{$harga_beli}', '{$stok}', '{$gambar}')";
        $conn = mysqli_connect($host, $user, $pass, $db);
        $result = mysqli_query($conn, $sql);
        header('location: index.php');
    }
    ?>

config/ubah.php

    <?php
    error_reporting(E_ALL);
    include_once 'koneksi.php';
    
    if (isset($_POST['submit'])) {
        $id = $_POST['id'];
        $nama = $_POST['nama'];
        $kategori = $_POST['kategori'];
        $harga_jual = $_POST['harga_jual'];
        $harga_beli = $_POST['harga_beli'];
        $stok = $_POST['stok'];
        $file_gambar = $_FILES['file_gambar'];
        $gambar = null;
    
        if ($file_gambar['error'] == 0) {
            $filename = str_replace(' ', '_', $file_gambar['name']);
            $destination = dirname(__FILE__) . '/gambar/' . $filename;
            if (move_uploaded_file($file_gambar['tmp_name'], $destination)) {
                $gambar = 'gambar/' . $filename;
            }
        }
    
        $sql = 'UPDATE data_barang SET ';
        $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
        $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok = '{$stok}' ";
    
        if (!empty($gambar))
            $sql .= ", gambar = '{$gambar}' ";
        
        $sql .= "WHERE id_barang = '{$id}'";
        $conn = mysqli_connect($host, $user, $pass, $db);
        $result = mysqli_query($conn, $sql);
    
        header('location: index.php');
    }
    
    $id = $_GET['id'];
    $conn = mysqli_connect($host, $user, $pass, $db);
    $sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
    $result = mysqli_query($conn, $sql);
    if (!$result) die('Error: Data tidak tersedia');
    $data = mysqli_fetch_array($result);
    
    function is_select($var, $val) {
        if ($var == $val) return 'selected="selected"';
        return false;
    }
    ?>

config/hapus.php

    <?php
    include_once 'koneksi.php';
    $id = $_GET['id'];
    $sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
    $conn = mysqli_connect($host, $user, $pass, $db);
    $result = mysqli_query($conn, $sql);
    header("location: ../index.php");
    ?>

### Layout folder

layout/head.php

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <link rel="stylesheet" href="../tugas/css/style.css" type="text/css">

layout/index.php

      <title>Data Barang</title>
    </head>
    <body>
        <div class="container">
            <h1>Data Barang</h1>
            <div class="main">
                <a href="tambah.php">Tambah Barang</a>
                <table>
                    <tr>
                        <th>Gambar</th>
                        <th>Nama Barang</th>
                        <th>Kategori</th>
                        <th>Harga Jual</th>
                        <th>Harga Beli</th>
                        <th>Stok</th>
                        <th>Aksi</th>
                    </tr>
                    <?php if ($result): ?>
                        <?php while ($row = mysqli_fetch_array($result)): ?>
                            <tr>
                                <td><img src="gambar/<?=$row['gambar'];?>" alt="<?= $row['nama'];?>"></td>
                                <td><?= $row['nama']; ?></td>
                                <td><?= $row['kategori']; ?></td>
                                <td><?= $row['harga_jual']; ?></td>
                                <td><?= $row['harga_beli']; ?></td>
                                <td><?= $row['stok']; ?></td>
                                <td>
                                    <a href="ubah.php?id=<?= $row['id_barang']; ?>">Ubah</a>
                                    <a href="config/hapus.php?id=<?= $row['id_barang']; ?>">Hapus</a> 
                                </td>
                            </tr>
                    <?php endwhile; else: ?>
                        <tr>
                            <td colspan="7">Belum ada data</td>
                        </tr>
                    <?php endif; ?>
                </table>
            </div>
        </div>
    </body>
    </html>

layout/tambah.php

    <title>Tambah Barang</title>
    </head> 
    <body>
        <div class="container">
            <h1>Tambah Barang</h1>
            <div class="main">
                <form action="tambah.php" method="post" enctype="multipart/form-data">
                    <div class="input">
                        <label>Nama Barang</label>
                        <input type="text" name="nama" />
                    </div>
                    <div class="input">
                        <label>Kategori</label>
                        <select name="kategori">
                            <option value="Komputer">Komputer</option>
                            <option value="Elektronik">Elektronik</option>
                            <option value="Handphone">Handphone</option>
                        </select>
                    </div>
                    <div class="input">
                        <label>Harga Jual</label>
                        <input type="text" name="harga_jual" />
                    </div>
                    <div class="input">
                        <label>Harga Beli</label>
                        <input type="text" name="harga_beli" />
                    </div>
                    <div class="input">
                        <label>Stok</label>
                        <input type="text" name="stok" />
                    </div>
                    <div class="input">
                        <label>File Gambar</label>
                        <input type="file" name="file_gambar" />
                    </div>
                    <div class="submit">
                        <input type="submit" name="submit" value="Simpan">
                    </div>
                </form>
            </div>
        </div>
    </body>
    
    </html>

layout/ubah.php

    <title>Ubah Barang</title>
    </head> 
    <body>
    <div class="container">
        <h1>Ubah Barang</h1>
        <div class="main">
            <form method="post" action="ubah.php" enctype="multipart/form-data">
                <div class="input">
                    <label>Nama Barang</label>
                    <input type="text" name="nama" value="<?php echo $data['nama'];?>" />
                </div>
                <div class="input">
                    <label>Kategori</label>
                    <select name="kategori">
                        <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
                        <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
                        <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
                    </select>
                </div>
                <div class="input">
                    <label>Harga Jual</label>
                    <input type="text" name="harga_jual" value="<?php echo $data['harga_jual'];?>" />
                </div>
                <div class="input">
                    <label>Harga Beli</label>
                    <input type="text" name="harga_beli" value="<?php echo $data['harga_beli'];?>" />
                </div>
                <div class="input">
                    <label>Stok</label>
                    <input type="text" name="stok" value="<?php echo $data['stok'];?>" />
                </div>
                <div class="input">
                    <label>File Gambar</label>
                    <input type="file" name="file_gambar" />
                </div>
                <div class="submit">
                    <input type="hidden" name="id" value="<?php echo $data['id_barang'];?>" />
                        <input type="submit" name="submit" value="Simpan" />
                </div>
            </form>
        </div>
    </div>
    </body>

### Main file

index.php

    <?php include('config/index.php') ?>
    <?php include('layout/head.php') ?>
    <?php include('layout/index.php') ?>

tambah.php

    <?php include('config/tambah.php'); ?>
    <?php include('layout/head.php'); ?>
    <?php include('layout/tambah.php'); ?>

ubah.php

    <?php include('config/ubah.php'); ?>
    <?php include('layout/head.php'); ?>
    <?php include('layout/ubah.php'); ?>

hasil:
![287325428](https://github.com/marifinnnnn/Lab9Web/assets/115518274/9684f36d-7dad-4f6b-abe3-2638ff5ba2f0)

![287325684](https://github.com/marifinnnnn/Lab9Web/assets/115518274/a827594c-cd91-4741-8405-fe7569f25125)

![287325710](https://github.com/marifinnnnn/Lab9Web/assets/115518274/0517b7a4-f2d2-4bbc-83ec-dcd1722c1e89)
