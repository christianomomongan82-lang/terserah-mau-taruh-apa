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
  - **MyRental:** telah dibandingkan dengan CSHub. Kesimpulan: jangan tawarkan CSHub kepada rental yang hanya perlu kalender kendaraan dan invoice; target CSHub harus benar-benar membutuhkan pengelolaan percakapan dan follow-up pelanggan.
- **Aset akuisisi yang sudah dibuat menurut percakapan sumber (2026-11-11):** Kalkulator Potensi Booking Rental, file HTML mandiri untuk menghitung potensi keuntungan yang hilang akibat calon pelanggan tidak terlayani atau booking tidak ditindaklanjuti. File aktual/lokasinya belum dipetakan ke repo atau Worker.
- **Kelayakan affiliate CSHub (BLOKIR GO):** email konfirmasi telah dibuat sebagai draft, BELUM DIKIRIM. Pertanyaan untuk vendor: (1) apakah usia 17 tahun diperbolehkan, (2) apakah persetujuan wali dapat digunakan, (3) apakah rekening pencairan harus atas nama affiliate, (4) apakah komisi 20% benar-benar recurring, (5) berapa lama komisi dibayarkan. Jangan mengklaim program bisa diikuti atau komisi dapat dicairkan sebelum ada jawaban resmi. Jangan memalsukan usia atau identitas.
- **Keputusan bisnis terakhir yang terkunci:** BELUM GO untuk membangun website distribusi besar-besaran sebelum syarat komisi dan kelayakan peserta jelas. CSHub adalah pilihan sementara, bukan keputusan final. Langkah bernilai berikutnya: selesaikan kelayakan partner dan pilih segmen rental paling menguntungkan berdasarkan bukti.
- **Pemantauan partner software:** percakapan sumber menyatakan monitor otomatis sudah aktif setiap Rabu pukul 09.00 WITA, maksimal tiga kandidat terbaik per laporan, mengevaluasi komisi, usia, pencairan, kompetisi, dan potensi laba; tidak berhenti hanya karena satu minggu tidak ada kandidat baru. Pemeriksaan pertama dijadwalkan 18 November 2026. Status automation aktual belum diperiksa; jangan klaim aktif berdasarkan ringkasan saja.
- **Nama RentalOps / website:** https://rentalops-site.usahalab-id.workers.dev dan Worker rentalops-site pernah dibuat/dideploy, tetapi itu aset landing page yang terkait dengan jalur akuisisi, bukan definisi keseluruhan bisnis. Keberadaan Worker tidak membuktikan CTA, link referral, atau kelayakan affiliate aktif.
- **Bukti Cloudflare diperiksa 2026-11-12:** Worker rentalops-site ada pada akun Cloudflare yang terhubung; created_on=2026-11-11T14:38:12Z, modified_on=2026-11-11T15:21:16Z, last_deployed_from=api. Ini membuktikan resource Worker ada, bukan bahwa halaman/CTA/referral saat ini benar atau berfungsi.
- **Pemetaan sumber kode:** daftar repo GitHub dari akun yang terhubung tidak menampilkan repo bernama RentalOps. Repo sumber/deployment pipeline belum dipetakan; jangan menebak repo dari hasil pencarian publik.
- **Batas keras:** jangan mengubah proyek ini menjadi SignageQuote atau SiapKirim. Jangan menyatakan produk/komisi/referral sudah aktif tanpa verifikasi. Jangan menggunakan nama pribadi pengguna pada nama brand, domain, website, atau salam.
- **Sumber pemulihan wajib:** percakapan share tanggal 2026-11-11 yang diberikan pengguna: https://chatgpt.com/share/6af49862-6ef4-83ec-b3ae-eb207ee75a9a?ogimg=plain. Isi pentingnya diringkas di bagian ini agar arah bisnis tetap pulih lintas chat.
- **Status verifikasi 2026-11-12:** arah dan keputusan bisnis di atas dipulihkan dari percakapan share; kelayakan CSHub, email terkirim/tidak, file kalkulator, status automation, isi website live, CTA, dan referral tetap perlu diverifikasi dari sumber masing-masing sebelum tindakan.
### P-SIGNAGE — SignageQuote
- **Status:** PROYEK TERPISAH; bukan proyek rental.
- **Tujuan:** kalkulator/estimator penawaran untuk bengkel signage/reklame.
- **Website yang tercatat:** https://signagequote.usahalab-id.workers.dev
- **Bukti Cloudflare diperiksa 2026-11-12:** Worker terpisah bernama `signagequote` ada; `created_on` 2026-11-07T01:39:04Z, `modified_on` 2026-11-11T16:00:30Z, `last_deployed_from=api`. Ini hanya identifikasi resource, bukan uji fungsional website.
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
