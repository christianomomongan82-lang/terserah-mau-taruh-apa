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
- **Kelayakan affiliate CSHub (BLOKIR GO):** email pertama TERKIRIM pada 2026-11-11 14:30 UTC / 22:30 WITA ke `care@cloudchat.id`, tetapi alamat/domain itu milik layanan CloudChat/PT Cloud Chat Indonesia, bukan CSHub; email itu salah alamat. Email kedua yang benar-benar terkirim ke operator CSHub `manufacturdigitalhub@gmail.com` (Manufactur Digital Hub) pada 2026-11-12; Gmail Message ID `1a1cb91653fc1426`, thread `1a1cb8019cf21fbb`. Isinya meminta konfirmasi usia 17/persetujuan wali, rekening pencairan, rate setiap tier, durasi komisi, atribusi/refund, aturan payout, biaya dan target. Sampai pemeriksaan terakhir belum ada balasan yang terlihat. Kontak `privacy@cshub.id` hanya untuk hal privasi/penghapusan akun; jangan pakai sebagai kontak affiliate. Jangan menyatakan eligibility/komisi terkonfirmasi sebelum ada jawaban tertulis.
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
### Checkpoint P-RENTAL — 2026-11-12 (koreksi fokus pemasukan cepat)
- **Status:** fokus tunggal sekarang adalah mendapatkan pemasukan pertama secepat mungkin dari model distribusi software rental yang sudah berjalan, bukan membangun bisnis baru, pindah ke rental event, atau memperluas multi-vendor.
- **Prioritas hasil:** first paying customer dan komisi pertama. Bukan ukuran pasar nasional, brand baru, atau fitur website tambahan. Hindari pekerjaan yang tidak memperpendek jalur ke pelanggan yang membayar.
- **Segmen aktif tetap:** operator rental mobil/motor dengan beberapa unit, arus inquiry WhatsApp yang nyata, dan risiko kehilangan lead atau bentrok booking karena admin/jadwal tersebar. Kriteria adalah hipotesis penyaringan; jangan mengunci batas minimal armada atau volume chat tanpa bukti.
- **Koreksi kesalahan asisten:** riset rental sound system/lighting/AV event yang sempat muncul pada sesi 2026-11-12 adalah pivot yang tidak diminta dan BUKAN keputusan pengguna. Jangan jadikan sebagai segmen aktif atau mengganti target rental mobil/motor. Catatan penelitian event-rental boleh dipertahankan hanya sebagai eksplorasi sampingan, bukan pekerjaan berikutnya.
- **Aset yang sudah ada dan bisa segera dipakai:** RentalOps Worker `https://rentalops-site.usahalab-id.workers.dev` sudah live. HTML Worker berhasil dibaca read-only pada 2026-11-12. Judul: `RentalOps — Booking Rental Lebih Rapi`; H1: `Jangan sampai unit yang sama dijanjikan ke dua penyewa.`; kalkulator sudah tertanam. Input kalkulator: permintaan booking per bulan, persentase chat terlambat/tidak tertangani, peluang konversi jika terlayani, laba kontribusi per booking, dan biaya sistem. Rumus potensi kebocoran = permintaan × tingkat chat terlambat × peluang konversi × laba kontribusi per booking. Halaman secara benar menyatakan software tidak otomatis memulihkan semua lead yang hilang.
- **Status CTA:** kedua CTA produk saat ini mengarah ke halaman publik biasa `https://cshub.id/id/solutions/rental-sewa`, belum merupakan tautan referral. Tidak ada perubahan/deployment website pada audit terakhir. Jangan mengklaim atau mengirim CTA referral sebelum ada link resmi affiliate yang valid.
- **Ekonomi referral CSHub:** halaman affiliate mencontohkan 20% recurring dari paket Starter Rp300.000/bulan = Rp60.000 per pelanggan aktif per bulan. Maka 5 pelanggan Starter aktif ≈ Rp300.000/bulan; 17 ≈ Rp1.020.000/bulan; 50 ≈ Rp3.000.000/bulan pada rate 20% yang sama. Ini komisi berulang, bukan uang sekali bayar; belum tentu cukup cepat untuk kebutuhan tunai segera. Jangan menjanjikan hasil.
- **Syarat payout yang masih kontradiktif:** landing affiliate menyatakan tanpa minimum dan 1×24 jam kerja, sedangkan panduan affiliate yang lebih rinci menunjukkan minimum Rp20.000 dan waktu 1–3 hari kerja. Anggap panduan operasional Rp20.000 dan 1–3 hari kerja sebagai asumsi konservatif, tetapi perlu konfirmasi tertulis.
- **Eligibility usia:** pengguna berusia 17 tahun; belum ada konfirmasi resmi apakah boleh bergabung dengan persetujuan wali, dan apakah rekening wali dapat dipakai. Jangan membuat akun dengan usia palsu atau menjanjikan pencairan sebelum jelas.
- **Email CSHub:** email resmi TERKIRIM ke operator CSHub `manufacturdigitalhub@gmail.com` pada 2026-11-12. Gmail message ID `1a1cb91653fc1426`, thread `1a1cb8019cf21fbb`. Pertanyaan meliputi usia/wali, nama rekening, tier/rate, durasi recurring, attribution/refund, payout, biaya dan target. Pada pemeriksaan terakhir belum ada balasan. Email awal ke `care@cloudchat.id` salah alamat; jangan anggap itu kontak CSHub.
- **Vendor lain:** MyRental dan Rentra adalah pembanding yang pernah diteliti, bukan fokus saat ini. Draft pertanyaan partnership Rentra Gmail ID `r266373474619121964` belum dikirim. Jangan menghabiskan waktu memperluas daftar vendor sebelum uji akuisisi pertama atau bukti bahwa CSHub tidak bisa dipakai.
- **Langkah cepat yang aktif:** (1) pakai kalkulator RentalOps yang sudah live sebagai konten/alat gratis untuk menarik rental mobil/motor; (2) sebarkan satu konten edukatif dengan tautan kalkulator ke komunitas rental yang relevan, bukan pindah segmen; (3) ukur respons dan cari satu operator dengan armada/chat cukup aktif untuk uji CSHub; (4) setelah link affiliate dan usia/payout terkonfirmasi, sambungkan CTA. Prioritaskan kanal publik/inbound agar tidak bergantung pada cold-DM; siapkan pesan kontak singkat hanya bila diperlukan.
- **Hasil yang benar-benar diselesaikan dalam sesi koreksi:** fokus pasar dikembalikan ke rental mobil/motor; live website dan kalkulator diperiksa; kesalahan pivot event-rental dicatat sebagai bukan keputusan; email CSHub benar-benar terkirim; tidak ada email Rentra dikirim dan tidak ada deployment website dilakukan.


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
