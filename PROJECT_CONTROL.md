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
### Checkpoint P-RENTAL — 2026-11-12 (sesi riset lanjutan)
- **Status:** riset dan pemilihan segmen berjalan AKTIF. Keputusan GO distribusi komersial tetap TERBLOKIR sampai syarat partner/payout CSHub atau vendor alternatif terkonfirmasi tertulis. Namun riset pasar, positioning, aset akuisisi, dan persiapan pelanggan tidak perlu menunggu balasan vendor.
- **Perubahan arah riset berdasarkan bukti:** jangan jadikan rental mobil segmen utama awal. Fokus validasi sekarang adalah **rental perlengkapan event kelas menengah, terutama sound system + lighting/AV untuk wedding, seminar, conference, event organizer, dan klien korporat**. Target bukan penyewaan kecil yang hanya menjual satu paket portable, melainkan operator dengan banyak komponen/unit, beberapa event berdekatan/bersamaan, tim/admin, dan inquiry/quotation lewat WhatsApp.
- **Alasan seleksi:** satu order sound/lighting dapat memuat beberapa speaker, mixer, amplifier, mic, lighting dan kabel; setiap item harus tersedia pada tanggal yang sama; beberapa event berjalan bersamaan; terdapat penawaran paket, kontrak, deposit, piutang/termin, surat jalan, pickup-return, kondisi/maintenance, dan follow-up klien. Kesesuaian produk lebih spesifik daripada rental mobil umum.
- **Sinyal nilai transaksi (harga yang dipublikasikan penyedia; bukan rata-rata industri dan bukan margin):** paket sound portable sekitar Rp850 ribu–Rp2,5 juta/hari, wedding/PA dan seminar sekitar Rp2,5–5,5 juta, paket sound+lighting sekitar Rp4,5–7 juta, line-array sekitar Rp7,5–15 juta. Sumber contoh: https://www.kreasiukasah.co.id/sewa-sound-system/ ; https://kreasihebat.id/produk/harga-penyewaan-sound-system/ ; https://sewaalatevent.id/product-category/audio-visual/sound-system/sound-system-package/ . Satu invoice event dengan nilai jutaan membuat harga SaaS Rp99k–299k/bulan secara nominal mungkin terjangkau, tetapi willingness-to-pay dan keuntungan aktual masih harus diuji.
- **Sinyal kompleksitas operasi:** halaman MyRental sound system menyebut use case multi-event, 5–10 event per minggu saat peak season, beberapa tim paralel, bundle sound+lighting+mic, tracking per unit, maintenance/condition, kontrak event, surat jalan, deposit, dan reminder H-1. Halaman tersebut menyebut skenario ini bukan testimoni nyata karena platform masih early access; gunakan hanya sebagai bukti kebutuhan yang vendor targetkan, bukan angka pasar tervalidasi. Sumber: https://myrental.id/industries/rental-sound-system
- **Perbandingan segmen:** rental mobil memiliki pasar luas, tetapi survei internal Asperda yang dikutip Bloomberg Technoz pada Maret 2026 menyebut 50% anggota melaporkan permintaan mudik stagnan dan 30% turun; rental harian B2C juga menghadapi musiman, risiko keamanan, biaya perawatan, dan kompetisi harga. Segmen kontrak korporasi cenderung lebih stabil namun kompetisinya ketat. Sumber: https://www.bloombergtechnoz.com/detail-news/103211/rental-mobil-sepi-saat-arus-mudik-permintaan-justru-turun/2
- **Alternatif yang tidak dipilih sebagai fokus pertama:** rental alat berat punya nilai kontrak/harian lebih tinggi (contoh listing excavator sekitar Rp1,4–8 juta/hari dan puluhan juta per bulan), tetapi proses B2B, kebutuhan maintenance/engine hours, operator, mobilisasi, proyek, dan kontrak lebih kompleks; software rental UMKM umum/CSHub belum terbukti menutup seluruh kebutuhan itu. Kamera/video lebih niche dan nilai sewa harian body/lensa lebih rendah pada kelas umum (sekitar Rp50k–350k; cinema kit bisa lebih tinggi); perlu validasi lanjutan sebelum dipilih. Sumber contoh: https://trisulamegah.com/blog/harga-sewa-excavator-2026/ ; https://cameraconfig.com/harga-sewa-kamera-per-hari/
- **Produk dan posisi jual sementara:**
  - **MyRental:** paling jelas cocok untuk back-office rental event: item per-unit, ketersediaan per tanggal, booking beberapa item dalam satu order, bundles, status maintenance/rusak, foto kondisi, kontrak/invoice/kwitansi/surat jalan, deposit, laporan, dan WhatsApp click-to-chat. Harga FAQ terbaru yang ditemukan: Starter Rp99k/bulan (50 item, 1 user), Growth Rp199k (unlimited item, 3 user), Scale Rp249k (5 user, booking online/keuangan). Ada konflik informasi pada halaman lama FAQ/homepage yang menampilkan angka/struktur harga berbeda; sebelum mempromosikan, periksa harga checkout aktual. Materi vendor menyebut platform masih early access; adopsi/retensi belum diverifikasi. Belum ditemukan program affiliate publik atau API publik untuk mitra; jangan anggap ada komisi. Sumber: https://myrental.id/industries/rental-sound-system ; https://myrental.id/faq
  - **Rentra:** back-office rental umum dengan booking online, inventory, bundles, DP/deposit, customer docs, quotation/invoice/receipt, multiple staff, subdomain booking, export, notifikasi Telegram; harga publik Rp99k/Rp199k/Rp299k per bulan untuk Basic/Professional/Enterprise (pembayaran tahunan diskon). Form register memiliki `Kode Referral (opsional)` dan halaman kontak membuka kebutuhan partnership, tetapi ini belum membuktikan affiliate/komisi. App listing menampilkan kontak dukungan `halo@rentra.id`. Draft email partnership dibuat 2026-11-12, Gmail draft ID `r266373474619121964`; status BELUM DIKIRIM. Pertanyaan mencakup program resmi, recurring commission, rate, atribusi/dashboard, payout, syarat umur 17/guardian, dan demo segmentasi event. Sumber: https://rentra.id/ ; https://rentra.id/register ; https://rentra.id/kontak ; https://play.google.com/store/apps/details?id=id.rentra.app
  - **CSHub:** front-office/lead conversion: WhatsApp shared inbox, AI untuk respon luar jam kerja, booking per tanggal dan jumlah unit, locking slot, chat escalation, AI lead qualification. Starter Rp300k/bulan berdasarkan pricing page. Lebih mahal dari Rentra/MyRental; layak untuk operator dengan lead WhatsApp aktif dan multi-admin, bukan bisnis kecil yang hanya perlu inventaris dan invoice. Komisi publik di halaman affiliate mensimulasikan 20% recurring Starter Rp300k, namun tier, durasi, payout dan eligibility di email resmi masih menunggu konfirmasi. Sumber: https://cshub.id/id/affiliate ; https://cshub.id/en/pricing ; https://cshub.id/id/solutions/rental-sewa
- **Arah distribusi produk:** jangan menjual satu software sebagai solusi tunggal untuk semua. Uji dua kasus: (A) rental event perlu inventaris, jadwal, bundling dan dokumen → MyRental/Rentra; (B) rental event punya chat volume tinggi, leads malam, beberapa admin, dan sering lambat merespons → CSHub. Pilih berdasarkan kebutuhan setelah diagnosis singkat, bukan mendorong produk karena komisi tertinggi.
- **Customer profile awal untuk disaring:** vendor sound/lighting/AV yang (1) menangani lebih dari satu event per tanggal/akhir pekan, (2) memiliki banyak item/jenis unit dan sering bundling, (3) memiliki admin/sales atau tim lapangan, (4) membuat quotation, invoice, kontrak, DP/termin, dan (5) menerima inquiry lewat WhatsApp/Instagram. Jangan mengunci cutoff jumlah unit/event bulanan sebelum ada wawancara/validasi.
- **Kontak partner CSHub:** email resmi sudah terkirim ke `manufacturdigitalhub@gmail.com`, ID di atas. Belum ada balasan.
- **Kontak partner Rentra:** draft belum dikirim; jangan mengirim tanpa otorisasi eksplisit. Kontak yang diverifikasi melalui Google Play: `halo@rentra.id`, nomor +6282322230446.
- **Keputusan tetap terkunci:** jangan membangun website distribusi besar-besaran, membuat klaim pendapatan, atau menjanjikan komisi sebelum kontrak/ketentuan affiliate dan eligibility jelas. Riset segmen, konten organik, lead magnets, dan validasi prospek boleh dilanjutkan sekarang.
- **Langkah berikutnya yang konkret:** susun satu konten/alat akuisisi spesifik rental audio-event yang menjelaskan biaya akibat double-booking, lead yang terlambat dibalas, dan kesalahan alokasi paket; telusuri lalu reuse Kalkulator Potensi Booking Rental yang sudah pernah dibuat (lokasi file masih belum terpetakan; pencarian kode di repo yang saat ini terhubung belum menemukannya). Setelah itu buat sampel prospek vendor event/audio rental dengan kanal inbound yang tidak mengharuskan pengguna cold-DM.
- **Batas bukti:** tidak ada sumber statistik resmi yang berhasil menetapkan jumlah operator rental sound/lighting nasional atau profitabilitas rata-rata mereka. Angka pasar yang beredar pada blog MyRental secara eksplisit estimasi, bukan data BPS; jangan gunakan angka operator/ukuran pasar blog tersebut sebagai fakta. Lakukan validasi dari direktori bisnis dan prospek nyata.
- **Yang benar-benar diselesaikan di sesi ini:** riset perbandingan segmen dan vendor berbasis halaman resmi/pricing; kesimpulan fokus awal ditetapkan sementara; draft inquiry partner Rentra disimpan di Gmail; checkpoint ini diperbarui. Tidak ada deployment website atau email Rentra yang dikirim.


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
