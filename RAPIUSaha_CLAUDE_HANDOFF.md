# RapiUsaha — Paket Konteks untuk Claude

Dokumen ini dibuat agar Claude dapat memahami proyek tanpa konektor GitHub. Gunakan sebagai konteks awal, tetapi bila ada perbedaan dengan instruksi terbaru pengguna, instruksi terbaru pengguna yang berlaku.

**Sumber kanonis:** https://github.com/christianomomongan82-lang/terserah-mau-taruh-apa  
**Dokumen aturan:** [CHATGPT_WORKING_RULES.md](https://github.com/christianomomongan82-lang/terserah-mau-taruh-apa/blob/main/CHATGPT_WORKING_RULES.md)  
**Indeks/checkpoint:** [PROJECT_CONTROL.md](https://github.com/christianomomongan82-lang/terserah-mau-taruh-apa/blob/main/PROJECT_CONTROL.md)  
**Tanggal snapshot:** 2026-11-12

## 1. Identitas dan arah proyek

- **Nama:** RapiUsaha.
- **ID proyek:** P-DIGITAL.
- **Status:** proyek aktif.
- **Model:** membuat produk digital sendiri untuk konsumen/UMKM dan menjualnya berulang kali.
- **Bukan:** jasa studio konten, jasa pengelolaan media sosial, affiliate software, bisnis rental, atau SaaS. Jangan mengubah arah ini tanpa persetujuan eksplisit pengguna.
- Akun Instagram RapiUsaha sudah dibuat pengguna; handle dan kelengkapan profil belum diverifikasi.

## 2. Keputusan yang sudah dikonfirmasi pengguna (USER_CONFIRMED)

- Membuat produk digital sendiri dan menjualnya berulang kali.
- Segmen pelanggan, masalah yang diselesaikan, kategori dan format produk belum dipilih; cari peluang yang paling kuat secara komersial.
- Kanal yang diterima: marketplace/toko digital, website/halaman penjualan dengan checkout otomatis, Google/SEO, Instagram/TikTok hanya untuk promosi produk utama.
- Menghindari telepon/video call pelanggan, cold-DM/mengejar prospek satu per satu, banyak chat pelanggan, teknologi rumit/tidak andal, dan pekerjaan manual berulang.
- Dukungan pascapembelian harus minimal; produk idealnya self-service.
- Modal awal Rp0.
- Waktu kerja lebih dari 6 jam per hari.
- Target pemasukan pertama secepat mungkin, idealnya kurang dari 1 bulan.
- Bersedia bekerja keras di awal agar operasi bisa diotomatisasi.
- Target pemasukan 30 hari: Rp100.000–Rp300.000.
- Prioritas: seimbang antara pemasukan awal dan potensi jangka panjang.
- Pengguna ingin mengelola pembayaran/payout sendiri dan tidak menghendaki keterlibatan orang tua/wali.
- Pengguna menyatakan memiliki rekening BRI dan e-wallet DANA. Belum diverifikasi apakah keduanya atas nama sendiri, lolos KYC, bisa menerima pembayaran komersial, atau memenuhi syarat payout.

## 2A. Strategi pemasaran terbaru (USER_CONFIRMED; 2026-11-12)

- Harga produk harus cukup terjangkau untuk menarik pembeli, sementara kualitas produk dan pengalaman pakai menjadi pembeda.
- Produk akan ditempatkan di marketplace yang cocok untuk produk digital; marketplace final belum dipilih.
- Instagram dan TikTok akan dipakai untuk promosi dengan video visual yang menarik, bukan hanya posting teks.
- Pengguna tidak ingin mengedit video. Asisten bertanggung jawab menghasilkan materi video promosi final yang siap diunggah; belum ada video yang dibuat.
- Produk spesifik, segmen prioritas, harga, marketplace, dan konsep video masih OPEN sampai riset dan rekomendasi selesai.
- Jangan menganggap generic HPP spreadsheet atau Paket Operasional Jualan WhatsApp sebagai produk final hanya karena pernah diusulkan.

## 2B. Riset marketplace dan kandidat produk (2026-11-12; rekomendasi asisten, belum dikunci pengguna)

- Riset lengkap tersimpan di [research/RapiUsaha_marketplace_channel_2026-11-12.md](research/RapiUsaha_marketplace_channel_2026-11-12.md).
- **Etsy adalah kandidat marketplace utama jangka panjang** karena Q3 2026 menyebut sekitar 88 juta active buyers (TTM), mengizinkan instant digital downloads, dan Etsy Payments tersedia di Indonesia. Ini rekomendasi asisten, belum keputusan final pengguna.
- Etsy mewajibkan pemilik akun minimal 18 tahun. Penjual 13–17 tahun hanya boleh melalui akun wali dengan izin dan pengawasan langsung serta data keuangan atas nama wali. Pengguna tidak ingin keterlibatan wali, jadi jangan membuka akun Etsy sebelum syarat usia mandiri terpenuhi.
- Biaya Etsy untuk bank Indonesia: listing US$0,20; transaction fee 6,5%; processing 4,5% + Rp7.000/order; deposit minimum Rp28.000 dan deposit fee Rp28.000 jika deposit di bawah threshold Rp1.400.000. Periksa ulang saat pendaftaran karena fee dapat berubah.
- Shopee memiliki traffic tinggi tetapi program resmi penjualan produk digital yang ditemukan terbatas pada e-voucher dari Partner yang disebut dalam lampiran perjanjian. Jangan mengasumsikan listing biasa dapat menjual file spreadsheet/PDF.
- Kreavo menerima pengguna minimal 17 tahun menurut Terms, tetapi halaman publiknya saat diperiksa menampilkan 0+ seller/produk dan 0 kreator/produk terjual; tidak ada bukti marketplace discovery ramai. Hanya pertimbangkan sebagai checkout sementara yang diarahkan traffic sendiri setelah payout diverifikasi.
- Tokoge mendukung file digital, tetapi usia minimum dan skala pembeli belum jelas; jangan menetapkannya sebagai marketplace utama.
- **Kandidat produk asisten untuk riset/prototipe:** Home Bakery / Cottage Bakery Order & Cost System. Etsy menampilkan 1.000+ hasil kategori bakery planner/order tracker. Salah satu listing digital mendapat badge Bestseller, lebih dari 20 views/24 jam dan harga promo US$8,75; tokonya memiliki 117,4 ribu penjualan keseluruhan toko, bukan listing itu sendiri. Pasar memiliki pembeli, tetapi kompetisi dan diskon berat.
- Belum ada produk yang disetujui final, belum ada listing, video, atau transaksi. Langkah berikut: cek gap produk/pesaing, buat prototipe phone-first yang sederhana, uji fungsi dan harga, siapkan video promosi. Jangan mencampur angka toko dengan penjualan listing, dan jangan menjadikan volume hasil pencarian sebagai data transaksi.

## 2C. Status prototipe dan video promosi (2026-11-12; belum produk final)

- Asisten membuat prototipe XLSX bernama `RapiPO_Home_Bakery_Order_Profit_Tracker_Prototype.xlsx` dalam percakapan, dengan sheet Start Here, Menu & Cost, Orders, dan Dashboard.
- Kalkulasi contoh diuji: sales US$123.00; estimated gross profit US$60.20; unpaid balance US$68.00; 3 active demo orders. Scan formula tidak menemukan error umum.
- Asisten membuat video teaser vertical `RapiPO_Prototype_Promo_Vertical.mp4`, 720×1280, 30fps, 18 detik, dengan musik orisinal. Video menyatakan prototype/demo; bukan pengumuman produk telah dijual.
- Kedua file adalah lampiran percakapan, belum di-commit sebagai binary ke GitHub. Belum diuji di aplikasi Google Sheets Android dan belum diuji oleh pembeli.
- RapiPO dan home bakery masih rekomendasi/proposal asisten; belum keputusan produk final eksplisit dari pengguna. Jangan menyatakan produk tervalidasi atau sudah diluncurkan.
- Next: uji XLSX pada Google Sheets Android, tambah order notes/change tracking jika memang membantu, buat bahasa Indonesia dan buyer guide, lalu verifikasi checkout/payout Kreavo dan siapkan Etsy setelah usia/payout memenuhi syarat.

## 2D. Filosofi bisnis dan standar memilih produk

- Dokumen kerja: [RapiUsaha_DASAR_FILOSOFI_DAN_FILTER_PRODUK.md](RapiUsaha_DASAR_FILOSOFI_DAN_FILTER_PRODUK.md).
- Gagasan utama: RapiUsaha menjual solusi yang layak dibayar; produk digital hanyalah kemasan dan distribusinya.
- Standar: mulai dari masalah nyata; jual hasil bukan fitur; buktikan kualitas lewat penggunaan; fokus satu segmen dan satu pekerjaan penting; buat MVP kecil yang sangat berguna; harga terjangkau tapi margin sehat; produk dan distribusi dirancang bersama; trust adalah bagian dari produk; validasi sebelum scale; hitung unit economics dan beban support.
- Bukti transaksi/repeat purchase paling kuat. Preorder/deposit/komitmen membayar kuat. Keluhan berulang/review/penjualan pesaing menengah. Likes, views, jumlah followers, dan hasil pencarian adalah sinyal lemah.
- RapiPO home bakery tetap kandidat/prototipe asisten, belum produk final yang disetujui pengguna. Mobile-first adalah hipotesis diferensiasi yang harus dibuktikan.
- Jangan terus mengembangkan RapiPO hanya karena sudah dibuat. Bandingkan alternatif dengan rubrik sebelum mengunci produk.

## 2E. Hasil perbandingan produk dan audit kompetitor (2026-11-12)

- Riset lengkap: [research/RapiUsaha_product_opportunity_comparison_2026-11-12.md](research/RapiUsaha_product_opportunity_comparison_2026-11-12.md).
- Home bakery tetap memiliki bukti permintaan produk/listing paling kuat di antara kandidat yang diperiksa (termasuk listing Bestseller dan listing dengan 61 ulasan item 4.8/5).
- **RapiPO generic all-in-one belum layak diluncurkan.** Kompetitor sudah menawarkan spreadsheet 11–14 tab dengan order/DP, recipe costing, inventory, CRM, expenses, dashboard, production planning, dan ada order form Google Forms yang auto-save ke Google Sheets.
- Mobile-first/simple bukan pembeda yang sudah terbukti; beberapa produk pesaing sudah mengklaim/mendukung Google Sheets mobile.
- Cleaning pricing calculators menawarkan harga/nilai problem lebih tinggi, tetapi bukti permintaan item-spesifik yang diperiksa masih lemah; nail tech kompetitif dan harga tertekan; photography lebih kompleks; sourdough microbakery sudah punya planner/preorder/fermentation competitors.
- Rekomendasi asisten: jangan menambah fitur pada RapiPO generik. Cari satu masalah/pekerjaan spesifik yang belum dipenuhi dengan baik dan buktikan willingness-to-pay. Order revision/version history dan weekly preorder capacity hanya ide riset, belum divalidasi.
- Tidak ada produk/segmen/harga/marketplace final yang disetujui pengguna. RapiPO tetap proposal/prototipe asisten.

## 3. Batas usia dan pembayaran

- Saat snapshot dibuat, pengguna berusia 17 tahun.
- Setiap platform pembayaran, marketplace, dan payout harus diperiksa berdasarkan ketentuan resmi terbaru.
- Jangan menyarankan pemalsuan usia, penggunaan identitas orang lain, atau menganggap rekening BRI/DANA otomatis memenuhi syarat.
- Jangan menganggap pengguna setuju melibatkan wali. Jika jalur mandiri tidak legal/diizinkan, jelaskan hambatannya dan cari alternatif sah yang sesuai.

## 4. Status produk dan keputusan

**Belum diputuskan (OPEN):**
- Produk spesifik dan masalah pelanggan.
- Segmen prioritas.
- Harga.
- Platform penjualan, checkout, dan payout yang sah untuk usia pengguna.
- Strategi distribusi final.

**Usulan asisten (ASSISTANT_PROPOSAL, belum disetujui):**
- “Paket Operasional Jualan WhatsApp” untuk UMKM/penjual rumahan. Komponen yang pernah diusulkan: pencatatan order/pembayaran, kalkulator harga/margin, katalog, template pesan pelanggan, dan panduan singkat.
- Perlakukan hanya sebagai hipotesis yang boleh dibandingkan dengan peluang lain. Jangan membangun atau menyebutnya sebagai produk final/tervalidasi tanpa persetujuan pengguna.

**Ditolak/diarsipkan (REJECTED/ARCHIVED):**
- Bisnis jasa/studio produksi konten dan bisnis yang menjual konten. Konten hanya boleh digunakan untuk promosi produk utama.
- P-DISTRO/affiliate software, P-RENTAL, SignageQuote, dan proyek lain adalah proyek/arsip terpisah, bukan bagian dari RapiUsaha. Jangan mencampur aset atau strateginya.

## 5. Status aktual pada 2026-11-12

- Profil bisnis dan batasan pengguna sudah dicatat di GitHub dan diverifikasi.
- Rekening BRI dan DANA sudah dicatat di GitHub dan diverifikasi sebagai informasi yang dinyatakan pengguna; status KYC/payout belum diverifikasi.
- Belum ada produk final yang disetujui.
- Belum ada produk selesai, validasi pelanggan, atau penjualan yang terverifikasi.
- Riset pemilihan produk masih berlangsung dan belum menghasilkan keputusan final. Klaim pasar atau kompetisi harus didukung sumber yang dapat diperiksa; jangan mengarang angka atau menganggap jumlah UMKM sebagai bukti willingness-to-pay.

## 6. Pekerjaan berikutnya

Lakukan riset komparatif untuk memilih satu peluang produk digital B2C yang paling layak. Bandingkan setidaknya:
1. Bukti masalah dan permintaan pelanggan.
2. Bukti kesediaan membayar (bukan hanya jumlah pengguna/pelaku usaha).
3. Kompetisi dan alternatif gratis yang sudah tersedia.
4. Kisaran harga yang benar-benar ditawarkan kompetitor.
5. Waktu dan biaya membuat MVP dengan modal Rp0.
6. Peluang mendapatkan penjualan pertama dalam 30 hari.
7. Beban dukungan dan potensi self-service/otomatisasi.
8. Kanal distribusi yang sesuai dengan batasan pengguna.
9. Legalitas akun, KYC, pembayaran, dan payout bagi pengguna usia 17 tahun.

Gunakan sumber primer dan sumber terbaru bila memungkinkan. Sajikan bukti, tautan, asumsi, kelemahan, serta rekomendasi utama dan alternatif. **Jangan mengunci segmen, produk, harga, atau strategi sebelum pengguna menyetujuinya.**

## 7. Cara bekerja dengan pengguna

- Bahasa Indonesia santai, langsung, kesimpulan di depan.
- Berikan hasil nyata, bukan laporan progres kosong atau daftar tugas yang membebani pengguna.
- Jangan meminta pengguna mengulang informasi yang sudah tertulis di dokumen ini.
- Pisahkan fakta terverifikasi, asumsi, usulan AI, keputusan pengguna, dan hal yang belum diputuskan.
- Jangan mengubah model bisnis/target/produk tanpa persetujuan eksplisit.
- Jika membuat keputusan atau menyelesaikan pekerjaan penting, catat hasilnya di sumber kanonis GitHub dan verifikasi ulang.
- Dokumen ini adalah snapshot untuk memulai konteks Claude, bukan pengganti pemeriksaan sumber kanonis jika akses GitHub tersedia.

## 8. Instruksi awal untuk Claude

Baca dokumen ini seluruhnya. Setelah itu, ringkas kembali identitas RapiUsaha, keputusan USER_CONFIRMED, usulan yang belum disetujui, hal yang ditolak, status aktual, dan langkah riset berikutnya. Jangan langsung membuat produk atau mengubah strategi. Mulailah dari riset peluang produk yang berbasis bukti.
