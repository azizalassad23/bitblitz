# BIT BLITZ — panduan singkat untuk guru

Permainan konversi bilangan **biner ↔ desimal**, 4-bit sampai 11-bit.
Tanpa server, tanpa login. Setelah halaman terbuka sekali, permainan tetap jalan
meski Wi-Fi putus.

## Berkas

| Berkas | Perlu diunggah? | Isi |
|---|---|---|
| `index.html` | **ya** | Permainan luring (satu perangkat, tanpa server) |
| `daring.html` | ya, kalau memakai mode daring | Permainan bergiliran antar-HP |
| `tailwind.css` | **ya** | Gaya tampilan, hasil bangun Tailwind |
| `.nojekyll` | ya | Agar GitHub Pages tidak memproses ulang |
| `README.md` | boleh | Panduan ini |
| `apps-script.gs` | tidak | Server mode daring — ditempel ke Google Apps Script |
| `tailwind.input.css` | tidak | Sumber gaya, hanya dipakai saat membangun ulang |

## Dua mode pemakaian

- **Luring — `index.html`.** Berjalan sendiri di satu perangkat, tanpa server dan tanpa
  internet. Cocok untuk kelas tatap muka: individu, kelompok, duel, kuis proyektor,
  dan maraton sekelas.
- **Daring — `daring.html`.** Murid bergiliran menjawab dari HP masing-masing, nilai
  masuk sendiri ke Google Spreadsheet. Butuh pemasangan sekali lewat Google Apps Script.
  Langkahnya ada di **[PANDUAN-DARING.md](PANDUAN-DARING.md)**.

`tailwind.css` adalah **berkas statis biasa** — bukan CDN. Jadi tidak ada permintaan
ke internet sama sekali, dan permainan tetap jalan di kelas tanpa jaringan.

## Menayangkan di GitHub Pages

1. Buat repositori baru di GitHub, unggah `index.html`, `tailwind.css`, dan `.nojekyll`.
2. Buka **Settings → Pages**.
3. Bagian *Source*, pilih **Deploy from a branch** → branch `main`, folder `/ (root)` → **Save**.
4. Tunggu satu-dua menit. Alamatnya menjadi `https://<nama-akun>.github.io/<nama-repo>/`.
5. Bagikan tautan itu ke murid — bisa dibuka di HP maupun laptop.

Untuk mencoba tanpa GitHub, cukup klik dua kali `index.html` di komputer Anda.

---

## Langkah 1 — pilih arah konversi

Sakelar di layar judul menentukan seluruh sesi: *Biner → Desimal* (murid mengetik angka)
atau *Desimal → Biner* (murid menyalakan lampu bit). Tipe soal tidak pernah diacak;
satu sesi tetap pada satu arah.

## Langkah 2 — pilih mode

### Mode individu

| Mode | Aturan | Kapan dipakai |
|---|---|---|
| **Latihan** | Tanpa timer, tanpa nyawa. Label nilai tempat selalu tampil. Jawaban salah dijelaskan langkahnya (`182 = 128+32+16+4+2`). | Pemanasan. Sebaiknya selalu dimainkan sebelum Solo Sprint. |
| **Solo Sprint** | 90 detik, 3 nyawa, level naik/turun mengikuti kemampuan. | Penilaian individu. |
| **Survival** | Tanpa timer, 3 nyawa, level naik terus dan tidak pernah turun. | Murid yang ingin mengejar rekor. |

### Mode kelompok

**🔗 ESTAFET BIT** — *1 HP/laptop per kelompok.*
Isi nama anggota (minimal 2, tanpa batas atas) dan pilih tingkat kesulitan. Sebelum tiap soal, layar bertanya
**siapa yang menjawab** — tim memilih sendiri, tapi **satu orang tidak boleh menjawab dua
kali berturut-turut**. Timer 120 detik terus berjalan termasuk saat memilih dan mengoper.
Tiga nyawa dipakai bersama satu tim.

Dua aturan yang membuat tim harus melibatkan semua anggotanya:
- Setiap kali **semua anggota sudah menjawab benar** satu putaran, tim mendapat **+8 detik**.
- Di akhir, skor dikalikan **faktor pemerataan** — sampai ×1,2 bila seluruh anggota
  pernah menjawab benar.

Level dikunci oleh guru dan tidak naik sendiri, supaya skor antar kelompok setara.

**🏃 MARATON KELAS** — *satu kelas, satu perangkat dioper berkeliling.*
Isi nama peserta satu per baris, **tanpa batas jumlah** — atau kosongkan saja dan cukup isi
jumlahnya, nanti dinomori otomatis (`Peserta 1`, `Peserta 2`, …) mengikuti nomor absen.

Giliran berjalan **berurutan dari nomor 1** dan berpindah sendiri — tidak ada layar
pemilihan yang perlu diketuk, supaya tidak memakan waktu. Nama yang sedang bergiliran
tampil besar di baris atas dan berkedip setiap kali berganti. Tidak ada nyawa: kelas
boleh terus mencoba sampai waktu habis.

Syarat lulus: **mencapai rentetan jawaban benar sesuai target sebelum waktu habis.**
Bawaannya 15 rentetan dalam 20 menit — keduanya bisa diubah. **Satu orang salah, rentetan
kembali ke nol.** Itulah inti permainannya: yang sudah paham jadi berkepentingan
memastikan temannya juga paham, karena satu kesalahan menghapus kerja seluruh kelas.

Di layar selalu tampak siapa yang sedang bergiliran, rentetan berjalan, dan bilah kemajuan
menuju target. Di akhir muncul hasil **LULUS / Belum lulus**, rentetan terbaik, akurasi
kelas, dan berapa peserta yang pernah menjawab benar.

**⚔ DUEL REBUTAN** — *1 perangkat, layar dibagi dua.*
Soal sama muncul di tengah, empat pilihan jawaban di masing-masing sisi.
Tim kiri memakai tombol `Q` `W` `A` `S`, tim kanan tombol **panah**; di layar sentuh
tiap tim cukup mengetuk sisinya sendiri. **Salah jawab = terkunci 3 detik**, sementara
lawan masih boleh menjawab — jadi asal tekan itu mahal. Menang duluan mencapai poin target.

**📽 KUIS KELAS** — *hanya 1 layar proyektor guru.*
Guru menekan `Spasi` untuk memunculkan soal; hitung mundur 15 detik tampil besar.
Kelompok menjawab di kertas dan mengangkatnya bersamaan saat waktu habis. Setelah jawaban
terungkap, guru menekan `1`–`8` untuk menandai kelompok yang benar (+100 poin), lalu
`Spasi` untuk lanjut.

Soal terakhir bisa dijadikan **Ronde Taruhan**: tiap kelompok mempertaruhkan sebagian
poinnya *sebelum* soal muncul. Benar = ditambah, salah = dikurangi. Kelompok yang
tertinggal jauh masih punya peluang membalik keadaan.

---

## Alat bantu guru

**Kode Sesi** (di kartu *Untuk guru*, layar judul). Isi misalnya `XIA-2026`, lalu minta
seluruh kelas mengisi kode yang sama. Semua murid akan menerima **angka soal yang sama
pada level dan urutan yang sama**, sehingga hasilnya adil dibandingkan. Kosongkan untuk
soal acak.

**Kode Hasil.** Di akhir Solo Sprint / Survival / Latihan, murid mendapat kode 8 karakter
seperti `014Q-699A`. Kode itu memuat skor, level tertinggi, akurasi, dan mode. Karena ada
karakter pemeriksa di dalamnya, kode karangan akan ditolak.

**Papan Kelas.** Tombol *PAPAN KELAS →* membuka layar rekap. Ketik nama murid dan Kode
Hasilnya, tekan **TAMBAH** — tabel tersusun otomatis dari skor tertinggi. Tombol **SALIN
TABEL** menyalinnya sebagai tabel siap tempel ke Excel atau Google Sheets.

---

## Aturan angka

- **Tangga level:** 1 = 4-bit · 2 = 5-bit · … · 5 = 8-bit (*boss: Satu Byte*) · 8 = 11-bit (*boss: Overflow*).
- **Naik level** setelah 5 jawaban benar beruntun; **turun level** setelah 2 salah beruntun
  (kecuali Survival dan Estafet). Kesulitan menyesuaikan diri, jadi satu kelas dengan
  kemampuan beragam tetap bisa bermain bersama.
- **Skor** = `lebar_bit × 10 × pengali_combo × bonus_kecepatan` (×2 saat boss round).
  Combo: 3 benar → ×1,5 · 6 → ×2 · 10 → ×3 · 15 → ×5. Satu kali salah, combo hangus.
- **Waktu** bertambah +2s/+3s/+4s tiap jawaban benar (makin tinggi level makin besar),
  dan berkurang −4s tiap jawaban salah.
- **Label nilai tempat** (1024 512 256 …) tampil penuh di level 1–4, meredup di 5–6,
  hilang di 7–8. Tombol di layar judul bisa mematikannya sejak awal untuk murid tingkat lanjut.

## Kontrol

| Tombol | Fungsi |
|---|---|
| `Enter` | Kirim jawaban |
| `1` `2` … `9` `0` `-` | Nyalakan/matikan bit ke-1 sampai ke-11 (mode Desimal → Biner) |
| `Q` `W` `A` `S` / panah | Menjawab di Duel Rebutan (tim kiri / tim kanan) |
| `Spasi` | Lanjut di Kuis Kelas |
| `1`–`8` | Menandai kelompok yang benar di Kuis Kelas |
| `♪` | Matikan efek suara (bunyi benar/salah) |
| `♫` | Matikan musik latar |
| `⛶` | **Perbesaran tampilan** — ditekan berulang untuk memutar *Normal → Besar → TV* |

### Musik latar

Selama permainan berjalan ada musik latar yang **dibangkitkan langsung oleh halaman**,
bukan berkas audio — jadi tetap satu file dan tetap jalan tanpa internet.

Ada dua suasana. Saat bermain normal, musiknya **riang** dengan progresi mayor yang
santai. Begitu waktu tersisa di bawah 20 detik, musik berganti ke suasana **panik**:
tempo naik, akornya berubah minor, bas berdetak tiap ketuk, dan warna latar layar ikut
bergeser memerah. Di Kuis Kelas, peralihan panik terjadi pada 5 detik terakhir.

Tombol `♪` dan `♫` terpisah, jadi musik bisa dimatikan tanpa menghilangkan bunyi
benar/salah — berguna kalau kelas mulai terlalu ramai.

### Menayangkan di TV atau proyektor kelas

Tekan `⛶` sampai tertulis **TV**. Seluruh tampilan membesar dan panggung soal memenuhi
tinggi layar, sehingga satu ronde muat tanpa perlu digulir — sudah diperiksa pada
1280×720 maupun 1920×1080. Pilihan ini tersimpan, jadi tidak perlu diatur ulang tiap
pertemuan.

Bilangan biner ditampilkan **menyatu tanpa pemisah** (`10110110`, bukan `1011 0110`),
sesuai cara penulisan di papan tulis dan soal ujian.

Skor, rekor, lencana, dan Papan Kelas tersimpan di perangkat masing-masing
(`localStorage`), bertahan antar pertemuan tanpa perlu akun.

---

## Kalau ingin mengubah tampilan

Gaya ditulis dengan **Tailwind CSS v4** di `tailwind.input.css`, lalu dibangun jadi
`tailwind.css`. Kalau `tailwind.input.css` atau kelas di `index.html` diubah, jalankan:

```bash
npx @tailwindcss/cli@4 -i tailwind.input.css -o tailwind.css --minify
```

Perintah itu perlu Node.js dan koneksi internet **sekali saja** saat membangun.
Hasilnya berkas statis, jadi permainannya sendiri tetap tidak butuh internet.
Kalau Anda tidak mengubah tampilan, perintah ini tidak perlu dijalankan sama sekali.

Rancangan lengkapnya ada di [KONSEP.md](KONSEP.md).
