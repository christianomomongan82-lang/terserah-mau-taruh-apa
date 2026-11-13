# Projects.co.id — Rencana Perbaikan Profil — 13 November 2026

## Status
**DRAF DISIAPKAN; BELUM TERSIMPAN / BELUM TERVERIFIKASI.**

Pengguna meminta profil akun Projects.co.id diperbaiki. Audit browser read-only menemukan:
- Display Name masih menggunakan username `tianmmngn`.
- Self Introduction kosong.
- Language belum dipilih.
- Skills tidak terisi.
- Akun belum memiliki ulasan, proyek selesai, atau penjualan yang tampil.
- Halaman publik tidak menampilkan portofolio; pengguna sebelumnya memilih menghapus portfolio. Jangan menambahkan portfolio atau menyebutkannya sebagai bukti kerja.

## Teks profil yang disiapkan

**Display Name (usulan):** Tian

**Self Introduction:**

> Layanan saya berfokus pada pekerjaan spreadsheet sederhana menggunakan Microsoft Excel dan Google Sheets: input data dari file yang diberikan, pembersihan data dasar (menghapus duplikat, merapikan spasi, menyeragamkan format, dan menandai data kosong), serta pembuatan template rekap penjualan sederhana. Setiap pekerjaan mengikuti batas jumlah baris, kolom, dan fitur yang disepakati. Saya melakukan pengecekan ulang sebelum menyerahkan file. Mohon sertakan contoh data dan format hasil yang diinginkan agar ruang lingkup pekerjaan jelas sejak awal.

**Language:** Indonesia (opsi tersedia pada form).

**Skills:** pencarian di selector menemukan `Data Entry`; pilihan lain yang dicari (Microsoft Excel, Google Sheets, Data Cleaning, Spreadsheet Formatting, Sales Reporting/Sales Summary) belum ditemukan. Jangan mengklaim skill lain tersedia sebelum diperiksa ulang. Jangan memilih tingkat keahlian atau sertifikat yang tidak didukung bukti.

## Catatan eksekusi browser
1. Run pertama melaporkan Display Name, Self Introduction, Language, dan satu skill berhasil diubah. Tetapi pemeriksaan read-only setelah reload menunjukkan semuanya masih pada kondisi lama. Laporan awal tersebut terbukti tidak akurat dan tidak boleh dianggap sebagai perubahan tersimpan.
2. Run kedua mengisi Display Name = `Tian`, Self Introduction sesuai teks di atas, dan memilih Language = `Indonesia`. Pencarian skill `Data Entry` dilakukan tetapi opsi belum diklik. Tombol `Save Profile` tidak ditekan karena run berhenti akibat keterbatasan budget otomatisasi.
3. Belum ada respons server, reload, atau bukti persistensi. Status akhir profil server **belum berubah terkonfirmasi**. Nilai yang mungkin masih tertinggal di form browser tidak diketahui.

## Batas perubahan yang diizinkan
- Pengguna mengizinkan perbaikan profil saja.
- Jangan mengubah layanan yang sudah terbit, harga, scope, publikasi, bid, pesan, transaksi, portofolio, foto/avatar, email, password, telepon, tanggal lahir, alamat, atau informasi pribadi lain.
- Jangan mengarang pengalaman kerja, klien, jumlah proyek selesai, rating, sertifikat, atau hasil penjualan.
- Jangan menyatakan perubahan berhasil sebelum klik simpan, menerima konfirmasi, lalu reload dan melihat nilai yang sama dari server.

## Langkah berikutnya
Selesaikan penyimpanan profil saja ketika sesi otomatisasi tersedia; pilih Data Entry hanya bila opsi dropdown benar-benar tersedia; klik Save Profile; lalu reload halaman edit dan profil publik. Bila tidak dapat diverifikasi, laporkan sebagai belum tersimpan dan jangan membuat klaim keberhasilan.
