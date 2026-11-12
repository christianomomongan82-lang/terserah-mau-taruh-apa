# RapiUsaha — Validasi Celah: Perubahan Pesanan Custom dan Pembanding Segmen
Tanggal pemeriksaan: 12 November 2026
Status: riset pembanding; tidak mengunci produk atau segmen final.

## Ringkasan eksekutif

1. Masalah perubahan pesanan custom di home bakery memang nyata: perubahan desain, rasa, ukuran/servings, tier, bunga, bahan khusus, permintaan diet, delivery/setup dan rush fee dapat mengubah biaya, tenaga, deadline dan harga.
2. Namun **jangan membuat RapiPO generic, order revision spreadsheet, atau approval tracker bakery sebagai produk standalone sekarang**. Ada solusi digital yang tepat sasaran:
   - Orders App menangkap pesanan dari WhatsApp/Instagram dan memungkinkan pembaruan detail custom order.
   - GetTheOK secara khusus membantu baker menyetujui perubahan dengan harga, catatan waktu dan riwayat versi. Paket gratis mengizinkan 3 approval/bulan; paket berbayar $9/$19/$29 per bulan.
   - Di Etsy ada template order, capacity, deposit, dan bahkan Bakery Order Change & Pickup Kit pada harga hasil pencarian $2.49.
3. Bukti masalah bukan otomatis bukti bahwa pembeli mau membayar untuk file baru. Solusi gratis atau SaaS lebih lengkap memperlemah alasan pembelian produk spreadsheet statis.
4. Home bakery masih punya sinyal pembelian digital item paling jelas dari segmen yang sebelumnya dibandingkan, tetapi kategori order management/costing/production planner sudah sangat padat dan fungsionalitasnya saling meniru.
5. Segmen pembanding pet grooming dan short-term rental cleaning juga memiliki banyak produk digital serupa, tetapi sinyal listing-level buyer validation yang diperiksa masih lemah/tidak konsisten. Belum cukup untuk mengunci perpindahan segmen.
6. **Keputusan kerja:** RapiPO dihentikan dari ekspansi fitur. Jangan membuat versi final atau meluncurkan produk baru sampai ditemukan celah yang lebih unik dan bukti WTP yang lebih baik.

## A. Bukti masalah home bakery — perubahan pesanan

- GetTheOK menjelaskan skenario wedding cake yang berubah berkali-kali selama berbulan-bulan: flavor dua kali, added tier, kebutuhan gluten-free, dan permintaan mengganti sugar flowers dengan fresh flowers tiga hari sebelum acara. Perubahan semacam itu memengaruhi biaya/tenaga, dan pembeli perlu menyetujui harga sebelum baker membeli bahan.
  Sumber: https://www.gettheok.app/use-cases/bakers
- Halaman itu merinci jenis perubahan: desain, jumlah servings, flavor/filling, bunga, kebutuhan diet, delivery/setup, rush fee, ingredient upgrades, dan add-ons.
- Reddit: home baker pemula melaporkan pelanggan terlambat membayar hingga tiga hari sebelum pickup, terus-menerus mengirim pesan, sangat picky/micromanaging, lalu menekan harga dan membuat baker mengembalikan 30% pembayaran karena takut konflik. Ini contoh pengalaman satu baker, bukan survei populasi.
  Sumber: https://www.reddit.com/r/pastry/comments/1uhu2ko/home_baker_difficult_customer/
- Reddit lain (search result, 2026-11-12) menggambarkan repeat customer mengubah keseluruhan desain kue anak sekitar 1,5 minggu sebelum pickup; baker sebelumnya menetapkan batas perubahan dan kemungkinan biaya tambahan. Ini mendukung bahwa perubahan terlambat dapat memicu konflik, tetapi tidak membuktikan seberapa sering terjadi.
  Sumber: https://www.reddit.com/r/PickAorB/comments/1xjlhsr/a_or_b_a_repeat_customer_wanted_to_change_her/
- Sebuah panduan bakery menyarankan design-lock date, memisahkan perubahan kosmetik vs struktural, re-quote untuk perubahan besar, dan change fee jika perubahan terlambat.
  Sumber: https://cakeintakr.com/blog/late-cake-order-changes/

## B. Pengganti/kompetitor yang sudah menyelesaikan masalah

### Orders App
- Menghubungkan WhatsApp Business/Instagram melalui API resmi Meta atau menerima paste order, lalu merapikan produk, ukuran, rasa, jumlah, instruksi, alamat dan status pembayaran.
- Detail pesanan bisa diedit ketika pelanggan mengubah kue, jumlah, kustomisasi atau pengiriman.
- Sumber: https://orders.app/id/order-management-for-home-bakers
- Ini adalah software operasional (bukan template download) dan kompetitor lebih lengkap untuk pencatatan chat/order daripada spreadsheet statis.

### GetTheOK
- Halaman spesifik untuk bakers mempromosikan approval perubahan: satu request, satu harga, satu keputusan; buyer bisa approve, request a change atau decline melalui link tanpa membuat akun; setiap revisi tersimpan, versi sebelumnya tidak ditimpa.
- Mendukung price impact (tambahan biaya, revised total, credit/reduction, atau no price change), lampiran di paket berbayar, download PDF record dan riwayat.
- Paket yang ditampilkan pada saat pemeriksaan: Free $0 (3 approvals/bulan, 1 active client & job, 1 attachment per approval); Starter $9/bulan (20 approvals/bulan); Solo $19/bulan (75 approvals/bulan); Pro $29/bulan (250 approvals/bulan).
- Sumber: https://www.gettheok.app/use-cases/bakers ; https://www.gettheok.app/
- Implikasi: produk spreadsheet/approval PDF kita tidak dapat mengklaim fitur approval history sebagai gap unik; produk SaaS gratis sudah menyediakan inti job tersebut.

### Etsy digital downloads
- Hasil pencarian “Bakery Order Log” saat diperiksa menampilkan sekitar 690+ item. Kategori berisi order form, booking/deposit tracker, capacity planner, production planner, cost calculator, pickup planner dan business spreadsheet.
- Halaman tersebut secara eksplisit menampilkan “Bakery Order Change & Pickup Kit” pada $2.49. Informasi item-level review/sales tidak berhasil dipastikan untuk listing itu; jangan mengklaim telah terbukti laku.
- Sumber: https://www.etsy.com/market/bakery_order_log
- Template home bakery custom capacity/deposit seharga sekitar $23.21 memiliki penjual dengan 0 sales dan 0 reviews saat diperiksa; listing Home Bakery Order & Capacity Planner seharga $14.90 juga memiliki penjual 0 sales/0 reviews pada pemeriksaan. Ini menunjukkan listing baru/mahal bukan bukti permintaan.
- Sumber: https://www.etsy.com/listing/4539932627/home-bakery-custom-order-capacity ; https://www.etsy.com/listing/4572686609/home-bakery-order-tracker-spreadsheet

## C. Apakah “change approval kit” masih layak?

Belum ada bukti kuat.
- Kebutuhan masalah jelas, tetapi GetTheOK menyediakan inti persetujuan dan version history dengan paket gratis; Orders App juga mengelola perubahan detail pesanan.
- Generic scope-creep/change-order tracker di Etsy menawarkan change log, approval tracker, message builder dan offline app seharga promo sekitar $12.60, tetapi toko/listing yang diperiksa menampilkan 0 sales dan no reviews.
  Sumber: https://www.etsy.com/listing/4563927402/scope-creep-change-order-approval
- Produk "Bakery Order Change & Pickup Kit" yang muncul pada hasil pencarian dihargai $2.49, sehingga ceiling harga untuk produk printable sederhana berpotensi rendah; tidak ada item sales evidence yang berhasil diverifikasi.
- Static PDF/Excel tidak otomatis mencatat jawaban pelanggan, attachment, timestamp dan revision history dengan tingkat bukti serta kenyamanan sama seperti aplikasi. Produk harus memberikan manfaat spesifik yang software gratis belum sediakan. Belum ditemukan manfaat tersebut dengan bukti WTP.
- Kesimpulan: **NO-GO untuk membangun order-change tracker generik.** Simpan masalah ini sebagai insight, bukan produk.

## D. Pembanding segmen lain

### Dog / mobile pet grooming
- Etsy category search menampilkan 626+ items untuk Dog Grooming Business Spreadsheet dan 1,000+ untuk Dog Grooming Business Tracker. Jumlah hasil tidak boleh dianggap penjualan.
- Salah satu mobile dog grooming tracker seharga $14.49 memiliki satu ulasan item 5/5; review memuji produk, harga dan layanan. Toko penjual mempunyai 556 sales total, bukan penjualan listing tersebut.
  Sumber: https://www.etsy.com/listing/4535031376/mobile-dog-grooming-business-tracker
- Listing dog grooming lain (YamadaStudio) seharga $14.90, item rating 5/5, seller shop 12 sales; fitur meliputi pet profile, coat/handling notes, grooming preferences, vaccine dates, appointment, pricing, rebooking, revenue/profit; hasil formula diuji di Google Sheets dan Excel. Item-level review count tidak terbukti dari halaman yang diambil.
  Sumber: https://www.etsy.com/listing/4586295976/dog-grooming-business-spreadsheet-excel
- Produk mobile groomer seharga $22.53 menawarkan pricing berdasarkan pet size/coat/behavior dan mileage, CRM, booking, route/travel, rebooking dan profit. Item review belum terlihat pada hasil pembukaan. Sumber: https://www.etsy.com/listing/4544431176/mobile-dog-groomer-spreadsheet-pricing
- Kesimpulan: market ada, tetapi direct competitors sudah menawarkan mobile pricing, mileage, rebooking, customer/pet histories, vaccine/handling notes dan dashboards. Bukti item-level paid demand belum cukup untuk mengungguli home bakery.

### Short-term-rental / Airbnb turnover cleaning
- Etsy memiliki berbagai digital turnover sheets: property/cleaner assignments, checklist, restock, linens, issue/maintenance, payout and unpaid-job tracking.
- Salah satu listing Short Term Rental Turnover Tracker berharga $24.99 tetapi halaman item menyatakan belum ada ulasan item; toko penjual mempunyai 32 sales dan 4 ratings secara keseluruhan (bukan angka penjualan produk tersebut). Satu listing Airbnb cleaning workbook yang lain juga belum memiliki ulasan item; jangan menyamakan total sales toko dengan listing.  
- Sumber: https://www.etsy.com/listing/4503251390/airbnb-cleaning-checklist-turnover-kit ; https://www.etsy.com/listing/4497110510/short-term-rental-cleaner-operations
- Kesimpulan: problem dan pembeli potensial ada, tetapi banyak tracker serupa; perlu bukti item review lebih kuat dan masalah spesifik yang belum ditangani sebelum memilih.

## E. Keputusan RapiUsaha setelah putaran ini

- RapiPO all-in-one generik: **NO-GO saat ini**. Jangan tambah fitur atau menjual prototipe sebagai final.
- Bakery order revision / change approval spreadsheet generik: **NO-GO saat ini**. Pain point nyata, tetapi free SaaS dan template pesaing sudah ada; WTP spesifik untuk file kita belum terbukti.
- Home bakery sebagai segmen: masih kandidat dengan paid digital-download evidence paling kuat dalam riset awal, tetapi belum dikunci.
- Dog grooming dan short-term-rental cleaning: kandidat pembanding, belum cukup bukti untuk mengganti home bakery.
- Jangan membuat video promosi yang mengiklankan produk belum dipilih/tervalidasi. Video prototipe yang sudah dibuat hanya boleh dipakai sebagai demo internal; bukan iklan peluncuran.

## F. Langkah riset berikutnya

1. Perluas pencarian ke beberapa segmen UMKM dengan paid listing reviews yang bisa diverifikasi di level item, bukan angka hasil pencarian atau penjualan toko.
2. Untuk setiap kandidat, cari masalah spesifik di review pembeli dan komunitas, lalu cek apakah solusi gratis atau software subscription sudah menyelesaikannya.
3. Hanya kandidat dengan problem intensity, item-level demand dan gap yang bertahan setelah audit software gratis boleh masuk tahap prototipe.
4. Untuk semua kandidat, cek jalur marketplace dan pembayaran yang legal serta bisa dipakai pengguna saat ini. Etsy tetap terkendala usia pemilik akun minimal 18 tanpa wali.
5. Tetap jangan mengunci produk tanpa bukti dan persetujuan pengguna.
