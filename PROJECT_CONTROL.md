# Project Control — Sumber Kebenaran Lintas Percakapan

**Terakhir diperbarui:** 2026-11-12  
**Fungsi:** indeks dan checkpoint lintas proyek. Dokumen ini mencegah proyek, keputusan, dan status dari percakapan berbeda tercampur.

> Aturan utama: dokumen ini membantu menemukan sumber yang benar; ia tidak menggantikan bukti langsung dari repo, deployment, file, atau keputusan eksplisit terbaru pengguna. Status yang belum dicek ditandai apa adanya.

## 1. Proyek yang terpisah

### P-RENTAL — Distribusi software B2B untuk bisnis rental
- **Status fokus percakapan:** AKTIF sebagai arah bisnis yang sedang dikerjakan; keputusan komersial final masih BELUM GO.
- **Identitas proyek:** bisnis distribusi software B2B berbasis komisi/partner, bukan perusahaan jasa rental dan bukan sekadar website RentalOps.
- **Arah jangka panjang:** membangun perusahaan distribusi software B2B dengan beberapa sumber komisi/vendor. Aset inti: saluran akuisisi pelanggan, konten informatif, alat bantu interaktif, dan sistem referral.
- **Segmen yang sedang dievaluasi:** pemilik bisnis rental. Jangan anggap seluruh pemilik rental sebagai target yang cocok untuk setiap produk; segmentasi harus berdasarkan kebutuhan operasional dan percakapan pelanggan.
- **Kandidat produk/vendor:
  - **CSHub:** pilihan sementara saat pembahasan terakhir karena potensi komisi recurring. Harga acuan Rp300.000/bulan; komisi yang dipublikasikan 20% (ilustrasi Rp60.000/pelanggan aktif/bulan). Halaman afiliasi punya informasi bertentangan tentang tingkat/durasi komisi. Angka ini belum terverifikasi dan bukan pendapatan terjamin.
  - **Wicom / SakuKapling:** kandidat pembanding; software developer kavling, harga acuan Rp1.200.000/tahun dan komisi terpublikasi 30% (ilustrasi Rp360.000/penjualan). Syarat usia juga belum terverifikasi.
  - **MyRental dan kecocokan produk:** kesimpulan lama bahwa CSHub hanya relevan untuk pengelolaan chat perlu diperbarui. Dokumentasi resmi cshub kini secara eksplisit menjelaskan produk sewa harian per tipe kendaraan, jumlah unit, ketersediaan berdasarkan rentang tanggal, pencegahan bentrok sebelum booking disimpan, hitung tarif harian, checkout/tagihan, inbox multi-admin, AI untuk chat di luar jam kerja, dan tiket kerusakan/denda. Ini berarti CSHub juga punya fungsi operasional rental, bukan sekadar CRM chat. Belum ada uji produk langsung; klaim ini bersumber dari halaman solusi dan dokumentasi vendor. Kandidat segmen awal yang paling masuk akal untuk diuji: rental mobil/motor multi-unit dengan chat masuk rutin, potensi bentrok booking, admin lebih dari satu, atau permintaan malam hari. Ini hipotesis kecocokan produk, bukan bukti bahwa segmen tersebut paling besar atau paling menguntungkan. Jangan menetapkan cutoff jumlah armada tanpa riset pelanggan.
- **Aset akuisisi yang sudah dibuat menurut percakapan sumber (2026-11-11):** Kalkulator Potensi Booking Rental, file HTML mandiri untuk menghitung potensi keuntungan yang hilang akibat calon pelanggan tidak terlayani atau booking tidak ditindaklanjuti. File aktual/lokasinya belum dipetakan ke repo atau Worker.
- **Kelayakan affiliate CSHub (BLOKIR GO):** email yang tercatat di Gmail ternyata TERKIRIM pada 2026-11-11 14:30 UTC / 22:30 WITA ke `care@cloudchat.id`, tetapi alamat/domain itu milik layanan CloudChat/PT Cloud Chat Indonesia, bukan CSHub. Email tersebut TIDAK dihitung sebagai kontak ke vendor CSHub dan tidak menyelesaikan pertanyaan eligibility. Tidak ada balasan CSHub yang tercatat. Kontak umum yang dipublikasikan CSHub di kebijakan privasinya adalah `manufacturdigitalhub@gmail.com` (operator: Manufactur Digital Hub); `privacy@cshub.id` dicantumkan untuk permintaan privasi/penghapusan akun. Pada 2026-11-12 dibuat draft email baru ke `manufacturdigitalhub@gmail.com`, ID Gmail draft `r6438034906410159650`; status DRAFT, BELUM DIKIRIM. Draft menanyakan batas usia 17 tahun/persetujuan wali, nama rekening pencairan, rate per tier, durasi komisi, atribusi/refund, serta konflik ketentuan penarikan. Jangan menyatakan syarat sudah dikonfirmasi sebelum ada jawaban tertulis.
- **Ketentuan affiliate yang dipublikasikan CSHub (diverifikasi 2026-11-12):** halaman affiliate mensimulasikan 20% recurring untuk Starter Rp300.000/bulan dan menyebut persentase naik mengikuti tier, tetapi angka rate setiap tier tidak dipublikasikan. Durasi komisi kontradiktif di halaman yang sama: satu bagian menyebut sampai bulan ke-24, FAQ menyebut tanpa batas selama pelanggan aktif. Pencairan juga kontradiktif: FAQ promosi menyebut tanpa minimum dan 1×24 jam kerja, sedangkan dokumentasi operasional menyebut minimum Rp20.000 dan diproses 1–3 hari kerja. Perlu jawaban resmi tertulis; gunakan ketentuan dokumentasi operasional sebagai detail yang lebih konkret, tetapi tetap jangan anggap final.
- **Usia/identitas:** Syarat & Ketentuan yang dibuka (terakhir diperbarui 18 Oktober 2026) tidak menampilkan batas usia spesifik. Panduan pendaftaran menjelaskan nama, nomor WA, email, dan persetujuan T&C/privasi, tetapi tidak menyatakan umur minimum. Ketiadaan klausul di halaman yang dibaca bukan bukti bahwa peserta 17 tahun diperbolehkan; tetap perlu konfirmasi langsung.
- **Keputusan bisnis terakhir yang terkunci:** BELUM GO untuk membangun website distribusi besar-besaran sebelum syarat komisi dan kelayakan peserta jelas. CSHub adalah pilihan sementara, bukan keputusan final. Langkah bernilai berikutnya: selesaikan kelayakan partner dan pilih segmen rental paling menguntungkan berdasarkan bukti.
- **Pemantauan partner software — TERVERIFIKASI AKTIF:** automation "Pantau program partner software B2B" ditemukan di sistem Automations pada 2026-11-12. ID `6af47a587e788191ab32ee88d34d47fe`, `is_enabled=true`, timezone `Asia/Makassar`, jadwal Rabu mingguan pukul 09.00, `next_run_time=2026-11-18T09:00:00+08:00`. Prompt tersimpan mencakup batas usia 17 tahun, pemeriksaan ketentuan resmi, CSHub dan Wicom/SakuKapling sebagai benchmark, maksimal tiga kandidat, serta laporan mingguan meskipun tidak ada kandidat baru. Ini automation yang benar-benar ditemukan, bukan sekadar klaim percakapan.
- **Nama RentalOps / website:** https://rentalops-site.usahalab-id.workers.dev. Ini aset landing page untuk jalur akuisisi, bukan definisi keseluruhan bisnis distribusi B2B.
- **Alasan penggantian alamat (dipulihkan dari percakapan share 2026-11-11):** pengguna meminta alamat website yang sebelumnya memuat nama akun pribadi diganti agar tidak menampilkan nama pribadi. Kesalahan yang terjadi: asisten sempat memberikan alamat SignageQuote ketika yang diminta pengguna adalah RentalOps. Jangan ulangi; cocokkan nama proyek, URL, dan isi halaman sebelum membagikan link.
- **Verifikasi live 2026-11-12:** `https://rentalops-site.usahalab-id.workers.dev` berhasil dibuka dan judul/isi yang tampil adalah **RentalOps — Booking Rental Lebih Rapi**. `https://signagequote.usahalab-id.workers.dev` berhasil dibuka dan judul/isi yang tampil adalah **SignageQuote — Estimator Bengkel Signage**. Kedua URL saat ini menuju proyek yang benar dan tidak tertukar.
- **Batas verifikasi live:** pembukaan halaman dan kecocokan judul/isi sudah dicek; ini belum membuktikan CTA, tautan referral, komisi, ataupun proses konversi berfungsi.
- **Bukti Cloudflare diperiksa 2026-11-12:** Worker rentalops-site ada pada akun Cloudflare yang terhubung; created_on=2026-11-11T14:38:12Z, modified_on=2026-11-11T15:21:16Z, last_deployed_from=api. Ini membuktikan resource Worker ada, bukan bahwa halaman/CTA/referral saat ini benar atau berfungsi.
- **Pemetaan sumber kode:** daftar repo GitHub dari akun yang terhubung tidak menampilkan repo bernama RentalOps. Repo sumber/deployment pipeline belum dipetakan; jangan menebak repo dari hasil pencarian publik.
- **Batas keras:** jangan mengubah proyek ini menjadi SignageQuote atau SiapKirim. Jangan menyatakan produk/komisi/referral sudah aktif tanpa verifikasi. Jangan menggunakan nama pribadi pengguna pada nama brand, domain, website, atau salam.
- **Sumber pemulihan wajib:** percakapan share tanggal 2026-11-11 yang diberikan pengguna: https://chatgpt.com/share/6af49862-6ef4-83ec-b3ae-eb207ee75a9a?ogimg=plain. Isi pentingnya diringkas di bagian ini agar arah bisnis tetap pulih lintas chat.
- **Status verifikasi 2026-11-12:** arah dan keputusan bisnis di atas dipulihkan dari percakapan share; kelayakan CSHub, email terkirim/tidak, file kalkulator, status automation, isi website live, CTA, dan referral tetap perlu diverifikasi dari sumber masing-masing sebelum tindakan.
### Checkpoint P-RENTAL — 2026-11-12 00:42 WITA
- **Status:** fokus riset tetap AKTIF; keputusan GO untuk distribusi CSHub masih TERBLOKIR oleh eligibility affiliate dan ketentuan komisi yang saling bertentangan.
- **Yang benar-benar diverifikasi:** email Gmail lama hanya terkirim ke CloudChat, bukan CSHub; belum ada email CSHub yang benar-benar terkirim dan belum ada balasan CSHub. Draft koreksi ke alamat operator CSHub yang dipublikasikan sudah dibuat, tetapi belum dikirim.
- **Bukti publik CSHub:** https://cshub.id/id/affiliate ; https://documentation.cshub.id/account/affiliate ; https://cshub.id/id/terms ; https://cshub.id/id/privacy ; https://cshub.id/id/solutions/rental-sewa ; https://documentation.cshub.id/getting-started/what-is-cshub ; https://cshub.id/en/pricing
- **Temuan produk:** halaman solusi dan dokumentasi CSHub menunjukkan rental booking per hari berbasis jumlah unit dan rentang tanggal, pemeriksaan bentrok, hitung tarif harian, invoice/checkout, multi-admin, AI chat, serta tiket kerusakan/denda. Starter Rp300.000/bulan menampilkan limit 20 produk, 1.000 transaksi, 30 appointment/booking, 2 human agents, dan 1 WhatsApp number. Klaim vendor belum diuji langsung.
- **Kecocokan segmen (hipotesis):** prioritaskan pengujian pada rental kendaraan multi-unit yang punya arus chat tinggi, beberapa admin, permintaan malam, dan risiko double-booking; jangan menargetkan semua rental secara rata. Belum terbukti segmen ini paling menguntungkan; perlu validasi kebutuhan dan willingness-to-pay.
- **Pembanding kompetitif MyRental (sumber resmi, 2026-11-12):** MyRental memasarkan software khusus rental dengan inventory dan availability per tanggal, cegah double-booking, kalender pickup/return, PDF invoice/kuitansi/kontrak/surat jalan/packing slip, database penyewa, dan WhatsApp click-to-chat (bukan pesan otomatis). Harga yang dipublikasikan: Starter Rp99.000/bulan (maks. 50 item, 1 user), Growth Rp199.000/bulan (item tak terbatas, 3 user), Scale Rp249.000/bulan (5 user, booking online dan laporan keuangan). Trial Growth 14 hari. Sumber: https://myrental.id/industries/rental-mobil ; https://myrental.id/faq
- **Pembanding/lead partner Rentra (sumber resmi, 2026-11-12):** situs Rentra menyediakan paket rental multi-kategori dengan booking online, kalender, inventaris, dokumen, multi-staff, DP/deposit, tarif per durasi, dan notifikasi WA/Telegram. Harga bulanan yang dipublikasikan: Basic Rp99.000 (100 produk, 3 staff), Professional Rp199.000 (500 produk, 5 staff), Enterprise Rp299.000 (produk tak terbatas, 10 staff). Form registrasi memiliki kolom `Kode Referral (opsional)`; halaman kontak secara eksplisit mengundang kebutuhan partnership serta mempublikasikan WhatsApp admin 6282322230446. Ini mengindikasikan mekanisme referral/partnership mungkin tersedia, tetapi belum membuktikan adanya komisi, siapa yang memenuhi syarat, atau aturan payout. Situs tidak mempublikasikan rincian affiliate yang ditemukan; T&C yang dibuka hanya aturan layanan tenant dan secara eksplisit menyebut dokumen template default. Jangan mengasumsikan angka komisi/referral. Sumber: https://rentra.id/ ; https://rentra.id/register ; https://rentra.id/kontak ; https://rentra.id/terms
- **Status MyRental partner:** pencarian halaman FAQ dan materi resmi MyRental tidak menemukan program affiliate/partner publik; FAQ justru menyatakan belum ada API publik untuk mitra. Ini tidak membuktikan tidak ada kerja sama private, tetapi belum layak dihitung sebagai saluran komisi sebelum ditanya/ditawarkan secara resmi. Sumber: https://myrental.id/faq

- **Implikasi positioning sementara:** MyRental lebih murah dan lebih dalam pada administrasi rental/dokumen; CSHub lebih mahal tetapi diferensiasinya adalah AI untuk menjawab/menindaklanjuti chat, shared omnichannel inbox, serta booking dari chat. Jadi jangan jual CSHub sebagai pengganti software rental untuk semua operator kecil. Uji CSHub hanya pada rental yang masalah mahalnya adalah lead/chat hilang, kebutuhan respon di luar jam kerja, atau pembagian chat antar-admin. Klaim diferensiasi ini bersumber dari materi produk kedua vendor dan belum diuji dengan pelanggan.
- **Keputusan yang tetap terkunci:** jangan membangun situs distribusi besar-besaran atau menjanjikan komisi sebelum eligibility dan ketentuan tertulis jelas. Jangan mengirim email dari akun pengguna tanpa otorisasi eksplisit.
- **Langkah berikutnya:** minta persetujuan pengguna untuk mengirim draft ke kontak resmi CSHub; sambil menunggu jawaban, lanjutkan validasi subsegmen rental dan bandingkan kebutuhan operasional dengan produk lain.

### P-SIGNAGE — SignageQuote
- **Status:** PROYEK TERPISAH; bukan proyek rental.
- **Tujuan:** kalkulator/estimator penawaran untuk bengkel signage/reklame.
- **Website yang tercatat:** https://signagequote.usahalab-id.workers.dev
- **Bukti Cloudflare diperiksa 2026-11-12:** Worker terpisah bernama `signagequote` ada; `created_on` 2026-11-07T01:39:04Z, `modified_on` 2026-11-11T16:00:30Z, `last_deployed_from=api`.
- **Verifikasi live 2026-11-12:** `https://signagequote.usahalab-id.workers.dev` berhasil dibuka; judul/isi yang tampil adalah **SignageQuote — Estimator Bengkel Signage**. URL ini bukan alamat RentalOps.
- **Artefak terkait yang pernah ada:** SignageQuote Offline Kit.
- **Batas keras:** jangan gunakan kode, URL, keputusan, atau status SignageQuote untuk menyimpulkan keadaan RentalOps.

### P-SIAPKIRIM — SiapKirim
- **Status:** PROYEK TERPISAH; bukan proyek rental dan bukan SignageQuote.
- **Tujuan:** layanan digital penyusunan dokumen lamaran kerja/CV untuk pelamar pemula.
- **Dokumen strategi yang tercatat:** SiapKirim_Strategi_Nasional_v0.2.md (Library, 2026-11-09).
- **Batas keras:** kemunculan kata “bisnis” atau hasil pencarian Library tidak menjadikan SiapKirim proyek aktif. Hanya lanjut jika pengguna menyebutnya secara eksplisit atau konteks percakapan membuktikannya.

### P-BACKTEST — Riset trading/backtest
- **Status:** PROYEK TERPISAH.
- **Repo utama yang tercatat:** https://github.com/christianomomongan82-lang/scanner
- **Fokus:** riset kuantitatif dan validasi strategi; preregistrasi dan aturan eksperimen harus dipertahankan.
- **Batas keras:** jangan mencampur data, metrik, keputusan, atau cabang repo backtest dengan bisnis rental.

### P-ASPRUSSIA — Rancangan negara fiksi AS-Prusia
- **Status:** PROYEK TERPISAH.
- **Sumber utama:** README.md pada repo ini.
- **Batas keras:** README.md adalah arsip proyek rancangan negara fiksi, bukan indeks bisnis dan bukan status RentalOps.

## 1A. Pemetaan repositori dan deployment (diperiksa 2026-11-12)

- Daftar repositori yang terlihat melalui koneksi GitHub akun pemilik saat pemeriksaan: `dinasti-medieval`, `dinasti-medieval-build`, `dinasti-medieval-work`, `tetod`, `scanner`, dan `terserah-mau-taruh-apa`.
- Tidak ada repo RentalOps atau SignageQuote pada daftar tersebut. Ini **tidak** membuktikan source code tidak ada di tempat lain; artinya repo sumber belum berhasil dipetakan.
- Daftar Worker Cloudflare menunjukkan dua script berbeda: `rentalops-site` dan `signagequote`. Karena keduanya dideploy melalui API menurut metadata daftar Worker, jangan otomatis menganggap keduanya tersambung ke repo GitHub tertentu.
- Sebelum perubahan kode berikutnya, sumber deploy yang benar harus ditetapkan: repo sumber jika ada, atau Worker Cloudflare yang sedang berjalan jika memang dikelola langsung. Catat hubungan source → build/deploy → Worker untuk masing-masing proyek.


## 2. Urutan sumber kebenaran

Jika sumber bertentangan, gunakan urutan ini:
1. Instruksi eksplisit terbaru pengguna dalam percakapan yang sedang berlangsung.
2. Keputusan/checkpoint terbaru yang tercatat pada sumber utama proyek yang tepat.
3. Bukti langsung dari keadaan aktual: repo, branch, commit, file, deployment, halaman, dashboard, atau hasil tool.
4. Catatan percakapan lama yang secara eksplisit merujuk proyek yang sama.
5. Memory/ringkasan percakapan.
6. Hasil pencarian Library atau pencarian semantik.

**Library search, ingatan, dan kemiripan kata tidak pernah cukup untuk mengubah proyek aktif.** Hasilnya hanya petunjuk untuk menemukan sumber; bukan bukti bahwa file tersebut relevan dengan percakapan sekarang.

## 3. Protokol sebelum melanjutkan pekerjaan

Sebelum tindakan atau rekomendasi yang mengubah arah proyek:
1. Identifikasi ID proyek dari pesan pengguna saat ini dan konteks percakapan langsung.
2. Cocokkan dengan satu entri proyek di dokumen ini.
3. Buka sumber utama proyek yang tepat; verifikasi nama, tujuan, keputusan terkunci, repo/branch, URL, dan artefak terakhir.
4. Pisahkan fakta menjadi **terverifikasi saat ini**, **catatan terakhir tetapi belum dicek**, **asumsi**, dan **belum diketahui**.
5. Periksa apakah langkah yang hendak dilakukan bertentangan dengan keputusan terkunci atau berasal dari proyek lain.
6. Jika ada konflik nyata, hentikan tindakan yang berisiko dan jelaskan konflik spesifiknya. Jangan menebak pengganti.

Untuk permintaan “lanjutkan”, “bisnis tadi”, “proyek itu”, atau rujukan serupa, konteks percakapan langsung menjadi penentu. Jangan memilih proyek berdasarkan dokumen yang paling mudah ditemukan.

## 4. Format checkpoint wajib

Setelah keputusan penting, perubahan teknis, eksperimen, atau milestone, perbarui entri proyek yang tepat dengan:
- **Tanggal:**
- **Status:** aktif / menunggu / selesai / dibatalkan / terblokir.
- **Tujuan saat ini:**
- **Keputusan terkunci:**
- **Yang benar-benar berubah/selesai:**
- **Bukti verifikasi:** URL, repo, branch, commit, file, hasil uji, atau screenshot yang relevan.
- **Yang belum diketahui/belum diverifikasi:**
- **Langkah berikutnya yang disepakati:**
- **Larangan/perangkap:** hal yang tidak boleh diulang atau dicampur.

Jangan memperbarui status menjadi selesai hanya karena tindakan dicoba. Catat hasil aktual. Jika belum ada bukti, tulis BELUM DIVERIFIKASI.

## 5. Protokol saat konteks terputus atau chat baru

1. Baca `CHATGPT_WORKING_RULES.md`.
2. Baca dokumen ini dan identifikasi ID proyek dari konteks terbaru.
3. Buka sumber utama proyek tersebut; jangan memulai dengan pencarian Library global.
4. Ambil checkpoint terakhir dan cek bukti aktual yang dapat diperiksa.
5. Nyatakan hanya status yang benar-benar didukung; lanjutkan dari titik itu.
6. Setelah bekerja, tulis checkpoint baru sebelum mengakhiri sesi.

Jika percakapan sumber tidak tersedia atau link/file tidak dapat dibuka, jangan berpura-pura telah membaca. Gunakan checkpoint yang ada dengan label ketidakpastian, lalu minta hanya satu detail yang benar-benar tidak dapat dipulihkan.

## 6. Pemeriksaan kontradiksi sebelum mengirim jawaban

Sebelum jawaban akhir tentang proyek, periksa:
- Apakah nama/ID proyek konsisten dari awal sampai akhir?
- Apakah produk, URL, repo, branch, dan file memang milik proyek itu?
- Apakah rekomendasi bertentangan dengan keputusan yang sudah dikunci?
- Apakah status yang disebut benar-benar diverifikasi, atau hanya catatan lama?
- Apakah ada klaim keberhasilan yang tidak didukung hasil nyata?
- Apakah proyek lain masuk karena hasil pencarian, ingatan, atau kemiripan istilah?

Jika salah satu gagal, koreksi dulu sebelum menjawab.

## 7. Batas kemampuan yang harus dinyatakan jujur

Ringkasan/memori lintas percakapan dapat tidak lengkap atau keliru, dan tidak menjamin seluruh chat lama otomatis terbaca. Dokumen eksternal ini adalah mekanisme pemulihan, bukan jaminan teknis bahwa model tidak akan pernah salah. Karena itu, sumber utama harus diperiksa ulang sebelum keputusan atau perubahan penting; jangan mengklaim kontinuitas yang belum diverifikasi.

---
