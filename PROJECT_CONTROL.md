# Project Control — Sumber Kebenaran Lintas Percakapan

**Terakhir diperbarui:** 2026-11-12  
**Fungsi:** indeks dan checkpoint lintas proyek. Dokumen ini mencegah proyek, keputusan, dan status dari percakapan berbeda tercampur.

> Aturan utama: dokumen ini membantu menemukan sumber yang benar; ia tidak menggantikan bukti langsung dari repo, deployment, file, atau keputusan eksplisit terbaru pengguna. Status yang belum dicek ditandai apa adanya.

## 1. Proyek yang terpisah

### P-RENTAL — Bisnis rental / RentalOps
- **Status fokus percakapan:** AKTIF.
- **Tujuan yang tercatat:** distribusi software B2B/afiliasi untuk pemilik rental mobil dan motor; akuisisi calon pelanggan melalui kanal inbound.
- **Nama kerja yang tercatat:** RentalOps.
- **Website yang tercatat:** https://rentalops-site.usahalab-id.workers.dev
- **Produk yang pernah dipertimbangkan:** CSHub.
- **Checkpoint terakhir yang tersedia:** website pernah dinyatakan live; CTA pernah mengarah ke halaman resmi CSHub, bukan link referral pribadi. Kelayakan afiliasi dan link referral belum boleh dianggap aktif tanpa bukti terbaru.
- **Status verifikasi 2026-11-12:** BELUM DIREKONSILIASI dengan repo/deployment dan percakapan sumber paling akhir. URL, CTA, eligibility, dan keputusan produk harus dicek langsung sebelum tindakan.
- **Batas keras:** jangan mengubahnya menjadi SignageQuote atau SiapKirim. Jangan menyatakan produk/komisi/referral sudah aktif tanpa verifikasi. Jangan menggunakan nama pribadi pengguna pada nama brand, domain, website, atau salam.
- **Sumber lanjutan:** percakapan dan repo/deployment RentalOps yang terkait; periksa sumber langsung sebelum mengklaim status teknis.

### P-SIGNAGE — SignageQuote
- **Status:** PROYEK TERPISAH; bukan proyek rental.
- **Tujuan:** kalkulator/estimator penawaran untuk bengkel signage/reklame.
- **Website yang tercatat:** https://signagequote.usahalab-id.workers.dev
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
