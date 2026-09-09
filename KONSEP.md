# BIT BLITZ — Konsep Permainan Konversi Biner ↔ Desimal

> Nama alternatif: *Biner Rush*, *Sandi Bit*, *Overflow!*
> Sasaran: murid SMA umum · Rentang: 4-bit sampai 11-bit · Deploy: GitHub Pages (statis)

---

## 1. Batasan Teknis yang Membentuk Desain

GitHub Pages hanya menyajikan file statis — tidak ada server, tidak ada database. Tiga
konsekuensi yang saya jadikan dasar seluruh rancangan:

| Batasan | Solusi desain |
|---|---|
| Tidak ada backend | Seluruh permainan di `index.html` + `tailwind.css`. Gaya ditulis dengan Tailwind v4 lalu **dibangun jadi berkas statis** — bukan CDN, sehingga tidak ada satu pun permintaan ke internet saat dimainkan. |
| Tidak ada sinkronisasi antar-perangkat | **Kode Hasil** — di akhir permainan murid mendapat kode 8 karakter ber-checksum yang disetor ke guru. Guru memasukkannya ke layar *Papan Kelas* untuk merekap. Tidak bisa dikarang karena checksum. |
| Perangkat kelas belum pasti | Satu halaman, **tiga mode kelompok** yang masing-masing cocok untuk satu skenario perangkat (lihat §5). Guru tinggal pilih di layar judul. |
| Keadilan penilaian | **Kode Sesi** (mis. `XIA-2026`) menjadi *seed* pembangkit soal. Semua murid yang memakai kode sama menerima urutan soal yang persis sama. |

Bonus: karena satu file, setelah halaman dibuka sekali ia tetap jalan meski Wi-Fi sekolah putus.

---

## 2. Tangga Kesulitan (4 → 11 bit)

| Level | Lebar | Rentang desimal | Julukan |
|---|---|---|---|
| 1 | 4-bit | 0–15 | Nibble |
| 2 | 5-bit | 0–31 | — |
| 3 | 6-bit | 0–63 | — |
| 4 | 7-bit | 0–127 | ASCII |
| 5 | 8-bit | 0–255 | **BOSS: Satu Byte** |
| 6 | 9-bit | 0–511 | — |
| 7 | 10-bit | 0–1023 | Kilo |
| 8 | 11-bit | 0–2047 | **BOSS: Overflow** |

**Bantuan nilai tempat** (`1024 512 256 128 64 32 16 8 4 2 1`) tampil sebagai label di
bawah tiap bit pada level 1–4, memudar pada level 5–6, dan hilang pada level 7–8.
Murid belajar melepas "roda bantu" tanpa disuruh.

---

## 3. Arah Konversi — Dipilih, Bukan Diacak

Tipe soal **tidak pernah diacak**. Murid memilih arah konversinya lewat satu sakelar besar
di layar judul, lalu permainan **langsung dimulai** — tanpa layar pengaturan di antaranya.

```
        ┌─────────────────────┬─────────────────────┐
        │   BINER → DESIMAL   │   DESIMAL → BINER   │
        │      1011 → ?       │      11 → ?         │
        └─────────────────────┴─────────────────────┘
                    [  M U L A I  ]
```

Satu sesi = satu arah, dari soal pertama sampai terakhir. Konsistensi ini justru
menguntungkan: murid masuk ke satu pola pikir dan tetap di sana, sehingga kecepatannya
benar-benar terukur — bukan terganggu biaya berpindah cara berpikir. Guru juga bisa
menugaskan satu arah dulu pada pertemuan ini, arah sebaliknya pada pertemuan berikutnya.

### Sisi A — BINER → DESIMAL
Layar menampilkan `1011 0110`. Murid mengetik angka lalu menekan `Enter`.
Input numerik, otomatis fokus, tidak perlu menyentuh mouse sama sekali.

### Sisi B — DESIMAL → BINER
Layar menampilkan `182`. Murid **menyalakan bit** dengan mengklik/menekan deretan
tombol lampu. *Ini kunci UX*: mengetik 11 digit biner itu melelahkan dan rawan salah
ketik; menyalakan lampu justru membangun intuisi nilai tempat. Bit juga bisa
di-*toggle* lewat tombol angka `1`–`9`, `0`, `-`.

Jawaban **dikunci dengan `Enter`/tombol KIRIM**, tidak dicek otomatis saat bit terakhir
menyala. Kalau dicek otomatis, murid bisa menyalakan bit asal-asalan sampai kebetulan
benar tanpa pernah berhitung. Dengan tombol kirim, menebak jadi mahal: salah berarti
combo hangus, nyawa berkurang, dan waktu terpotong 4 detik.

Jadi sakelar itu sekaligus mengganti cara menjawab — mengetik angka versus menyalakan
lampu. Murid langsung paham sedang berada di mode yang mana tanpa perlu membaca petunjuk.

### Boss Round
Di level 5 dan 8, tiga soal datang berantai tanpa jeda — **tetap pada arah yang sama**,
nilai dobel, satu kali salah langsung gugur dari ronde bos.

### Mengganti arah
Sakelar tetap tampil di layar hasil. Selesai satu ronde, murid bisa membalik arah dan
menekan main lagi dalam dua ketukan — tanpa kembali ke menu.

---

## 4. Empat Mekanik Adrenalin

### 4.1 Timer — tekanan utama
- Hitung mundur **90 detik** untuk satu ronde (bukan per soal).
- Jawaban benar **menambah waktu**: +2s (4–6 bit), +3s (7–8 bit), +4s (9–11 bit).
  Semakin berani naik level, semakin lama bertahan → mendorong murid menantang diri.
- Jawaban salah **−4 detik**.
- Di bawah 10 detik: bar berubah merah, berdenyut, dan ada bunyi detak.
  *Ini momen paling menegangkan — sengaja dibuat terlihat dari jauh agar teman sebangku ikut tegang.*

### 4.2 Combo & Nyawa — hukuman yang terasa
- **Combo**: 3 benar berturut → ×1.5 · 6 benar → ×2 · 10 benar → ×3 · 15 benar → ×5 (api menyala di layar).
- Satu kali salah → combo **hangus total**. Ini yang membuat jawaban ke-9 terasa mendebarkan.
- **Nyawa**: 3 buah. Habis = permainan berakhir walau waktu masih tersisa.

### 4.3 Skor
```
skor = (lebar_bit × 10) × pengali_combo × bonus_kecepatan
bonus_kecepatan:  < 2 detik → ×2   |   < 4 detik → ×1.5   |   selebihnya ×1
```
Jadi satu soal 11-bit, combo ×3, dijawab dalam 1,8 detik = 110 × 3 × 2 = **660 poin**.
Rentang skor yang lebar membuat papan peringkat terasa hidup.

### 4.4 Level & Unlock
- Naik level otomatis setelah **5 benar berturut-turut**.
- Turun level setelah **2 salah berturut-turut** — kesulitan adaptif, agar murid yang
  tertinggal tidak patah semangat sementara yang cepat tetap tertantang.
- **Lencana** (tersimpan permanen di perangkat): *Nibble Master* (tembus level 1),
  *Byte Lord* (tuntaskan 8-bit), *Overflow Survivor* (tembus 11-bit), *Byte Sempurna*
  (10 benar tanpa salah), *Kilat* (jawab di bawah 1 detik), *Tanpa Bantuan*
  (selesai dengan label nilai tempat dimatikan).

---

## 5. Mode INDIVIDU vs Mode KELOMPOK

### 5.1 Mode Individu

| Sub-mode | Aturan |
|---|---|
| **Latihan** | Tanpa timer, tanpa nyawa, bantuan nilai tempat selalu tampil. Jawaban salah memunculkan penjelasan langkah demi langkah (`182 = 128+32+16+4+2`). Wajib dimainkan sekali sebelum mode bertekanan — menurunkan kecemasan sebelum adrenalin dinaikkan. |
| **Solo Sprint** | 90 detik · 3 nyawa · mulai 4-bit · level adaptif · berakhir dengan skor + Kode Hasil. Ini mode penilaian utama. |
| **Survival** | Tanpa timer, 3 nyawa, level naik terus dan tidak pernah turun. Untuk murid yang ingin mengejar rekor pribadi. |

### 5.2 Mode Kelompok — tiga varian sesuai perangkat

**A. ESTAFET BIT** *(1 perangkat per kelompok — pilihan utama)*
- Sebelum tiap soal, layar bertanya **"SIAPA YANG MENJAWAB?"** dan tim memilih sendiri anggotanya. Aturan kerasnya: **satu orang tidak boleh menjawab dua kali berturut-turut** — tombolnya benar-benar dimatikan.
- **3 nyawa dipakai bersama satu tim** — kesalahan satu orang terasa oleh semua. Tekanan sosial yang positif.
- Timer 120 detik terus berjalan **termasuk saat memilih penjawab dan mengoper perangkat** → memaksa tim bergerak cepat dan kompak.
- **Bonus putaran penuh:** setiap kali seluruh anggota sudah menjawab benar satu putaran, tim mendapat **+8 detik**. Hadiahnya datang saat itu juga, bukan menunggu akhir permainan.
- **Faktor pemerataan** di akhir: skor dikali sampai ×1,2 bila semua anggota pernah menjawab benar.
- Level dikunci guru dan tidak naik sendiri, supaya skor antar kelompok setara.

> *Catatan perubahan dari rancangan awal.* Semula giliran dirancang berputar otomatis dan
> combo hanya naik bila penjawab berganti. Saat dibangun, kombinasi itu ternyata tidak
> punya gigi: kalau giliran sudah dipaksa berputar, syarat "penjawab harus berbeda" selalu
> terpenuhi dengan sendirinya sehingga tidak mengubah apa pun. Pilihan bebas + larangan
> menjawab dua kali berturut-turut + bonus putaran penuh membuat keputusan itu kembali ada
> di tangan tim — dan tim yang menyembunyikan anggota lemahnya benar-benar kehilangan waktu
> dan skor.

**A-2. MARATON KELAS** *(satu kelas, satu perangkat dioper berkeliling)*
- Peserta **tanpa batas jumlah**; boleh diisi nama, boleh dinomori otomatis mengikuti absen.
- Giliran **berurutan dari nomor 1**, berpindah sendiri tanpa layar pemilihan — semua pasti
  kebagian, dan tidak ada ketukan tambahan yang memakan waktu.
- **Tidak ada nyawa.** Kelas boleh terus mencoba sampai waktu habis.
- Lulus bila mencapai **rentetan benar sesuai target** (bawaan: 15 rentetan dalam 20 menit).
  **Satu orang salah, rentetan kembali ke nol.**
- Ini pembalikan yang menarik dari mode individu: di sini murid yang sudah paham jadi
  berkepentingan memastikan temannya paham juga, karena satu kesalahan menghapus kerja
  seluruh kelas. Tekanan datang dari rekan, bukan dari guru — dan tidak ada cara memenangkannya
  selain benar-benar mengajari yang belum bisa.

**B. DUEL REBUTAN** *(1 perangkat, layar dibagi dua — untuk babak final)*
- Layar terbagi dua, satu soal di tengah, dua kelompok berlomba menekan lebih dulu.
- Jawaban berupa **4 pilihan ganda** (mengetik di satu keyboard oleh dua tim tidak praktis).
  Tim kiri memakai tombol `Q W A S`, tim kanan `↑ ↓ ← →`; di HP cukup ketuk sisi masing-masing.
- **Salah jawab = terkunci 3 detik.** Ini yang mencegah asal tekan — tim harus tetap berhitung.
- Format *best of 11*, menang bila lebih dulu mencapai 6 poin.

**C. KUIS KELAS** *(hanya 1 layar proyektor guru)*
- Guru menekan `Spasi` untuk memunculkan soal; hitung mundur 15 detik tampil sangat besar.
- Kelompok menjawab di kertas/papan kecil dan mengangkatnya bersamaan saat waktu habis
  (mencegah saling contek dan membuat momen pengangkatan jadi dramatis).
- Guru menekan angka `1`–`8` untuk memberi poin ke kelompok yang benar. Papan skor tampil di sisi layar.
- **RONDE TARUHAN** (soal terakhir): tiap kelompok mempertaruhkan sebagian poinnya *sebelum* soal muncul. Kelompok yang tertinggal jauh masih punya peluang membalik keadaan — ini puncak ketegangan paling efektif untuk satu kelas.

### 5.3 Ringkasan Perbedaan Aturan

| Aspek | Individu | Kelompok |
|---|---|---|
| Nyawa | 3, milik sendiri | 3, **milik bersama satu tim** |
| Siapa yang menjawab | selalu murid itu sendiri | dipilih tim, **tak boleh dua kali berturut-turut** |
| Timer | 90 detik pribadi | 120 detik tim, terus jalan saat memilih & mengoper / 15 detik per soal di Kuis Kelas |
| Kesulitan | adaptif per murid | ditetapkan guru, sama untuk semua (agar adil dibandingkan) |
| Kemenangan | skor absolut + rekor pribadi | peringkat relatif antar kelompok |
| Pemerataan | — | **+8 detik** tiap putaran penuh, lalu **faktor pemerataan** di akhir: skor × (1 + 0,2 × rasio anggota yang pernah menjawab benar) — sampai +20%. |

---

## 6. Alur Layar

```
LAYAR JUDUL
│   [ SAKELAR ARAH: BIN→DEC  |  DEC→BIN ]   ← selalu terlihat
│
├── Latihan   → langsung main (santai, tanpa tekanan)
├── Individu  → langsung main (SOLO SPRINT) → Hasil + Kode Hasil
├── Kelompok  → pilih varian A/B/C → daftar nama anggota → langsung main
└── Guru
    ├── Papan Kelas  (tempel Kode Hasil murid → rekap peringkat)
    └── Kode Sesi    (samakan urutan soal seluruh kelas)
```

Aturannya: **sakelar arah → tombol mode → soal pertama muncul.** Tidak ada layar
pengaturan yang menghadang. Satu-satunya jalur yang butuh langkah tambahan adalah mode
kelompok — karena nama anggota memang harus didaftarkan dulu agar giliran estafet bisa
disebutkan di layar.

---

## 7. Pertimbangan Kelas Nyata

- **Mode Proyektor** — satu tombol memperbesar seluruh antarmuka agar terbaca dari bangku belakang.
- **Suara bisa dimatikan.** Efek suara dibangkitkan lewat Web Audio API (tanpa file eksternal, agar tetap satu file). Default: menyala, tapi tombol bisu selalu terlihat.
- **Keyboard-first** — angka lalu `Enter`; tombol bit juga bisa ditekan lewat angka `1`–`8`/`0`. Tidak wajib pakai mouse.
- **Responsif HP** — tata letak potret, tombol bit besar dan mudah disentuh.
- **Kontras tinggi & `prefers-reduced-motion`** dihormati, untuk murid yang sensitif terhadap animasi cepat.
- **Riwayat lokal** — skor tersimpan di `localStorage`, sehingga rekor pribadi bertahan antar pertemuan tanpa perlu login.

---

## 8. Rencana Implementasi Bertahap

| Fase | Isi | Status |
|---|---|---|
| 1 | Sakelar arah + mesin soal dua arah + Solo Sprint + timer/combo/nyawa | ✅ selesai |
| 2 | Tangga 8 level + lencana + rekor lokal + mode Latihan | ✅ selesai |
| 3 | Estafet Bit + Duel Rebutan | ✅ selesai |
| 4 | Kuis Kelas (proyektor) + Papan Kelas + Kode Sesi/Kode Hasil | ✅ selesai |

Struktur repositori:

```
/
├── index.html      ← seluruh permainan
├── README.md       ← panduan singkat untuk guru
└── .nojekyll       ← agar GitHub Pages tidak memproses ulang
```
