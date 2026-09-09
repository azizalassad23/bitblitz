# BIT BLITZ DARING — panduan pemasangan

Mode daring untuk pembelajaran jarak jauh: murid bergiliran menjawab dari HP
masing-masing, dan nilainya masuk sendiri ke Google Spreadsheet Anda.

Berbeda dari `index.html` (permainan luring yang berjalan sendiri di satu perangkat),
`daring.html` membutuhkan **satu server kecil** sebagai wasit — dibuat gratis lewat
Google Apps Script memakai akun Google yang sudah Anda punya.

---

## Bagian 1 — Memasang server (sekali saja, ±10 menit)

1. Buka **[sheets.new](https://sheets.new)** untuk membuat Spreadsheet baru.
   Beri nama, misalnya *Nilai Konversi Biner*.
2. Di Spreadsheet itu, buka menu **Extensions → Apps Script**.
3. Hapus seluruh isi `Code.gs`, lalu tempel **seluruh isi berkas `apps-script.gs`**.
4. Cari baris berikut di dekat atas, ganti angkanya dengan PIN rahasia Anda:

   ```js
   var PIN_GURU = '1234';
   ```

   PIN ini yang mencegah murid membuat atau menghentikan sesi. Jangan dibagikan.
5. Klik ikon **Save** (💾).
6. Klik **Deploy → New deployment**.
   - **Select type** → ikon roda gigi → **Web app**
   - **Description** : bebas
   - **Execute as** : **Me**
   - **Who has access** : **Anyone** ← wajib, kalau tidak murid tidak bisa masuk
   - klik **Deploy**
7. Google akan meminta izin. Pilih akun Anda → *Advanced* → *Go to … (unsafe)* →
   **Allow**. Peringatan itu muncul karena skripnya buatan sendiri, bukan dari toko
   aplikasi; isinya ada di depan mata Anda.
8. Salin **Web app URL**. Bentuknya seperti:

   ```
   https://script.google.com/macros/s/AKfycb…/exec
   ```

> **Setiap kali `apps-script.gs` diubah**, perubahan tidak langsung aktif.
> Buka **Deploy → Manage deployments → ikon pensil → Version: New version → Deploy**.

---

## Bagian 2 — Menyambungkan halaman ke server

Buka `daring.html`, cari baris ini di dekat awal `<script>`:

```js
const URL_SERVER = '';
```

Tempel URL tadi di antara tanda kutip:

```js
const URL_SERVER = 'https://script.google.com/macros/s/AKfycb…/exec';
```

Simpan, lalu unggah ke GitHub Pages bersama berkas lain:

| Berkas | Perlu diunggah? |
|---|---|
| `daring.html` | **ya** |
| `index.html` | ya (permainan luring) |
| `tailwind.css` | **ya** — gaya untuk keduanya |
| `.nojekyll` | ya |
| `apps-script.gs` | tidak (isinya sudah ditempel ke Apps Script) |

Alamat untuk murid menjadi:
`https://<nama-akun>.github.io/<nama-repo>/daring.html`

---

## Bagian 3 — Menjalankan satu sesi

### Guru

1. Buka `daring.html`, pilih **SAYA GURU**.
2. Tekan **TES KONEKSI**. Harus muncul *"Tersambung. Versi server 1."*
   Kalau gagal, lihat daftar masalah di bawah.
3. Isi **Kode sesi** (mis. `XIA-2026`), **PIN guru**, lalu atur:
   - **Tingkat kesulitan** — 4-bit sampai 11-bit, dipilih sebelum permainan dimulai
   - **Arah konversi** — Biner→Desimal atau Desimal→Biner
   - **Target rentetan benar** — syarat kelas dinyatakan lulus
   - **Batas waktu per giliran** — 20–30 detik biasanya pas
4. Tekan **BUAT SESI**, lalu bagikan kode sesinya ke murid.
5. Setelah semua bergabung (jumlahnya terlihat di tabel), tekan **MULAI**.
6. Selesai bermain, tekan **AKHIRI & SIMPAN NILAI**.

### Murid

1. Buka tautan yang sama, pilih **SAYA MURID**.
2. Masukkan **kode sesi** dan **nama asli** (nilai direkap berdasarkan nama ini).
3. Tunggu. Semua murid melihat soal yang sama dan melihat siapa yang sedang bergiliran;
   kotak jawaban **hanya aktif pada giliran sendiri**.

---

## Bagian 4 — Kalau ada murid yang sinyalnya putus

Sudah ditangani otomatis. Giliran yang tidak dijawab sampai melewati batas waktu akan
**dilewati sendiri** kira-kira 11 detik setelahnya, dicatat sebagai *terlewat*, dan
permainan lanjut ke murid berikutnya. Guru tidak perlu menekan apa pun.

Kalau ingin melewati lebih cepat, ada tombol **LEWATI GILIRAN** di panel guru.

Murid yang keluar lalu membuka tautan itu lagi dan memasukkan kode + nama yang sama akan
**tersambung kembali sebagai dirinya**, bukan sebagai peserta baru — asalkan memakai HP
dan browser yang sama.

---

## Bagian 5 — Hasil di Spreadsheet

Dua sheet dibuat sendiri:

**`Hasil`** — satu baris per murid tiap sesi:
waktu, kode sesi, tingkat, arah, nama, benar, salah, terlewat, akurasi, rata-rata detik,
tercepat, **keluar layar**, **total detik di luar**, **upaya tempel**, rentetan terbaik
kelas, dan hasil kelas.

**`Log`** — satu baris per jawaban: soalnya apa, jawabannya apa, benar/salah/telat, dan
berapa detik. Berguna untuk melihat pola, bukan sekadar angka akhir.

---

## Bagian 6 — Soal kecurangan, sejujurnya

**Halaman ini tidak bisa mencegah kecurangan, dan tidak ada halaman web yang bisa.**
Murid yang membuka kalkulator konversi di HP kedua atau di laptop sebelahnya sama sekali
tidak terlihat — itu di luar jangkauan browser mana pun.

Yang dilakukan halaman ini adalah membuat kecurangan **terlihat** dan **tidak sepadan**:

| Cara | Yang dilakukan |
|---|---|
| Catatan keluar layar | Menghitung berapa kali dan berapa lama murid berpindah aplikasi/tab selama permainan. Masuk ke kolom *Keluar layar* dan *Total detik di luar*. |
| Salin-tempel dimatikan | Menempel jawaban diblokir, dan upayanya dihitung. |
| Batas waktu per giliran | Berpindah ke aplikasi lain memakan detik yang justru berharga. Di 4–8 bit, menghitung sendiri sering lebih cepat. |
| Penilaian di server | Skor dihitung di server, bukan di HP — jadi tidak bisa diubah lewat alat pengembang browser. |
| Soal dibuat server | Murid tidak bisa melihat soal berikutnya sebelum gilirannya tiba. |
| Giliran dikunci | Server menolak jawaban dari murid yang bukan gilirannya. |

Dua hal yang perlu Anda sadari:

- **Angka "keluar layar" besar bukan bukti curang.** Bisa jadi ada telepon masuk atau
  notifikasi. Perlakukan sebagai bahan pertanyaan, bukan vonis.
- **Soal konversi tidak bisa disembunyikan jawabannya.** Soalnya *adalah* jawabannya
  dalam basis lain. Tidak ada rancangan yang mengubah kenyataan itu.

Pelengkap yang paling ampuh justru bukan kode: setelah sesi, minta satu-dua murid yang
skornya melonjak untuk mengulang satu soal secara lisan sambil menyebutkan langkahnya.
Dua menit, dan sangat menentukan.

Halaman ini **terbuka soal apa yang dicatat** — murid diberi tahu di layar sebelum masuk,
dan diberi tahu lagi saat mereka kembali setelah keluar layar. Pemantauan diam-diam
bukan hanya melanggar kepercayaan, tapi juga lebih buruk sebagai pencegah.

---

## Bagian 7 — Kalau ada masalah

| Gejala | Kemungkinan sebab |
|---|---|
| *"Gagal: Alamat server belum diisi"* | `const URL_SERVER` di `daring.html` masih kosong. |
| Tes koneksi gagal / halaman diam | **Who has access** belum disetel **Anyone** saat deploy. |
| Perubahan skrip tidak terasa | Belum **Deploy → Manage deployments → New version**. |
| *"PIN guru salah"* | PIN di panel berbeda dengan `PIN_GURU` di Apps Script. |
| *"Kode sesi tidak ditemukan"* | Sesi belum dibuat, atau kodenya beda (huruf besar/kecil tidak masalah). |
| Murid tidak bisa masuk setelah mulai | Peserta baru hanya boleh bergabung sebelum **MULAI**. Yang sudah terdaftar tetap bisa menyambung ulang. |
| Nilai tidak muncul di sheet | Nilai ditulis saat **AKHIRI & SIMPAN NILAI**, atau otomatis ketika target rentetan tercapai. |
| Lambat saat kelas besar | Apps Script gratis punya batas. Untuk 30+ murid, naikkan batas waktu per giliran ke 30 detik. |
