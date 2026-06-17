# BLUEPRINT FRONTEND UI/UX — PLATFORM KLINIK HEWAN

---

## DESIGN SYSTEM

### Color Palette
```
Primary     : #0EA5E9  (sky-500)     → brand utama, button, link
Primary Dark: #0284C7  (sky-600)     → hover state
Secondary   : #10B981  (emerald-500) → sukses, aktif, selesai
Warning     : #F59E0B  (amber-500)   → pending, peringatan stok
Danger      : #EF4444  (red-500)     → error, batal, kritis
Purple      : #8B5CF6  (violet-500)  → dokter, rekam medis
Gray Base   : #F8FAFC  (slate-50)    → background halaman
Card BG     : #FFFFFF               → background card
Sidebar BG  : #0F172A  (slate-900)  → sidebar gelap
Sidebar Text: #94A3B8  (slate-400)  → menu tidak aktif
Sidebar Act : #FFFFFF               → menu aktif
Border      : #E2E8F0  (slate-200)  → border card, input
Text Primary: #0F172A  (slate-900)  → judul, label
Text Muted  : #64748B  (slate-500)  → deskripsi, placeholder
```

### Typography
```
Font Family : Inter (Google Fonts)
─────────────────────────────────
Page Title  : 24px, font-bold, slate-900
Section Title: 18px, font-semibold, slate-900
Card Title  : 14px, font-semibold, slate-700
Body        : 14px, font-normal, slate-700
Small/Muted : 12px, font-normal, slate-500
Label       : 12px, font-medium, slate-600, uppercase tracking-wide
Badge       : 11px, font-semibold
```

### Spacing & Radius
```
Page Padding  : p-6 (24px)
Card Padding  : p-5 (20px)
Gap antar card: gap-4 (16px)
Border Radius : rounded-xl (card), rounded-lg (input, button), rounded-full (badge, avatar)
Shadow        : shadow-sm (card default), shadow-md (modal, dropdown)
```

### Status Color Mapping
```
Badge Status → Warna:
pending      → amber-100 text-amber-700
konfirmasi   → sky-100 text-sky-700
selesai      → emerald-100 text-emerald-700
batal        → red-100 text-red-700
aktif        → emerald-100 text-emerald-700
sembuh       → sky-100 text-sky-700
dirujuk      → violet-100 text-violet-700
meninggal    → slate-100 text-slate-600
kritis       → red-100 text-red-700
lemah        → orange-100 text-orange-700
stabil       → amber-100 text-amber-700
baik         → sky-100 text-sky-700
sangat_baik  → emerald-100 text-emerald-700
lunas        → emerald-100 text-emerald-700
draft        → slate-100 text-slate-600
```

---

## LAYOUT SISTEM

### Shell Utama (Dashboard)
```
┌──────────────────────────────────────────────────────┐
│ SIDEBAR (240px fixed)    │ MAIN AREA (flex-1)         │
│ ┌────────────────────┐   │ ┌──────────────────────┐   │
│ │ Logo + Nama Klinik │   │ │ TOPBAR (64px)        │   │
│ ├────────────────────┤   │ │ breadcrumb | user    │   │
│ │ Avatar + Nama User │   │ └──────────────────────┘   │
│ │ Badge Role         │   │                            │
│ ├────────────────────┤   │ PAGE CONTENT               │
│ │ NAV MENU           │   │ p-6, overflow-y-auto       │
│ │ > Menu Item        │   │                            │
│ │   Menu Item        │   │                            │
│ │   Menu Item        │   │                            │
│ ├────────────────────┤   │                            │
│ │ Logout Button      │   │                            │
│ └────────────────────┘   └──────────────────────────┘ │
└──────────────────────────────────────────────────────┘

Mobile (<768px):
- Sidebar collapse jadi overlay dengan hamburger toggle
- Topbar tetap fixed
- Content full width
```

### Sidebar Menu per Role
```
OWNER (slate-900 bg, sky-500 accent)
├── 🏠 Dashboard
├── 📊 Laporan
├── 👥 Manajemen User
├── 🐾 Semua Hewan
├── 📅 Appointment
├── 🏥 Rawat Inap
├── 📅 Booking Online
├── ⚙️ Pengaturan Klinik
└── 🚪 Logout

DOKTER (slate-900 bg, violet-500 accent)
├── 🏠 Dashboard
├── 📅 Appointment Saya
├── 📋 Rekam Medis
├── 🏥 Rawat Inap
├── 🐾 Data Hewan
└── 🚪 Logout

STAFF (slate-900 bg, emerald-500 accent)
├── 🏠 Dashboard
├── 🛒 POS Kasir
├── 📦 Inventory
├── 📅 Appointment
├── 🏥 Rawat Inap
├── 💰 Pengeluaran
├── 📅 Booking Online
└── 🚪 Logout

CUSTOMER (slate-900 bg, sky-500 accent)
├── 🏠 Dashboard
├── 🐾 Hewan Saya
├── 📋 Rekam Medis
└── 🏥 Monitoring
└── 🚪 Logout
```

---

## HALAMAN PER MODUL

---

### 1. LOGIN PAGE
```
Layout: center screen, bg-slate-50

┌─────────────────────────────────┐
│        [Logo Klinik]            │
│      Nama Klinik Hewan          │
│   Masuk ke Panel Manajemen      │
│                                 │
│  ┌───────────────────────────┐  │
│  │ Email                     │  │
│  └───────────────────────────┘  │
│  ┌───────────────────────────┐  │
│  │ Password              👁  │  │
│  └───────────────────────────┘  │
│  ┌───────────────────────────┐  │
│  │      Masuk  [loading]     │  │
│  └───────────────────────────┘  │
│   ⚠ pesan error di sini         │
└─────────────────────────────────┘

Behavior:
- Enter key submit form
- Loading state disable tombol + spinner
- Error shake animation
- Auto redirect ke /[role]/dashboard
```

---

### 2. DASHBOARD OWNER
```
┌─────────────────────────────────────────────────┐
│ Selamat datang, [nama] 👋        Hari, Tgl       │
├─────────────────────────────────────────────────┤
│ FILTER PERIODE: [ Hari Ini ▼ ] [Custom Range]   │
├──────────┬──────────┬──────────┬────────────────┤
│💰Pemasuk │💸Pengelu │📈Laba    │🧾Transaksi     │
│Rp xxx    │Rp xxx    │Rp xxx    │NN transaksi    │
│↑12% mtl  │↑5% mtl   │↑8% mtl  │hari ini        │
├──────────┴──────────┴──────────┴────────────────┤
│👥Pasien Baru │🏥Rawat Inap  │📅Appointment      │
│NN hari ini   │NN aktif      │NN hari ini        │
├─────────────────────────┬───────────────────────┤
│ LINE CHART              │ PIE CHART             │
│ Pemasukan vs Pengeluaran│ Metode Pembayaran     │
│ 30 hari terakhir        │ Tunai/QRIS/Transfer   │
├─────────────────────────┴───────────────────────┤
│ BAR CHART — Produk Terlaris (Top 10)            │
├─────────────────────────┬───────────────────────┤
│ ⚠ STOK MENIPIS          │ 🏥 RAWAT INAP AKTIF   │
│ List produk < minimum   │ List pasien aktif     │
│ [Kelola Inventory]      │ [Lihat Semua]         │
└─────────────────────────┴───────────────────────┘
```

---

### 2B. DASHBOARD DOKTER
```
┌─────────────────────────────────────────────────┐
│ Selamat datang, Dr. [nama] 👋                   │
├──────────┬──────────┬──────────┬────────────────┤
│📅Appt    │🏥Rawat   │📋Rekam   │🐾Pasien        │
│hari ini  │Inap Aktif│Medis Bln │Bulan Ini       │
├──────────┴──────────┴──────────┴────────────────┤
│ APPOINTMENT HARI INI                            │
│ ┌──────────────────────────────────────────┐   │
│ │ 08:00 │ Kucing │ Nama Hewan │ Pemilik  │ ● │   │
│ │ 09:00 │ Anjing │ Nama Hewan │ Pemilik  │ ● │   │
│ └──────────────────────────────────────────┘   │
├─────────────────────────────────────────────────┤
│ PASIEN RAWAT INAP AKTIF                        │
│ Kandang 1: [Nama Hewan] — Kondisi: Stabil      │
│ Kandang 2: [Nama Hewan] — Kondisi: Baik        │
└─────────────────────────────────────────────────┘
```

---

### 2C. DASHBOARD STAFF
```
┌──────────┬──────────┬──────────┬────────────────┐
│🛒Transaksi│📦Stok   │📅Appt    │📋Booking       │
│hari ini   │menipis  │hari ini  │menunggu        │
├───────────┴──────────┴──────────┴───────────────┤
│ SHORTCUT AKSI CEPAT                             │
│ [🛒 Buka POS]  [📅 Tambah Appointment]          │
│ [📦 Tambah Stok]  [✅ Konfirmasi Booking]       │
├─────────────────────────────────────────────────┤
│ APPOINTMENT HARI INI (list ringkas)             │
│ BOOKING MENUNGGU KONFIRMASI (list ringkas)      │
└─────────────────────────────────────────────────┘
```

---

### 2D. DASHBOARD CUSTOMER
```
┌──────────┬──────────┬──────────────────────────┐
│🐾Hewan   │📋Rekam   │🏥Monitoring              │
│Saya      │Medis     │Rawat Inap                │
├──────────┴──────────┴──────────────────────────┤
│ HEWAN SAYA (card grid)                         │
│ ┌────────┐ ┌────────┐                          │
│ │[foto]  │ │[foto]  │                          │
│ │Nama    │ │Nama    │                          │
│ │Kucing  │ │Anjing  │                          │
│ │● Aktif │ │● Aktif │                          │
│ └────────┘ └────────┘                          │
├─────────────────────────────────────────────────┤
│ KUNJUNGAN TERAKHIR                             │
│ [Nama Hewan] — [Diagnosis] — [Tanggal]         │
└─────────────────────────────────────────────────┘
```

---

### 3. MANAJEMEN USER (OWNER)
```
PageHeader: "Manajemen User" | [+ Tambah User]

FILTER BAR:
[ Semua Role ▼ ] [ Search nama/email... 🔍 ] [ Status: Semua ▼ ]

TABLE:
┌──────┬──────────────┬───────┬──────────┬────────┬─────────┐
│Avatar│Nama + Email  │Role   │No HP     │Status  │Aksi     │
├──────┼──────────────┼───────┼──────────┼────────┼─────────┤
│ 👤  │ Nama         │●Owner │ 08xx     │●Aktif  │✏️ ⋮    │
│ 👤  │ Nama         │●Dokter│ 08xx     │●Aktif  │✏️ ⋮    │
│ 👤  │ Nama         │●Staff │ 08xx     │●Nonaktif│✏️ ⋮   │
└──────┴──────────────┴───────┴──────────┴────────┴─────────┘

MODAL TAMBAH/EDIT USER:
Tab: [Info Dasar] [Profil Dokter] ← tab kedua muncul jika role=dokter

Info Dasar:
- Email*, Password* (hanya saat tambah), Nama Lengkap*
- Role* (dropdown), No HP, Alamat
- Foto Profil (upload)

Profil Dokter (jika role=dokter):
- No STR, Spesialisasi
- Bio (textarea)
- Jadwal Praktik (dynamic: +Tambah Hari)
  [ Senin ▼ ] [ 08:00 ] [ 16:00 ] [🗑]
  [ Rabu  ▼ ] [ 08:00 ] [ 12:00 ] [🗑]
  [+ Tambah Hari]

Dropdown ⋮ per row:
- Edit
- Nonaktifkan / Aktifkan
```

---

### 4. MANAJEMEN HEWAN
```
PageHeader: "Data Hewan" | [+ Tambah Hewan]

FILTER: [ Search nama hewan/pemilik ] [ Spesies ▼ ] [ Status ▼ ]

CARD GRID (3 kolom desktop, 1 mobile):
┌───────────────────────┐
│ [FOTO HEWAN 80x80]    │
│ Nama Hewan            │
│ Spesies • Ras         │
│ Pemilik: Nama Owner   │
│ ● Aktif               │
│ [Lihat Detail] [Edit] │
└───────────────────────┘

HALAMAN DETAIL HEWAN:
Tab: [Info] [Rekam Medis] [Vaksin] [Riwayat Kunjungan]

Info:
┌─────────────────────────────────────────────────┐
│ [FOTO]  Nama Hewan                 ● Aktif      │
│         Kucing • Persia • Jantan                │
│         Lahir: 12 Jan 2021 (3 tahun)            │
│         Berat: 4.2 kg • Warna: Oranye           │
│         Pemilik: Nama Owner • 08xx              │
└─────────────────────────────────────────────────┘

Rekam Medis: list card per kunjungan
Vaksin: tabel + tombol tambah vaksin
Riwayat Kunjungan: timeline appointment

MODAL TAMBAH/EDIT HEWAN:
- Foto (ImageUpload)
- Nama*, Spesies* (dropdown: Kucing/Anjing/Kelinci/Burung/Lainnya)
- Ras, Jenis Kelamin* (radio), Tanggal Lahir
- Berat (kg), Warna, Ciri Khas
- Pemilik* (search customer — hanya staff/owner yang bisa pilih)
```

---

### 5. APPOINTMENT
```
PageHeader: "Appointment" | [+ Buat Appointment]

VIEW TOGGLE: [📋 List] [📅 Kalender]

LIST VIEW:
FILTER: [ Tanggal ] [ Dokter ▼ ] [ Status ▼ ] [ Search ]

TABLE:
┌──────────┬──────────┬────────┬─────────┬────────┬──────────┐
│Tgl & Jam │Hewan     │Pemilik │Dokter   │Jenis   │Status    │
├──────────┼──────────┼────────┼─────────┼────────┼──────────┤
│17 Jun    │ Milo     │ Andi   │Dr.Budi  │Konsult │●Konfirm  │
│08:00     │ Kucing   │        │         │        │          │
└──────────┴──────────┴────────┴─────────┴────────┴──────────┘
Klik row → expand detail + tombol aksi

KALENDER VIEW:
Grid 7 hari, tiap cell tampilkan appointment sebagai chip warna per status
Klik chip → popup detail appointment

MODAL BUAT APPOINTMENT:
- Cari Hewan* (search autocomplete → tampilkan nama + pemilik)
- Dokter (dropdown, opsional)
- Tanggal & Jam*
- Jenis* (dropdown)
- Keluhan (textarea)
- Catatan Staff (textarea)
- Sumber: Langsung / Online

AKSI PER APPOINTMENT:
Status pending   → [Konfirmasi] [Batal]
Status konfirmasi → [Selesai] [Buat Rekam Medis] [Rawat Inap] [Batal]
Status selesai   → [Lihat Rekam Medis]
```

---

### 6. REKAM MEDIS
```
PageHeader: "Rekam Medis" | [+ Input Rekam Medis]

FILTER: [ Search hewan/pemilik ] [ Dokter ▼ ] [ Tanggal Range ]

LIST — Card per rekam medis:
┌──────────────────────────────────────────────────┐
│ 🐾 Nama Hewan (Spesies)    📅 17 Jun 2024        │
│ 👤 Pemilik: Nama           👨‍⚕️ Dr. Nama           │
│ Diagnosis: [teks diagnosis]                      │
│ [Lihat Detail] [Print] [Edit]                   │
└──────────────────────────────────────────────────┘

HALAMAN DETAIL REKAM MEDIS (printable):
┌──────────────────────────────────────────────────┐
│ REKAM MEDIS KLINIK HEWAN [NAMA KLINIK]          │
│ ──────────────────────────────────────          │
│ No: RM-20240617-001  Tanggal: 17 Jun 2024       │
│ Dokter: Dr. Nama     No STR: xxxxx              │
├──────────────────────────────────────────────────┤
│ IDENTITAS HEWAN                                 │
│ Nama: Milo  Spesies: Kucing  Ras: Persia        │
│ Pemilik: Nama  No HP: 08xx                      │
├──────────────────────────────────────────────────┤
│ PEMERIKSAAN                                     │
│ Berat: 4.2kg  Suhu: 38.5°C                     │
│ Keluhan: [teks]                                 │
│ Anamnesis: [teks]                               │
├──────────────────────────────────────────────────┤
│ DIAGNOSIS & TINDAKAN                            │
│ Diagnosis: [teks]                               │
│ Tindakan: [teks]                                │
├──────────────────────────────────────────────────┤
│ RESEP                                           │
│ 1. Nama Obat — 1 tab/8 jam/5 hari — 15 tab     │
│ 2. Nama Obat — 2 tab/12 jam/3 hari — 6 tab     │
├──────────────────────────────────────────────────┤
│ Catatan Followup: [teks]                        │
│           [Tanda Tangan Dokter]                 │
└──────────────────────────────────────────────────┘

MODAL INPUT REKAM MEDIS:
Section 1 — Pasien:
- Pilih dari Appointment (dropdown appointment hari ini) ATAU
- Cari Hewan langsung (search)

Section 2 — Pemeriksaan:
- Berat (kg), Suhu (°C) — side by side
- Keluhan (textarea), Anamnesis (textarea)

Section 3 — Diagnosis:
- Diagnosis* (textarea)
- Tindakan (textarea)

Section 4 — Resep (dynamic rows):
┌───────────────┬──────┬─────────┬───────┬─────┬────┐
│ Nama Obat     │ Dosis│ Frekuensi│ Durasi│ Qty │ 🗑 │
└───────────────┴──────┴─────────┴───────┴─────┴────┘
[+ Tambah Obat]

Section 5:
- Catatan Followup (textarea)
- Tampilkan ke Customer (toggle)
```

---

### 7. RAWAT INAP
```
PageHeader: "Rawat Inap" | [+ Admisi Pasien]

STATUS TABS: [Aktif (N)] [Sembuh] [Semua]

CARD GRID — Pasien Aktif (2 kolom):
┌──────────────────────────────┐
│ Kandang 01                   │
│ [foto] Nama Hewan (Spesies)  │
│ Pemilik: Nama                │
│ Masuk: 15 Jun 2024 (2 hari)  │
│ Dokter: Dr. Nama             │
│ Kondisi Terakhir: ● Stabil   │
│ Update: 2 jam lalu           │
│ [Lihat Detail] [+ Log]       │
└──────────────────────────────┘

HALAMAN DETAIL RAWAT INAP:
┌──────────────────────────────────────────────────┐
│ INFO PASIEN                    [Pulangkan] [Edit]│
│ Milo (Kucing Persia) — Kandang 01               │
│ Masuk: 15 Jun 2024 | Dokter: Dr. Nama           │
│ Diagnosis: [teks]                               │
├──────────────────────────────────────────────────┤
│ [+ Tambah Log Monitoring]                       │
├──────────────────────────────────────────────────┤
│ TIMELINE MONITORING                             │
│                                                 │
│ ● 17 Jun 2024 — 08:30 — oleh: Staff Nama       │
│   Kondisi: ● Stabil                             │
│   Berat: 4.1kg | Suhu: 38.4°C                  │
│   Nafsu Makan: Normal                           │
│   Catatan: [teks kondisi]                       │
│   Tindakan: [teks]                              │
│   [foto] [foto] [foto]                          │
│                                                 │
│ ● 16 Jun 2024 — 09:00 — oleh: Dr. Nama         │
│   Kondisi: ● Lemah                              │
│   ...                                           │
└──────────────────────────────────────────────────┘

MODAL TAMBAH LOG MONITORING:
- Kondisi* (dropdown dengan warna)
- Berat (kg), Suhu (°C) — side by side
- Nafsu Makan* (dropdown)
- Catatan Kondisi* (textarea)
- Tindakan Hari Ini (textarea)
- Obat Hari Ini (textarea)
- Upload Foto (multiple, max 5, preview grid)
- Tampilkan ke Customer (toggle)

MODAL PULANGKAN PASIEN:
- Status Akhir* (dropdown: sembuh/dirujuk/meninggal)
- Tanggal Keluar* (date, default hari ini)
- Catatan Penutup (textarea)
```

---

### 8. POS (POINT OF SALE)
```
Layout: 2 kolom fixed, full height, no scroll page

┌──────────────────────────────┬─────────────────────────┐
│ KIRI — KATALOG (60%)         │ KANAN — TRANSAKSI (40%) │
│                              │                         │
│ [Search produk/layanan... 🔍]│ KERANJANG               │
│ [Semua][Produk][Layanan]     │ ─────────────────────   │
│                              │ (kosong — EmptyState)   │
│ GRID PRODUK (3 kolom):       │                         │
│ ┌──────┐┌──────┐┌──────┐    │ ┌────────────────────┐  │
│ │[foto]││[foto]││[foto]│    │ │Nama Item      x    │  │
│ │Nama  ││Nama  ││Nama  │    │ │Rp harga  [-][1][+] │  │
│ │Rp xxx││Rp xxx││Rp xxx│    │ │Subtotal: Rp xxx    │  │
│ │Stok:N││Stok:N││Stok:N│    │ └────────────────────┘  │
│ └──────┘└──────┘└──────┘    │                         │
│                              │ ─────────────────────   │
│ (klik produk → masuk cart)   │ Customer:               │
│                              │ [Search/Tamu...]        │
│ Produk stok 0 → disabled     │                         │
│ Stok < 5 → badge "Sisa N"    │ Subtotal    Rp xxx      │
│                              │ Diskon      [____] %/Rp │
│                              │ Total       Rp xxx      │
│                              │                         │
│                              │ Bayar Via:              │
│                              │ [Tunai][Transfer]       │
│                              │ [QRIS][Debit]           │
│                              │                         │
│                              │ (jika Tunai):           │
│                              │ Diterima  [________]    │
│                              │ Kembali   Rp xxx        │
│                              │                         │
│                              │ Catatan (opsional)      │
│                              │ [________________]      │
│                              │                         │
│                              │ [    PROSES BAYAR    ]  │
└──────────────────────────────┴─────────────────────────┘

MODAL STRUK (setelah bayar berhasil):
┌─────────────────────────────────┐
│         [LOGO KLINIK]           │
│       NAMA KLINIK HEWAN         │
│  Alamat | No HP                 │
│ ─────────────────────────────── │
│ No  : INV-20240617-0001         │
│ Tgl : 17 Jun 2024, 14:30        │
│ Kasir: Nama Staff               │
│ ─────────────────────────────── │
│ Nama Produk    2 × Rp5.000      │
│                      Rp10.000   │
│ Nama Layanan   1 × Rp50.000     │
│                      Rp50.000   │
│ ─────────────────────────────── │
│ Subtotal          Rp  60.000    │
│ Diskon            Rp   5.000    │
│ Total             Rp  55.000    │
│ Tunai             Rp  60.000    │
│ Kembali           Rp   5.000    │
│ ─────────────────────────────── │
│        Terima kasih!            │
│   [🖨 Print] [Transaksi Baru]   │
└─────────────────────────────────┘
```

---

### 9. INVENTORY
```
PageHeader: "Inventory" | [+ Tambah Produk]

TABS: [Produk] [Layanan] [Kategori] [Mutasi Stok]

TAB PRODUK:
FILTER: [Search] [Kategori ▼] [⚠ Stok Menipis]

TABLE:
┌────┬──────────────┬──────────┬─────────┬───────┬────────┬──────┐
│Foto│Nama + Kode   │Kategori  │Harga    │Stok   │Min Stok│Aksi  │
├────┼──────────────┼──────────┼─────────┼───────┼────────┼──────┤
│[f] │Nama          │Obat      │Rp xx    │ 50    │ 10     │⋮     │
│[f] │Nama          │Makanan   │Rp xx    │⚠ 3   │ 10     │⋮     │
└────┴──────────────┴──────────┴─────────┴───────┴────────┴──────┘
Baris stok < minimum → bg-red-50, stok merah bold

Dropdown ⋮:
- Edit Produk
- Tambah Stok Masuk
- Adjustment Stok
- Riwayat Mutasi
- Nonaktifkan

MODAL TAMBAH STOK MASUK:
- Produk: [nama produk] (readonly)
- Stok Saat Ini: N (readonly)
- Jumlah Masuk* (number)
- Stok Setelah: N+jumlah (auto hitung)
- Catatan (textarea)

MODAL ADJUSTMENT STOK:
- Stok Saat Ini: N (readonly)
- Stok Seharusnya* (number)
- Selisih: ±N (auto hitung, warna merah/hijau)
- Alasan* (textarea)

TAB MUTASI STOK:
TABLE:
┌──────────┬─────────┬───────┬──────┬──────┬────────┬───────────┐
│Tanggal   │Produk   │Tipe   │Sblm  │Ubah  │Sesudah │Referensi  │
├──────────┼─────────┼───────┼──────┼──────┼────────┼───────────┤
│17 Jun    │Nama     │●Masuk │ 10   │+20   │ 30     │Manual     │
│17 Jun    │Nama     │●Keluar│ 30   │-2    │ 28     │INV-xxx    │
└──────────┴─────────┴───────┴──────┴──────┴────────┴───────────┘

TAB LAYANAN:
TABLE dengan CRUD: Nama, Kategori, Harga, Durasi, Dokter Required, Status
```

---

### 10. LAPORAN (OWNER)
```
PageHeader: "Laporan Keuangan"

FILTER PERIODE:
[Hari Ini] [Minggu Ini] [Bulan Ini] [Tahun Ini] [Custom ▼]
Custom: [dari: ____] [sampai: ____] [Terapkan]

ROW 1 — KARTU RINGKASAN:
┌────────────┬────────────┬────────────┬────────────┐
│💰 Pemasukan│💸Pengeluaran│📈 Laba     │🧾Transaksi │
│ Rp xxx.xxx │ Rp xxx.xxx │ Rp xxx.xxx │ NN total   │
└────────────┴────────────┴────────────┴────────────┘

ROW 2 — CHART:
┌──────────────────────────┬─────────────────────────┐
│ LINE CHART               │ PIE CHART               │
│ Pemasukan vs Pengeluaran │ Metode Pembayaran        │
│ (per hari/minggu/bulan)  │ Tunai NN% | QRIS NN%    │
└──────────────────────────┴─────────────────────────┘

ROW 3 — CHART:
┌──────────────────────────┬─────────────────────────┐
│ BAR CHART                │ BAR CHART               │
│ Produk Terlaris (Top 10) │ Pemasukan per Kategori  │
│                          │ Produk vs Layanan        │
└──────────────────────────┴─────────────────────────┘

ROW 4 — TABEL TRANSAKSI:
PageHeader mini: "Detail Transaksi" | [📥 Export CSV]
TABLE:
┌──────────┬──────────────┬──────────┬──────────┬────────┬──────┐
│No Inv    │Customer      │Kasir     │Total     │Metode  │Status│
└──────────┴──────────────┴──────────┴──────────┴────────┴──────┘

ROW 5 — 2 KOLOM:
┌──────────────────────────┬─────────────────────────┐
│ ⚠ STOK MENIPIS           │ 👨‍⚕️ PERFORMA DOKTER      │
│ Produk | Stok | Min      │ Dokter | Pasien | Bulan  │
└──────────────────────────┴─────────────────────────┘
```

---

### 11. BOOKING ONLINE — PUBLIC
```
LANDING PAGE /:
┌─────────────────────────────────────────────────┐
│ NAVBAR: Logo | Layanan | Dokter | [Booking] [Login]│
├─────────────────────────────────────────────────┤
│ HERO SECTION                                    │
│ "Kesehatan Hewan Peliharaan Anda,               │
│  Prioritas Kami"                                │
│ [Booking Sekarang] [Lihat Layanan]              │
│ [ilustrasi dokter hewan]                        │
├─────────────────────────────────────────────────┤
│ LAYANAN UNGGULAN (grid 3)                       │
│ 🏥 Konsultasi | 🛁 Grooming | 🏨 Rawat Inap     │
├─────────────────────────────────────────────────┤
│ TIM DOKTER (card grid)                          │
│ [Foto] Dr. Nama | Spesialisasi | Bio singkat    │
├─────────────────────────────────────────────────┤
│ INFO KLINIK                                     │
│ Jam buka | Alamat | Kontak | Maps embed         │
├─────────────────────────────────────────────────┤
│ FOOTER                                          │
└─────────────────────────────────────────────────┘

HALAMAN BOOKING /booking:
Step indicator: [1. Pilih Jadwal] → [2. Data Hewan] → [3. Konfirmasi]

STEP 1 — Pilih Jadwal:
- Pilih Dokter (dropdown, opsional)
- Pilih Tanggal (date picker, hanya hari yang ada slot)
- Grid slot tersedia:
  ┌──────────┐ ┌──────────┐ ┌──────────┐
  │ 08:00    │ │ 09:00    │ │ 10:00    │
  │ Tersedia │ │ Penuh    │ │ Tersedia │
  └──────────┘ └──────────┘ └──────────┘
  Slot penuh → disabled, warna abu

STEP 2 — Data Hewan:
(Jika customer login → nama & HP terisi otomatis)
- Nama Pemilik*, No HP*, Email
- Nama Hewan*, Spesies*
- Keluhan (textarea)

STEP 3 — Konfirmasi:
- Ringkasan: dokter, tanggal, jam, nama hewan, keluhan
- [Kirim Booking]
- Setelah submit: success screen
  "Booking terkirim! Kami akan menghubungi Anda untuk konfirmasi."

HALAMAN STAFF — MANAJEMEN BOOKING:
TABLE dengan filter status:
┌──────────┬─────────┬──────────┬─────────┬──────────┬────────┐
│Tgl Booking│Nama    │Hewan     │Slot     │Status    │Aksi    │
├──────────┼─────────┼──────────┼─────────┼──────────┼────────┤
│17 Jun    │ Andi    │ Milo     │08:00    │●Menunggu │✅ ❌   │
└──────────┴─────────┴──────────┴─────────┴──────────┴────────┘
✅ → Modal konfirmasi (auto buat appointment)
❌ → Modal tolak (wajib isi alasan)
```

---

### 12. HALAMAN CUSTOMER — REKAM MEDIS & MONITORING
```
REKAM MEDIS CUSTOMER /customer/rekam-medis:
- Filter: Pilih Hewan (tab per hewan yang dimiliki)
- Card per rekam medis (hanya yang is_visible_customer=true):
  Tanggal | Dokter | Diagnosis | [Lihat Detail]
- Detail: sama dengan halaman dokter tapi read-only, tanpa resep detail dosis

MONITORING CUSTOMER /customer/monitoring:
- Hanya tampil jika ada hewan yang sedang rawat inap
- Card hewan rawat inap:
  Nama | Kandang | Masuk sejak
  Kondisi terakhir: ● Stabil (update 2 jam lalu)
- Timeline log (hanya is_visible_customer=true):
  Kondisi, Catatan, Foto grid
- Foto bisa di-tap → lightbox fullscreen
```

---

## KOMPONEN INTERAKSI KRITIS

### Toast Notification
```
Posisi    : top-right
Sukses    : bg-emerald-500, icon ✓, auto dismiss 3 detik
Error     : bg-red-500, icon ✗, auto dismiss 5 detik
Warning   : bg-amber-500, icon ⚠, auto dismiss 4 detik
Info      : bg-sky-500, icon ℹ, auto dismiss 3 detik
```

### Loading States
```
Button submit    : spinner kiri + teks "Menyimpan..."
Tabel data       : skeleton rows (5 baris placeholder)
Card dashboard   : skeleton card
Halaman penuh    : center spinner dengan overlay
Upload foto      : progress bar + persentase
```

### Empty States
```
Tiap tabel/list kosong tampilkan:
[ilustrasi sederhana]
"Belum ada [nama data]"
"[deskripsi singkat]"
[Tombol aksi primer jika relevan]
```

### Confirm Dialog
```
Semua aksi destruktif wajib confirm:
- Nonaktifkan user
- Batal transaksi
- Hapus pengeluaran
- Tolak booking
- Pulangkan pasien

Format:
Judul  : "Konfirmasi [Aksi]"
Body   : "Apakah Anda yakin ingin [aksi]? [konsekuensi]"
Tombol : [Batal] [Ya, [Aksi]] ← tombol aksi warna danger
```

---

## RESPONSIVE BREAKPOINT

```
Mobile  < 768px  : 1 kolom, sidebar overlay, tabel scroll horizontal
Tablet  768-1024 : 2 kolom grid, sidebar collapsible
Desktop > 1024px : layout penuh seperti wireframe di atas

POS page: minimum 1024px, tidak dioptimasi untuk mobile
Print   : CSS khusus @media print untuk struk dan rekam medis
```

---

## PRINT CSS — STRUK & REKAM MEDIS

```css
@media print {
  sidebar, topbar, button, filter → display: none
  content → width: 100%, padding: 0
  struk → max-width: 80mm (thermal printer)
  rekam medis → A4, font 12px, semua border hitam
  page-break → rekam medis satu halaman jika memungkinkan
}
```
