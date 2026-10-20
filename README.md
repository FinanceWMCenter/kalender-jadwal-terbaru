# Calendar

Kalender interaktif bergaya Google Calendar untuk mengelola empat jenis jadwal:

| Kategori             | Warna              |
| -------------------- | ------------------ |
| Pembayaran Sewa      | Coral pastel       |
| Pembayaran Rutin     | Kuning/gold pastel |
| Meeting              | Biru pastel        |
| Task/Report          | Ungu pastel        |
| Status selesai/lunas | Sage green pastel  |

Aplikasi ini murni HTML, CSS, dan JavaScript tanpa framework atau build step, sehingga bisa langsung di-hosting di GitHub Pages.

## Struktur proyek

```
kalender/
├── index.html        Struktur halaman, navbar, sidebar, dan modal
├── style.css         Tema pastel, layout, dan tampilan responsif
├── app.js            Logika kalender, CSV, pengingat, dan sinkron Google Sheet
├── config.js         Pengaturan sambungan Google Sheet (kosong = mode lokal)
├── apps-script/
│   ├── Code.gs       Kode Apps Script utama yang ditempel di Google Sheet
│   └── Rapikan.gs    File tambahan: rapikan tampilan Sheet, tab rekap, izin Drive
├── contoh-data.csv   Contoh jadwal sewa (data fiktif) untuk mencoba Upload CSV
└── README.md
```

## Menjalankan di VS Code

1. Buka folder `kalender` lewat **File → Open Folder**.
2. Pasang ekstensi **Live Server** (Ritwick Dey) dari panel Extensions.
3. Klik kanan `index.html` → **Open with Live Server**. Aplikasi terbuka di `http://127.0.0.1:5500`.

Membuka `index.html` langsung dengan klik dua kali juga bisa, tetapi notifikasi browser hanya berjalan di `http://localhost` atau `https://` (termasuk GitHub Pages).

## Fitur

- Tampilan **Month / Week / Day**, tombol **Today**, panah navigasi, dan pemilih bulan-tahun.
- Sidebar kiri yang bisa dilipat: tombol **Create** dan **Upload CSV**, kalender kecil, filter **My Calendars**, filter **Cabang**, ringkasan tagihan bulan berjalan, kartu **Kebutuhan dana**, dan menu kelola data.
- Sidebar kanan yang bisa dilipat: tab **Pengingat** (Pembayaran Rutin H-1 dan Hari-H, Pembayaran Sewa H-30 sampai jatuh tempo, termasuk pending task) dan tab **Detail** event.
- **Pending task** seperti Google Calendar: tugas atau pembayaran yang sudah lewat tanggalnya tetapi belum dicentang ikut tampil di hari ini (paling atas, berlabel _Pending_ dan tanggal aslinya) sampai dicentang Selesai/Lunas. Meeting yang sudah lewat tidak dihitung pending.
- **Hapus data (buat ulang jadwal)**: pilih kategori dan cabang yang ingin dihapus, lalu buat ulang lewat Create atau Upload CSV.
- **Kelola cabang**: daftar cabang resmi dengan entitas/PT, ganti nama sekaligus untuk semua jadwal, dan nonaktifkan cabang yang tutup.
- **Bukti pembayaran**: catat tanggal bayar, unggah file bukti berupa gambar atau PDF (di kedua mode) atau tempel link, dan keterangan transfer.
- **Rekap tagihan**: Periode (bulan), Kategori Jadwal, Jumlah Tagihan, Sudah Lunas, Belum Lunas, dan Cabang (khusus Pembayaran Sewa), bisa diunduh sebagai CSV.
- **Email pengingat harian** (mode Google Sheet): ringkasan jadwal terlambat, hari ini, 7 hari ke depan, dan pengingat awal.
- Checkbox di setiap event. Saat dicentang, status menjadi Selesai/Lunas, teks tercoret, dan warna berubah ke sage green. Status tersimpan permanen di `localStorage`.
- Event berulang (mingguan, bulanan, tahunan). Tanggal 31 otomatis menyesuaikan ke akhir bulan pada bulan yang lebih pendek.
- Seret event untuk memindahkan tanggal atau jam. Seret file CSV ke halaman untuk mengimpor.
- Notifikasi browser opsional: Pembayaran Rutin H-1 dan Hari-H; Pembayaran Sewa H-30, H-14, H-7, H-3, H-1, dan Hari-H; Meeting dan Task/Report H-30 sampai Hari-H.
- Ekspor semua data ke CSV sebagai cadangan, lalu impor kembali di browser atau perangkat lain.

Pintasan keyboard: `T` hari ini, `M` `W` `D` ganti tampilan, `←` `→` geser periode, `C` buat event, `Esc` tutup popover.

Navigasi dengan mouse dan touchpad:

- **Tampilan bulan**: gulir roda mouse ke bawah untuk bulan berikutnya, ke atas untuk bulan sebelumnya. Di layar pendek yang grid bulannya bisa digulir, gulir dulu sampai ujung, lalu gulir sekali lagi.
- **Tampilan minggu/hari**: roda mouse tetap menggulir jam. Ganti minggu/hari dengan **Shift + gulir**, atau gulir di atas baris nama hari.
- **Touchpad**: usap dua jari ke kiri/kanan di semua tampilan. Satu usapan = satu periode.
- **Tombol samping mouse** (Back/Forward): periode sebelumnya/berikutnya.
- **Layar sentuh (HP/tablet)**: usap ke kiri/kanan.
- **Kalender kecil** di sidebar kiri: gulir di atasnya untuk mengganti bulan.

## Kategori

Empat kategori bawaan selalu tersedia: **Pembayaran Sewa**, **Pembayaran Rutin**, **Meeting**, dan **Task/Report**. Di Google Sheet, nama tab lama (Pembayaran Sewa Kantor, Jadwal Meeting, Task & Report) tetap dikenali; jalankan `rapikanTampilanSheet` di `Rapikan.gs` untuk mengganti namanya. Kategori lain bisa ditambah tanpa batas:

- Klik **+ Tambah kategori** di bawah **My Calendars**, atau **+ Kategori baru** langsung di form **Create**.
- Isi nama, pilih warna, dan centang **Kategori pembayaran** jika kategori itu punya nominal, PPN/PPh, dan status Lunas (misalnya "Pembayaran Vendor"). Tanpa centang, statusnya Selesai seperti meeting.
- Kategori tambahan bisa diedit atau dihapus lewat ikon pensil di sampingnya. Kategori bawaan tidak bisa dihapus.
- Saat impor CSV, kolom `Kategori` yang isinya sama dengan nama kategori tambahan otomatis masuk ke kategori tersebut.

Isian form menyesuaikan kategori: cabang, sub unit, tahap, dan masa sewa hanya muncul untuk Pembayaran Sewa; PPN/PPh hanya untuk kategori pembayaran.

## Cabang

Isian **Cabang** di form Create berupa pilihan dari daftar cabang, sehingga tidak ada salah ketik ("Sunter" dan "sunter " tidak lagi terbaca sebagai dua cabang).

- **Kelola cabang** (sidebar kiri, khusus Admin): tambah cabang beserta entitas/PT, ubah nama atau entitas, dan nonaktifkan/aktifkan.
- **Tambah cepat**: di form Create, pilih **+ Tambah cabang baru…** di isian Cabang. Cabang langsung dibuat dan terpilih tanpa menutup form.
- **Ganti nama** memperbarui semua jadwal yang memakai cabang itu sekaligus. Judul otomatis seperti "Sewa Pemuda – Tahap 1" ikut berganti. Perubahan tercatat di riwayat.
- **Nonaktifkan** untuk cabang yang tutup atau pindah: cabang hilang dari pilihan form, tetapi jadwal lamanya tetap ada dan tetap tampil.
- **Filter cabang** di sidebar kiri menampilkan jadwal satu cabang saja. Ringkasan, pengingat, dan rekap dana ikut mengikuti filter ini.
- Cabang baru dari **Upload CSV** atau baris yang diketik langsung di Google Sheet otomatis masuk daftar. Pratinjau impor menyebut cabang baru yang akan ditambahkan.
- Mode Google Sheet: daftar tersimpan di tab `_Cabang` (kolom `Nama_Cabang`, `Entitas`, `Aktif`, `Keterangan`). Mode lokal: tersimpan di browser.

## Bukti pembayaran

Di panel **Detail** jadwal pembayaran, klik **Catat pembayaran & bukti**:

- **Tanggal bayar** (default hari ini, tidak boleh setelah hari ini).
- **File bukti** berupa gambar (JPG, PNG, WEBP, HEIC, dan lainnya) atau PDF, maksimal 10 MB. Di HP, tombol pilih file juga menawarkan kamera/galeri.
  - Mode Google Sheet: file disimpan di folder Google Drive **Bukti Bayar Kalender** milik Admin. Pengguna aktif di tab `_Pengguna` otomatis mendapat akses lihat ke folder itu. Jika unggahan gagal karena izin Drive, Admin menjalankan fungsi **izinkanAksesDrive** (file `Rapikan.gs`) sekali, lalu Deploy versi baru.
  - Mode lokal: file disimpan di browser ini (IndexedDB), bisa dibuka lewat **Lihat bukti**, tetapi tidak ikut Ekspor CSV dan tidak terlihat di perangkat lain.
- Atau **link bukti** (Google Drive, OneDrive, dan sebagainya). Hanya link `http://`/`https://` yang diterima.
- **Keterangan pembayaran**: nomor referensi transfer, bank, nama penerima.
- **Tandai lunas** tercentang otomatis.

Di Google Sheet, data ini tersimpan di kolom `Tgl_Bayar`, `Bukti_Bayar`, dan `Keterangan_Bayar` (dibuat otomatis saat pertama dipakai). Kontributor boleh mencatat bukti untuk jadwal milik orang lain selama `kontributor_boleh_centang` = TRUE, tetapi tetap tidak bisa mengubah isian lain. Kolom yang sama ikut diekspor dan bisa diimpor lewat CSV.

## Rekap tagihan & kebutuhan dana

- Kartu **Kebutuhan dana** di sidebar kiri: total tagihan belum lunas yang jatuh tempo dalam 30 hari, 90 hari, atau 1 tahun ke depan, dirinci per kategori, plus yang pending.
- **Rincian per bulan & cabang** membuka tabel rekap dengan kerangka:

  | Periode      | Kategori Jadwal  | Jumlah Tagihan | Sudah Lunas   | Belum Lunas   | Cabang         |
  | ------------ | ---------------- | -------------- | ------------- | ------------- | -------------- |
  | Oktober 2026 | Pembayaran Sewa  | Rp 18.500.000  | Rp 0          | Rp 18.500.000 | Cabang Selatan |
  | Oktober 2026 | Pembayaran Rutin | Rp 19.150.000  | Rp 18.300.000 | Rp 850.000    | –              |

  Pembayaran Sewa dirinci per cabang; kategori pembayaran lain satu baris per bulan. Setiap bulan diberi subtotal, dan ada total di baris terakhir. Pilih **Periode**: 12 bulan ke depan (ditambah baris pending sebelum bulan ini) atau Januari–Desember tahun tertentu.

- Di bawahnya ada rekap per cabang (belum lunas) dengan entitas dan jatuh tempo terdekat. Klik nama bulan untuk membukanya di kalender. **Unduh CSV** menyimpan kedua tabel dengan kolom yang sama.
- Rekap mengikuti filter kategori dan cabang di sidebar.

## Pengingat & email harian

**Pengaturan pengingat** (menu Kelola data, khusus Admin):

- **Pembayaran Rutin**: selalu H-1 dan Hari-H saja (tidak bisa diubah).
- **Pembayaran Sewa mulai**: H-30 (bawaan), H-14, atau H-7, sampai jatuh tempo. Tidak lagi ada pengingat H-60/H-90; nilai lama di `_Pengaturan` yang lebih dari 30 otomatis dibatasi menjadi H-30.
- **Meeting, Task/Report & lainnya**: jadwal masuk panel lonceng sejak H-berapa (default H-30).
- Grup di panel lonceng: Pending, Hari ini, Besok (H-1), 2–7 hari, dan 8–30 hari ke depan.
- **Email ringkasan harian** (mode Google Sheet): dikirim setiap hari pada jam yang dipilih ke penerima yang diisi (kosong = pemilik spreadsheet). Isi email: jadwal terlambat, jatuh tempo hari ini, 7 hari ke depan, serta pengingat awal, dengan aturan baru (Pembayaran Rutin H-1 dan Hari-H, sewa mulai H-30) setelah **pasangEmailPengingatRapi** di `Rapikan.gs` dijalankan sekali. Email tidak dikirim pada hari tanpa jadwal yang perlu diingatkan. Tombol **Kirim email uji ke saya** untuk mencoba.

Email dikirim oleh Apps Script lewat pemicu terjadwal, jadi tetap berjalan walaupun tidak ada yang membuka kalender. Kuota Gmail: 100 email/hari untuk akun Gmail biasa, 1.500/hari untuk Google Workspace, jauh di atas kebutuhan satu email per hari.

## Format CSV (bebas)

Tidak ada format khusus. Aplikasi mengenali kolom dari nama header-nya, dan **hanya kolom tanggal yang wajib**. Pemisah koma, titik koma (Excel Indonesia), dan tab dikenali otomatis. Contoh file jadwal sewa yang langsung bisa diunggah:

```
No;Cabang;Sub_Unit;Term_Tahap;Tanggal_Jatuh_Tempo;Nominal_IDR;Durasi_Sewa;Catatan
1;Kantor Pusat;Gedung A;Tahap 1;15/01/2027;120,000,000;01/03/2027 - 29/02/2028;PPN: 13200000 | PPh: 0
2;Kantor Pusat;;Tahap 2;01/12/2027;132,000,000;01/03/2028 - 28/02/2029;PPN: 14520000 | PPh: 0
```

| Isi                         | Nama kolom yang dikenali                               | Keterangan                                                                                                                             |
| --------------------------- | ------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| Tanggal jatuh tempo (wajib) | `Tanggal_Jatuh_Tempo`, `Jatuh_Tempo`, `Tanggal`, `Tgl` | `15/01/2026`, `2026-01-15`, `15 Jan 2026`. Jika nama kolom lain, kolom berisi tanggal tetap dideteksi otomatis.                        |
| Cabang                      | `Cabang`, `Lokasi`, `Outlet`                           | Sel kosong mengikuti baris di atasnya.                                                                                                 |
| Sub unit                    | `Sub_Unit`, `Unit`, `Gedung`, `Entitas`                | Sel kosong mengikuti baris di atasnya selama cabangnya sama.                                                                           |
| Tahap pembayaran            | `Term_Tahap`, `Term`, `Tahap`, `Termin`                | Mis. `Tahap 1`, `Deposit`, `Periode 2`.                                                                                                |
| Nominal                     | `Nominal_IDR`, `Nominal`, `Jumlah`, `DPP`              | `134,000,000`, `134.000.000`, `Rp 4.750.000,00`, `1,5jt`.                                                                              |
| Masa sewa                   | `Durasi_Sewa`, `Masa_Sewa`, `Periode`                  | Rentang tanggal: `01/03/2026 - 28/02/2027`.                                                                                            |
| PPN / PPh                   | kolom `PPN`, `PPh`, atau ditulis di Catatan            | `PPN: 14740000 \| PPh: 0` di Catatan otomatis dipisah.                                                                                 |
| Kategori                    | `Kategori`                                             | Opsional. Jika tidak ada, kategori dipilih di layar pratinjau (file berkolom cabang/tahap/masa sewa otomatis menjadi Pembayaran Sewa). |
| Judul                       | `Judul`                                                | Opsional. Jika tidak ada, dibuat otomatis: `Sewa Pemuda Gedung A – Tahap 1`.                                                           |
| Waktu                       | `Waktu`, `Jam`                                         | Untuk meeting: `10:00-11:30`, `60 menit`.                                                                                              |
| Catatan, Status             | `Catatan`, `Status`                                    | Status `Lunas`/`Selesai`/`Belum`.                                                                                                      |
| Bukti bayar                 | `Tgl_Bayar`, `Bukti_Bayar`, `Keterangan_Bayar`         | Opsional. `Bukti_Bayar` harus berupa link http(s).                                                                                     |

Kolom lain yang tidak dikenali tetap disimpan dan tampil sebagai info tambahan di panel detail.

### Pratinjau sebelum impor

- **Baru**: akan diimpor.
- **Mirip**: tanggal dan nominal sama dengan baris lain di cabang yang sama (mis. baris rekap per entitas). Tidak diimpor kecuali dicentang "Impor juga baris yang mirip".
- **Duplikat**: sudah ada di kalender atau sama persis dengan baris lain, selalu dilewati.
- **Perlu dicek**: tetap diimpor, tetapi ada kejanggalan, misalnya dua jatuh tempo pada tanggal yang sama untuk lokasi dan tahap yang sama dengan nominal berbeda.
- Jika file tidak punya kolom status, jatuh tempo yang sudah lewat otomatis ditandai lunas (bisa dimatikan di pratinjau) agar tidak memenuhi daftar "Terlambat".

### Mengganti data dengan file yang diperbarui

Saat file yang sama diunggah ulang (misalnya setelah tanggal yang salah diperbaiki), pratinjau menampilkan pilihan **Data dari impor sebelumnya**:

- **Tambahkan**: hanya baris yang belum ada yang masuk. Data lama tetap ada.
- **Ganti dengan file ini**: semua event hasil impor CSV sebelumnya di kategori yang sama dihapus, lalu diganti isi file baru. Event yang dibuat manual tidak tersentuh, dan status lunas yang sudah Anda centang tetap dipertahankan untuk jadwal yang sama.

Setiap impor bisa dibatalkan lewat tombol **Urungkan** pada notifikasi di kiri bawah.

### Data contoh

Data contoh hanya dimuat sekali, saat aplikasi pertama kali dibuka di sebuah browser, lalu tersimpan di browser itu. Hapus kapan saja lewat **Kelola data → Hapus data contoh**, atau centang **Hapus data contoh bawaan** di layar pratinjau impor (tercentang otomatis jika data contoh masih ada).

### Ekspor

**Ekspor semua ke CSV** menghasilkan file berpemisah titik koma dengan kolom `No;Kategori;Judul;Cabang;Sub_Unit;Term_Tahap;Tanggal_Jatuh_Tempo;Nominal_IDR;PPN;PPh;Masa_Sewa;Waktu;Catatan;Status;Tgl_Bayar;Bukti_Bayar;Keterangan_Bayar`, yang bisa langsung diimpor kembali.

## Dua mode penyimpanan

|                             | Mode lokal (`config.js` kosong) | Mode Google Sheet (`config.js` diisi)   |
| --------------------------- | ------------------------------- | --------------------------------------- |
| Tempat data                 | Browser masing-masing           | Satu Google Sheet milik Admin           |
| Login                       | Tidak ada                       | Akun Google, hanya email yang diizinkan |
| Antarperangkat / antarorang | Terpisah                        | Sama untuk semua, tersinkron otomatis   |
| Edit langsung di Sheet      | -                               | Muncul di kalender dalam ±20 detik      |

Semua fitur kalender (Create, Upload CSV, kategori tambahan, pengingat, ekspor, dan seterusnya) tersedia di kedua mode.

## Mode Google Sheet: cara memasang

Lakukan sekali saja dengan akun Google yang akan menjadi **Admin** (pemilik spreadsheet). Siapkan sekitar 20 menit.

### Langkah 1: Siapkan Google Sheet

1. Buat Google Spreadsheet kosong (atau unggah `template-kalender-kosong.xlsx` jika ingin tab panduan ikut tersedia). Tab kategori dan tab sistem dibuat otomatis oleh Apps Script pada Langkah 3.
2. Buka **File → Setelan**, lalu pastikan **Zona waktu** = _(GMT+07:00) Jakarta_.

### Langkah 2: Buat OAuth Client ID (untuk tombol "Masuk dengan Google")

1. Buka https://console.cloud.google.com, lalu buat project baru, misalnya `Kalender WM`.
2. Buka **Google Auth Platform** (dulu bernama _OAuth consent screen_) dan klik **Get started**:
   - App name: `Calendar`, User support email: email Anda.
   - Audience:
     - **Internal** jika memakai akun Google Workspace perusahaan dan semua pengguna memakai email domain kantor. Ini paling mudah: tidak perlu dipublikasikan, dan akun di luar domain otomatis tidak bisa login.
     - **External** jika ada pengguna dengan email di luar domain kantor (misalnya Gmail).
   - Contact information: email Anda, lalu **Create**.
3. Khusus Audience **External**: buka menu **Audience**, lalu klik **Publish app** sehingga statusnya _In production_. Untuk login dasar (nama dan email), langkah ini tidak memerlukan verifikasi Google. Tanpa langkah ini, hanya email yang didaftarkan sebagai _test user_ yang bisa login.
4. Buka menu **Clients** (atau **APIs & Services → Credentials**), lalu klik **Create client**:
   - Application type: **Web application**.
   - **Authorized JavaScript origins**, tambahkan:
     - `https://financewmcenter.github.io`
     - `http://localhost:5500`
     - `http://127.0.0.1:5500`
   - Klik **Create**, lalu salin **Client ID** (berakhiran `.apps.googleusercontent.com`).

### Langkah 3: Pasang Apps Script

1. Di Google Sheet, buka **Ekstensi → Apps Script**.
2. Hapus semua isi `Code.gs`, lalu tempel seluruh isi file `apps-script/Code.gs` dari proyek ini.
3. Di bagian atas, ganti `TEMPEL_CLIENT_ID_DI_SINI.apps.googleusercontent.com` dengan Client ID dari Langkah 2, lalu klik ikon **Simpan**.
4. Di toolbar editor, pilih fungsi **siapkanSheet**, lalu klik **Run** dan izinkan semua akses yang diminta (Google Sheet, Google Drive untuk bukti bayar, Gmail untuk email pengingat, dan pemicu terjadwal). Tab Pembayaran Sewa Kantor, Pembayaran Rutin, Jadwal Meeting, Task & Report, serta tab sistem (`_Kategori`, `_Cabang`, `_Pengguna`, `_Riwayat`, `_Pengaturan`) langsung terbentuk, dan lembar kosong "Sheet1" dihapus.
5. Klik **Deploy → New deployment**, klik ikon gerigi, lalu pilih **Web app**:
   - Execute as: **Me**
   - Who has access: **Anyone**
6. Klik **Deploy**. Jika diminta **Authorize access**, pilih akun Anda. Jika muncul peringatan _Google hasn't verified this app_, klik **Advanced → Go to … (unsafe)** lalu **Allow**. Peringatan ini wajar karena skripnya milik Anda sendiri.
7. Salin **Web app URL** (berakhiran `/exec`).

"Anyone" di sini tidak berarti data terbuka. Setiap permintaan tetap harus login Google dan dicocokkan dengan tab `_Pengguna`. Pada akun Google Workspace, jika pilihan "Anyone" tidak tersedia (hanya ada "Anyone within domain"), minta Admin Google Workspace mengizinkan berbagi Web App Apps Script ke luar domain. Pilihan "Anyone within domain" tidak bisa dipakai, karena kalender di GitHub Pages memanggil Apps Script tanpa cookie login Google.

### Langkah 4: Hubungkan aplikasi

Isi `config.js`:

```js
window.CALENDAR_CONFIG = {
  appsScriptUrl: "https://script.google.com/macros/s/XXXXXXXX/exec",
  googleClientId: "XXXXXXXX.apps.googleusercontent.com",
};
```

Commit dan push lewat VS Code (**Source Control → Commit → Sync Changes**). Dalam 1–2 menit, situs GitHub Pages menampilkan layar login.

### Langkah 5: Atur pengguna

- Pemilik spreadsheet otomatis menjadi **Admin**.
- Tambahkan pengguna lain di tab `_Pengguna`: kolom `Email`, `Nama`, `Peran` (Admin / Kontributor / Pembaca), dan `Aktif` (TRUE/FALSE). Isi juga baris untuk email Anda sendiri agar **nama** Anda yang tampil di kalender, bukan alamat email.
- Hak akses:
  - **Admin** bisa melakukan semuanya.
  - **Kontributor** bisa menambah jadwal dan mengubah atau menghapus jadwal miliknya sendiri. Jadwal milik orang lain hanya bisa dicentang Lunas/Selesai, dan izin ini bisa dimatikan di `_Pengaturan` (`kontributor_boleh_centang`).
  - **Pembaca** hanya bisa melihat.
- Mengubah `Aktif` menjadi FALSE langsung mencabut akses.
- Alternatif untuk satu domain kantor: di `_Pengaturan`, isi `mode_akses` = `domain` dan `domain_kantor` = domain email kantor (misalnya `wmcenter.id`). Semua email domain itu bisa masuk sebagai Kontributor tanpa didaftarkan satu per satu; email yang tercantum di `_Pengguna` tetap memakai peran yang tertulis di sana.

### Langkah 6: Pindahkan data lama (opsional)

Jika sebelumnya Anda memakai mode lokal di browser yang sama, di **Kelola data** akan muncul tombol **Kirim … jadwal dari browser ini ke Sheet**. Cara lainnya: ekspor CSV dari browser lama, lalu **Upload CSV** setelah login.

### Cara kerja sinkron

- Setiap kategori adalah satu **tab** di Sheet. Tab baru otomatis menjadi kategori baru. Tab berawalan `_` adalah tab sistem.
- Perubahan dari kalender tertulis ke Sheet dalam 1–2 detik. Perubahan langsung di Sheet terbaca kalender setiap ±20 detik (atur di `_Pengaturan`), dan setiap kali halaman dibuka kembali.
- Kolom sistem (ID, Dibuat_Oleh, Diubah_Oleh, Diubah_Pada, Dihapus, dan lain-lain) diisi otomatis. Jangan diubah manual.
- Menghapus dari kalender tidak menghapus baris, hanya mengisi `Dihapus = TRUE`, sehingga bisa dipulihkan.
- Jika dua orang mengubah jadwal yang sama hampir bersamaan, yang menyimpan belakangan diberi tahu dan datanya dimuat ulang, sehingga tidak ada perubahan yang tertimpa diam-diam.
- Semua perubahan tercatat di tab `_Riwayat` dan tampil di tab **Aktivitas** pada sidebar kanan.
- Baris yang belum bisa ditampilkan (misalnya tanggal kosong) diberi keterangan di kolom `Catatan_Sistem` dan ditampilkan ke Admin di tab Aktivitas.
- Perubahan butuh koneksi internet. Jika koneksi terputus saat menyimpan, perubahan itu dibatalkan dan Anda diberi tahu. Kalender tetap menampilkan data terakhir.

### Isi tab `_Pengaturan`

Baris yang belum ada ditambahkan otomatis; nilai yang sudah diisi tidak pernah ditimpa. Sebagian juga bisa diubah lewat **Pengaturan pengingat** di kalender.

| Kunci                       | Bawaan         | Keterangan                                                                      |
| --------------------------- | -------------- | ------------------------------------------------------------------------------- |
| `mode_akses`                | `daftar_email` | `domain` = semua email `domain_kantor` boleh masuk                              |
| `domain_kantor`             | (kosong)       | Mis. `wmcenter.id`                                                              |
| `peran_default_domain`      | `Kontributor`  | Peran email domain yang tidak tercantum di `_Pengguna`                          |
| `kontributor_boleh_centang` | TRUE           | Kontributor boleh centang Lunas/Selesai dan catat bukti bayar jadwal orang lain |
| `interval_sinkron_detik`    | 20             | 10–300                                                                          |
| `hari_pengingat`            | 30             | Pengingat semua jadwal mulai H-berapa                                           |
| `hari_pengingat_sewa`       | 30             | Pengingat sewa mulai H-berapa (maksimal 30)                                     |
| `email_pengingat`           | FALSE          | TRUE = kirim email harian                                                       |
| `email_penerima`            | (kosong)       | Pisahkan dengan koma; kosong = pemilik spreadsheet                              |
| `jam_email`                 | 7              | Jam kirim (0–23)                                                                |

Jika `email_pengingat` atau `jam_email` diubah langsung di Sheet (bukan lewat kalender), jalankan fungsi **aturPengingatEmail** sekali di editor Apps Script agar jadwal email ikut diperbarui.

### Merapikan tampilan Google Sheet (Rapikan.gs)

File `apps-script/Rapikan.gs` adalah file **tambahan**, bukan pengganti `Code.gs`:

1. Di editor Apps Script, klik **+** di samping _Files_ → **Script**, beri nama `Rapikan`, tempel isi `apps-script/Rapikan.gs`, lalu **Simpan**.
2. Jalankan fungsi berikut **berurutan** (pilih lalu **Run**, izinkan akses yang diminta), kemudian **Deploy → Manage deployments → ikon pensil → Version: New version → Deploy**:
   1. **izinkanAksesDrive**
   2. **rapikanTampilanSheet**
   3. **pasangPemicuRekap**
   4. **pasangEmailPengingatRapi**

   Penjelasan tiap fungsi:
   - **rapikanTampilanSheet**: mengganti nama tab lama menjadi _Pembayaran Sewa_, _Pembayaran Rutin_, _Meeting_, _Task/Report_ (tab ganda digabung: tab kosong dihapus, isi tab lama dipindahkan ke tab bernama baru); mengurutkan tab (4 kategori utama, kategori tambahan, lalu tab sistem); memberi warna header dan tab sesuai kategori; format angka `#,##0` untuk Nominal/PPN/PPh dan `dd/mm/yyyy` untuk tanggal; checkbox untuk Lunas/Selesai; baris lunas berwarna hijau dan baris terhapus abu-abu tercoret; kolom sistem disembunyikan (tetap dibaca kalender, atur lewat `RAPIKAN_SEMBUNYIKAN_SISTEM_`). Aturan _conditional formatting_ lama di tab kategori diganti.
   - **perbaruiRekapTagihan**: membuat tab `_Rekap Tagihan` dengan kolom Periode, Kategori Jadwal, Jumlah Tagihan, Sudah Lunas, Belum Lunas, Cabang.
   - **pasangPemicuRekap**: memperbarui tab rekap otomatis setiap hari sekitar pukul 06.00.
   - **izinkanAksesDrive**: memberi izin Google Drive agar unggah bukti bayar (gambar/PDF) berhasil.
   - **pasangEmailPengingatRapi**: email harian dengan aturan baru (Pembayaran Rutin H-1 dan Hari-H; Pembayaran Sewa H-30, H-14, H-7 sampai Hari-H; pending task) pada jam `jam_email`. Pemicu email lama dari `Code.gs` otomatis dihapus, dan dibersihkan lagi setiap kali email baru berjalan, sehingga tidak ada email ganda. Email hanya dikirim bila `email_pengingat` = TRUE. **ujiEmailPengingatRapi** mengirim contoh email ke akun Anda.
3. Jika `siapkanSheet` di `Code.gs` membuat ulang tab bernama lama, jalankan lagi **rapikanTampilanSheet**. Sementara itu kalender tetap membaca keduanya dan menulis jadwal baru ke tab bernama baru.
4. Nilai lama `hari_pengingat_sewa` (mis. 90) otomatis diubah menjadi 30 di `_Pengaturan` saat Admin membuka kalender.

### Memperbarui Apps Script di kemudian hari

1. Buka Google Sheet → **Ekstensi → Apps Script**, hapus seluruh isi `Code.gs`, tempel isi `apps-script/Code.gs` yang baru, lalu **Simpan**. Pastikan `GOOGLE_CLIENT_ID` tetap berisi Client ID Anda.
2. Pilih fungsi **siapkanSheet** → **Run**. Bila Google meminta izin baru (misalnya Drive atau Gmail), klik **Review permissions → Advanced → Go to … (unsafe) → Allow**.
3. **Deploy → Manage deployments**, klik ikon pensil, pilih **Version: New version**, lalu **Deploy**. URL Web App tidak berubah, jadi `config.js` tidak perlu diubah.

Selama langkah ini belum dilakukan, kalender tetap berjalan, tetapi Admin melihat peringatan "Apps Script belum diperbarui" dan fitur yang membutuhkan Apps Script baru (Kelola cabang, unggah file bukti, email) menampilkan pesan yang menjelaskannya.

## Penyimpanan di mode lokal

Data disimpan di `localStorage` browser masing-masing pengguna. Artinya:

- Kode di repository bersifat publik, tetapi **data jadwal dan nominal Anda tidak ikut terunggah**.
- Setiap browser dan perangkat punya datanya sendiri. Gunakan **Ekspor semua ke CSV** lalu **Upload CSV** untuk memindahkan data.
- Menghapus data situs atau cache browser akan menghapus jadwal. Ekspor CSV secara berkala sebagai cadangan.

## Publikasi ke GitHub Pages

### Persiapan sekali saja

1. Buat akun di [github.com](https://github.com) jika belum punya.
2. Pasang Git dari [git-scm.com](https://git-scm.com/downloads), lalu buka ulang VS Code.
3. Atur identitas Git di Terminal VS Code (**Terminal → New Terminal**):

```bash
git config --global user.name "Nama Anda"
git config --global user.email "email@anda.com"
```

### Cara A: lewat tombol di VS Code (paling mudah)

1. Buka panel **Source Control** (ikon cabang di sidebar kiri, atau `Ctrl+Shift+G`).
2. Klik **Publish to GitHub**.
3. Masuk ke akun GitHub saat diminta, lalu pilih **Publish to GitHub public repository**.
4. Beri nama repository, misalnya `kalender-jadwal`, dan pastikan semua file tercentang.
5. VS Code membuat repository dan mengunggah semua file secara otomatis.

### Cara B: lewat terminal

1. Buat repository kosong di GitHub: klik **+ → New repository**, isi nama `kalender-jadwal`, pilih **Public**, lalu **jangan** centang "Add a README".
2. Jalankan perintah ini di Terminal VS Code dari dalam folder proyek (ganti `USERNAME` dengan username GitHub Anda):

```bash
git init
git add .
git commit -m "Kalender interaktif: versi awal"
git branch -M main
git remote add origin https://github.com/USERNAME/kalender-jadwal.git
git push -u origin main
```

Saat `git push` pertama kali, VS Code akan meminta login GitHub lewat browser.

### Mengaktifkan GitHub Pages

1. Buka repository di GitHub, lalu klik **Settings**.
2. Pilih **Pages** di menu kiri.
3. Pada **Build and deployment → Source**, pilih **Deploy from a branch**.
4. Pada **Branch**, pilih `main` dan folder `/ (root)`, lalu klik **Save**.
5. Tunggu 1 sampai 2 menit, lalu muat ulang halaman Settings → Pages. Link aplikasi muncul di bagian atas:

```
https://USERNAME.github.io/kalender-jadwal/
```

Link ini bisa dibuka siapa saja.

### Memperbarui aplikasi

Setelah mengubah kode:

```bash
git add .
git commit -m "Jelaskan perubahan di sini"
git push
```

Atau lewat panel Source Control: tulis pesan, klik **Commit**, lalu **Sync Changes**. GitHub Pages memperbarui situs dalam 1 sampai 2 menit.

## Kustomisasi cepat

- **Awal minggu**: ubah `WEEK_START` di bagian atas `app.js` (`0` = Minggu, `1` = Senin).
- **Rentang pengingat**: ubah `REMINDER_DAYS` dan `NOTIFY_MILESTONES` di `app.js`.
- **Nama dan warna kategori**: label ada di objek `CATEGORIES` di `app.js`, warna ada di kelas `.cat-sewa`, `.cat-rutin`, `.cat-meeting`, `.cat-task` di `style.css`.
- **Data contoh**: saat pertama dibuka, aplikasi memuat data contoh. Hapus lewat **Hapus data (buat ulang jadwal)**, atau kosongkan isi fungsi `sampleEvents()` jika tidak ingin ada data contoh sama sekali.

## Hapus data & buat ulang jadwal

**Kelola data → Hapus data (buat ulang jadwal)** membuka pilihan:

- **Kategori** yang dihapus (jumlah jadwal tampil di sampingnya) dan **Cabang** (semua atau satu cabang).
- **Termasuk jadwal yang sudah lunas/selesai**: matikan bila hanya ingin menghapus yang belum lunas.
- **Ekspor cadangan** menyimpan semua data ke CSV sebelum dihapus.

Mode lokal: bisa diurungkan lewat tombol **Urungkan**. Mode Google Sheet: khusus Admin, baris ditandai `Dihapus = TRUE` (tidak benar-benar hilang, bisa dipulihkan dengan mengosongkan kolom itu di Sheet). Setelah menghapus, buat ulang jadwal lewat **Create** atau **Upload CSV**.
