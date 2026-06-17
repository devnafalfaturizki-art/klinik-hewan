# BLUEPRINT PRODUK — PLATFORM KLINIK HEWAN

---

## IDENTITAS PRODUK

```
Nama Produk  : VetCare Platform
Tipe         : Web Application Fullstack
Stack        : Next.js 14 + TypeScript + Supabase + TailwindCSS + shadcn/ui
Target User  : Klinik hewan skala kecil-menengah
Role         : owner | dokter | staff | customer
```

---

## ARSITEKTUR SISTEM

```
src/
├── app/
│   ├── (auth)/login/
│   ├── (public)/
│   │   ├── page.tsx                    # Landing page
│   │   ├── layanan/page.tsx
│   │   ├── dokter/page.tsx
│   │   └── booking/page.tsx
│   ├── (dashboard)/
│   │   ├── layout.tsx
│   │   ├── sidebar.tsx
│   │   ├── owner/
│   │   │   ├── dashboard/page.tsx
│   │   │   ├── users/page.tsx
│   │   │   ├── laporan/page.tsx
│   │   │   ├── booking/slots/page.tsx
│   │   │   └── settings/page.tsx
│   │   ├── dokter/
│   │   │   ├── dashboard/page.tsx
│   │   │   ├── appointment/page.tsx
│   │   │   ├── rekam-medis/page.tsx
│   │   │   └── rawat-inap/page.tsx
│   │   ├── staff/
│   │   │   ├── dashboard/page.tsx
│   │   │   ├── pos/page.tsx
│   │   │   ├── inventory/page.tsx
│   │   │   ├── rawat-inap/page.tsx
│   │   │   ├── appointment/page.tsx
│   │   │   ├── pengeluaran/page.tsx
│   │   │   └── booking/page.tsx
│   │   └── customer/
│   │       ├── dashboard/page.tsx
│   │       ├── hewan/page.tsx
│   │       ├── rekam-medis/page.tsx
│   │       └── monitoring/page.tsx
│   └── api/
│       ├── auth/
│       ├── users/
│       ├── pets/
│       ├── appointments/
│       ├── medical-records/
│       ├── inpatients/
│       ├── pos/
│       ├── inventory/
│       ├── expenses/
│       ├── reports/
│       ├── booking/
│       └── settings/
├── components/
│   ├── ui/                             # Base components
│   ├── layout/                         # Sidebar, TopBar
│   ├── modules/                        # Per modul
│   │   ├── pos/
│   │   ├── klinik/
│   │   ├── inventory/
│   │   └── laporan/
│   └── shared/                         # Dipakai lintas modul
├── lib/
│   ├── supabase/client.ts
│   ├── supabase/server.ts
│   ├── auth.ts
│   ├── validations/                    # Semua Zod schema
│   ├── constants.ts
│   └── utils/
│       ├── invoice.ts
│       ├── report.ts
│       └── upload.ts
└── middleware.ts
```

---

## DATABASE SCHEMA

### TABLE: profiles
```sql
id              uuid PRIMARY KEY REFERENCES auth.users
role            text CHECK (role IN ('owner','dokter','staff','customer'))
nama_lengkap    text NOT NULL
no_hp           text
alamat          text
foto_profil     text
is_active       boolean DEFAULT true
created_at      timestamptz DEFAULT now()
```

### TABLE: dokter_profiles
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
user_id         uuid REFERENCES profiles NOT NULL
no_str          text
spesialisasi    text
bio             text
jadwal_praktik  jsonb
-- jadwal_praktik: [{ hari: "Senin", jam_mulai: "08:00", jam_selesai: "16:00" }]
```

### TABLE: pets
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
owner_id        uuid REFERENCES profiles NOT NULL
nama            text NOT NULL
spesies         text NOT NULL
ras             text
jenis_kelamin   text CHECK (jenis_kelamin IN ('jantan','betina'))
tgl_lahir       date
berat_kg        numeric(5,2)
warna           text
ciri_khas       text
foto            text
status          text DEFAULT 'aktif' CHECK (status IN ('aktif','meninggal'))
created_at      timestamptz DEFAULT now()
```

### TABLE: pet_vaccines
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
pet_id          uuid REFERENCES pets NOT NULL
nama_vaksin     text NOT NULL
tgl_vaksin      date NOT NULL
tgl_berikutnya  date
dokter_id       uuid REFERENCES profiles
keterangan      text
created_at      timestamptz DEFAULT now()
```

### TABLE: categories
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
nama            text NOT NULL
tipe            text CHECK (tipe IN ('produk','layanan'))
```

### TABLE: products
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
category_id     uuid REFERENCES categories
kode_produk     text UNIQUE NOT NULL
nama            text NOT NULL
deskripsi       text
foto            text
harga_beli      numeric(12,2) DEFAULT 0
harga_jual      numeric(12,2) NOT NULL
stok            integer DEFAULT 0
stok_minimum    integer DEFAULT 5
satuan          text DEFAULT 'pcs'
is_active       boolean DEFAULT true
created_at      timestamptz DEFAULT now()
```

### TABLE: services
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
category_id     uuid REFERENCES categories
nama            text NOT NULL
deskripsi       text
harga           numeric(12,2) NOT NULL
durasi_menit    integer
dokter_required boolean DEFAULT false
is_active       boolean DEFAULT true
```

### TABLE: appointments
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
pet_id          uuid REFERENCES pets NOT NULL
dokter_id       uuid REFERENCES profiles
customer_id     uuid REFERENCES profiles NOT NULL
tgl_janji       timestamptz NOT NULL
jenis           text CHECK (jenis IN ('konsultasi','grooming','operasi','vaksin','lainnya'))
status          text DEFAULT 'pending' CHECK (status IN ('pending','konfirmasi','selesai','batal'))
keluhan         text
catatan_staff   text
sumber          text DEFAULT 'langsung' CHECK (sumber IN ('langsung','online'))
created_at      timestamptz DEFAULT now()
```

### TABLE: medical_records
```sql
id                  uuid PRIMARY KEY DEFAULT gen_random_uuid()
pet_id              uuid REFERENCES pets NOT NULL
dokter_id           uuid REFERENCES profiles NOT NULL
appointment_id      uuid REFERENCES appointments
tanggal             timestamptz DEFAULT now()
berat_saat_periksa  numeric(5,2)
suhu                numeric(4,1)
keluhan             text
anamnesis           text
diagnosis           text NOT NULL
tindakan            text
resep               jsonb
-- resep: [{ nama_obat, dosis, frekuensi, durasi, qty, aturan_pakai }]
catatan_followup    text
is_visible_customer boolean DEFAULT true
created_at          timestamptz DEFAULT now()
```

### TABLE: inpatients
```sql
id                  uuid PRIMARY KEY DEFAULT gen_random_uuid()
pet_id              uuid REFERENCES pets NOT NULL
dokter_id           uuid REFERENCES profiles NOT NULL
medical_record_id   uuid REFERENCES medical_records
no_kandang          text NOT NULL
tgl_masuk           timestamptz DEFAULT now()
tgl_keluar          timestamptz
diagnosis_awal      text
tindakan_awal       text
status              text DEFAULT 'aktif' CHECK (status IN ('aktif','sembuh','dirujuk','meninggal'))
created_at          timestamptz DEFAULT now()
```

### TABLE: inpatient_logs
```sql
id                  uuid PRIMARY KEY DEFAULT gen_random_uuid()
inpatient_id        uuid REFERENCES inpatients NOT NULL
staff_id            uuid REFERENCES profiles NOT NULL
timestamp           timestamptz DEFAULT now()
kondisi             text CHECK (kondisi IN ('kritis','lemah','stabil','baik','sangat_baik'))
berat               numeric(5,2)
suhu                numeric(4,1)
nafsu_makan         text CHECK (nafsu_makan IN ('tidak_mau','sedikit','normal','lahap'))
catatan_kondisi     text
tindakan_hari_ini   text
obat_hari_ini       text
foto_urls           jsonb DEFAULT '[]'
-- foto_urls: ["https://...supabase.../clinic-uploads/..."]
is_visible_customer boolean DEFAULT true
```

### TABLE: transactions
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
no_transaksi    text UNIQUE NOT NULL
-- format: INV-YYYYMMDD-XXXX (auto generate via trigger)
customer_id     uuid REFERENCES profiles
kasir_id        uuid REFERENCES profiles NOT NULL
tgl_transaksi   timestamptz DEFAULT now()
subtotal        numeric(12,2) NOT NULL
diskon_nominal  numeric(12,2) DEFAULT 0
total           numeric(12,2) NOT NULL
metode_bayar    text CHECK (metode_bayar IN ('tunai','transfer','qris','debit'))
uang_diterima   numeric(12,2)
kembalian       numeric(12,2) DEFAULT 0
status          text DEFAULT 'lunas' CHECK (status IN ('draft','lunas','batal'))
catatan         text
appointment_id  uuid REFERENCES appointments
created_at      timestamptz DEFAULT now()
```

### TABLE: transaction_items
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
transaction_id  uuid REFERENCES transactions NOT NULL
tipe_item       text CHECK (tipe_item IN ('produk','layanan'))
product_id      uuid REFERENCES products
service_id      uuid REFERENCES services
nama_item       text NOT NULL
harga_satuan    numeric(12,2) NOT NULL
qty             integer NOT NULL DEFAULT 1
diskon          numeric(12,2) DEFAULT 0
subtotal        numeric(12,2) NOT NULL
```

### TABLE: stock_mutations
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
product_id      uuid REFERENCES products NOT NULL
tipe            text CHECK (tipe IN ('masuk','keluar','adjustment'))
qty_sebelum     integer NOT NULL
qty_perubahan   integer NOT NULL
qty_sesudah     integer NOT NULL
referensi       text
staff_id        uuid REFERENCES profiles NOT NULL
catatan         text
created_at      timestamptz DEFAULT now()
```

### TABLE: expenses
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
kategori        text NOT NULL
deskripsi       text NOT NULL
jumlah          numeric(12,2) NOT NULL
tgl_pengeluaran date NOT NULL
bukti_foto      text
staff_id        uuid REFERENCES profiles NOT NULL
created_at      timestamptz DEFAULT now()
```

### TABLE: booking_slots
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
dokter_id       uuid REFERENCES profiles NOT NULL
tanggal         date NOT NULL
jam_mulai       time NOT NULL
jam_selesai     time NOT NULL
kuota           integer DEFAULT 1
terisi          integer DEFAULT 0
is_available    boolean DEFAULT true
```

### TABLE: online_bookings
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
slot_id         uuid REFERENCES booking_slots NOT NULL
customer_id     uuid REFERENCES profiles
nama_guest      text
no_hp_guest     text
nama_hewan      text NOT NULL
spesies         text NOT NULL
keluhan         text
status          text DEFAULT 'menunggu' CHECK (status IN ('menunggu','dikonfirmasi','ditolak','selesai'))
catatan_klinik  text
alasan_tolak    text
created_at      timestamptz DEFAULT now()
```

### TABLE: clinic_info
```sql
id              uuid PRIMARY KEY DEFAULT gen_random_uuid()
nama_klinik     text NOT NULL
logo            text
alamat          text
no_hp           text
email           text
jam_buka        time
jam_tutup       time
hari_buka       jsonb
-- hari_buka: ["Senin","Selasa","Rabu","Kamis","Jumat","Sabtu"]
sosmed          jsonb
-- sosmed: { instagram, facebook, whatsapp }
footer_struk    text
```

---

## RLS POLICY

```
profiles
├── SELECT: user bisa baca profil sendiri
├── SELECT: owner/dokter/staff bisa baca semua profil
└── UPDATE: user hanya bisa update profil sendiri, owner update semua

pets
├── SELECT: customer hanya lihat WHERE owner_id = auth.uid()
├── SELECT: owner/dokter/staff lihat semua
├── INSERT: customer bisa tambah hewan sendiri, staff/owner/dokter juga bisa
└── UPDATE: customer update hewan sendiri, staff/owner/dokter update semua

medical_records
├── SELECT customer: WHERE pet_id IN (pets milik customer) AND is_visible_customer = true
├── SELECT dokter/staff/owner: semua
└── INSERT/UPDATE: hanya dokter dan owner

inpatient_logs
├── SELECT customer: WHERE is_visible_customer = true AND inpatient.pet_id IN (pets milik customer)
├── SELECT dokter/staff/owner: semua
└── INSERT: dokter dan staff dan owner

transactions
├── SELECT customer: WHERE customer_id = auth.uid()
├── SELECT staff/owner: semua
└── INSERT: hanya staff dan owner

stock_mutations
├── SELECT/INSERT: hanya staff dan owner

expenses
├── SELECT/INSERT/UPDATE: hanya staff dan owner
├── DELETE: hanya owner

booking_slots
├── SELECT: public (semua bisa lihat)
└── INSERT/UPDATE/DELETE: hanya owner

online_bookings
├── SELECT: customer lihat milik sendiri, staff/owner lihat semua
└── INSERT: public (guest dan customer bisa booking)
└── UPDATE: hanya staff dan owner

clinic_info
├── SELECT: public
└── UPDATE: hanya owner
```

---

## API ROUTES CONTRACT

### AUTH
```
POST /api/auth/login        body: { email, password }
POST /api/auth/logout
GET  /api/auth/me           return: { user, profile, role }
```

### USERS
```
GET    /api/users                   query: ?role=&search=&page=
POST   /api/users                   body: { email, password, role, nama_lengkap, no_hp, ...dokter_profile? }
GET    /api/users/[id]
PATCH  /api/users/[id]              body: partial profile
DELETE /api/users/[id]              soft delete: is_active = false
```

### PETS
```
GET    /api/pets                    query: ?owner_id=&search=&page=
POST   /api/pets                    body: pet data + foto upload
GET    /api/pets/[id]               return: pet + vaccines + appointment history
PATCH  /api/pets/[id]
POST   /api/pets/[id]/vaccines      body: vaccine data
```

### APPOINTMENTS
```
GET    /api/appointments            query: ?dokter_id=&tanggal=&status=&page=
POST   /api/appointments            body: appointment data
PATCH  /api/appointments/[id]       body: { status, catatan_staff }
```

### MEDICAL RECORDS
```
GET    /api/medical-records         query: ?pet_id=&dokter_id=&page=
POST   /api/medical-records         body: record + resep array
GET    /api/medical-records/[id]
PATCH  /api/medical-records/[id]
```

### INPATIENTS
```
GET    /api/inpatients              query: ?status=aktif
POST   /api/inpatients              body: admisi data
GET    /api/inpatients/[id]
PATCH  /api/inpatients/[id]         body: { status, tgl_keluar }
GET    /api/inpatients/[id]/logs
POST   /api/inpatients/[id]/logs    body: log + foto upload (multipart)
```

### POS

```
GET    /api/pos/catalog             return: produk + layanan aktif + stok
POST   /api/pos/transactions        body: { customer_id?, items[], metode_bayar, diskon, uang_diterima }
                                    action: insert transaction + items + kurangi stok + insert mutations
GET    /api/pos/transactions/[id]   return: detail untuk struk
PATCH  /api/pos/transactions/[id]   body: { status: 'batal' } + rollback stok
```

### INVENTORY
```
GET    /api/inventory/products      query: ?search=&category_id=&low_stock=
POST   /api/inventory/products
PATCH  /api/inventory/products/[id]
POST   /api/inventory/products/[id]/stock   body: { tipe, qty, catatan }
GET    /api/inventory/mutations             query: ?product_id=&page=
GET    /api/inventory/services
POST   /api/inventory/services
PATCH  /api/inventory/services/[id]
GET    /api/inventory/categories
POST   /api/inventory/categories
PATCH  /api/inventory/categories/[id]
```

### EXPENSES
```
GET    /api/expenses                query: ?kategori=&from=&to=&page=
POST   /api/expenses
PATCH  /api/expenses/[id]
DELETE /api/expenses/[id]           hanya owner
```

### REPORTS
```
GET    /api/reports/summary         query: ?from=&to=
                                    return: { total_pemasukan, total_pengeluaran, laba_bersih, jumlah_transaksi, pasien_baru, rawat_inap_aktif }
GET    /api/reports/revenue         query: ?from=&to=&group_by=day|week|month
                                    return: [{ tanggal, pemasukan, pengeluaran }]
GET    /api/reports/breakdown       return: { produk, layanan } per periode
GET    /api/reports/top-products    query: ?from=&to=&limit=10
GET    /api/reports/payment-methods return: [{ metode, jumlah, total }]
GET    /api/reports/transactions    query: ?from=&to=&page= (untuk tabel + export CSV)
GET    /api/reports/stock-alert     return: produk dengan stok < minimum
GET    /api/reports/doctors         return: jumlah pasien per dokter per periode
```

### BOOKING
```
GET    /api/booking/slots           query: ?tanggal=&dokter_id= (public)
POST   /api/booking                 body: booking data (public)
GET    /api/booking/list            query: ?status=&page= (staff/owner)
PATCH  /api/booking/[id]            body: { status, catatan_klinik, alasan_tolak }
                                    jika dikonfirmasi: auto create appointment
POST   /api/booking/slots           body: slot data (owner)
DELETE /api/booking/slots/[id]      (owner)
```

### SETTINGS
```
GET    /api/settings/clinic
PATCH  /api/settings/clinic         body: clinic_info data + logo upload
```

---

## KOMPONEN UI WAJIB

```
src/components/ui/
├── Input.tsx           props: label, error, required, ...HTMLInput
├── Select.tsx          props: label, options, error, ...radix
├── Modal.tsx           props: open, onClose, title, children
├── Table.tsx           props: columns, data, pagination, search
├── Badge.tsx           props: status → warna otomatis per value
├── Card.tsx            props: title, value, icon, trend, color
├── PageHeader.tsx      props: title, subtitle, action
├── LoadingSpinner.tsx
├── EmptyState.tsx      props: title, description, action?
├── ConfirmDialog.tsx   props: open, onConfirm, title, description
├── ImageUpload.tsx     props: onUpload, preview, multiple?, maxSize=5MB
├── DateRangePicker.tsx props: from, to, onChange
└── PrintWrapper.tsx    props: children (handle print CSS)
```

---

## VALIDASI ZOD — SEMUA SCHEMA

```typescript
// src/lib/validations/

// user.ts
CreateUserSchema: { email, password, role, nama_lengkap, no_hp?, alamat? }
UpdateUserSchema: Partial<CreateUserSchema tanpa password>

// pet.ts
CreatePetSchema: { owner_id, nama, spesies, ras?, jenis_kelamin, tgl_lahir?, berat_kg? }

// appointment.ts
CreateAppointmentSchema: { pet_id, dokter_id?, tgl_janji, jenis, keluhan? }
UpdateAppointmentSchema: { status, catatan_staff? }

// medical-record.ts
CreateMedicalRecordSchema: {
  pet_id, dokter_id, appointment_id?,
  berat_saat_periksa?, suhu?, keluhan, diagnosis, tindakan?,
  resep: [{ nama_obat, dosis, frekuensi, durasi, qty, aturan_pakai }]
  catatan_followup?, is_visible_customer
}

// inpatient.ts
CreateInpatientSchema: { pet_id, dokter_id, no_kandang, diagnosis_awal, tindakan_awal? }
CreateInpatientLogSchema: {
  kondisi, berat?, suhu?, nafsu_makan?,
  catatan_kondisi, tindakan_hari_ini?, obat_hari_ini?,
  is_visible_customer
}

// pos.ts
CreateTransactionSchema: {
  customer_id?,
  items: [{ tipe_item, product_id?, service_id?, qty, diskon? }],
  metode_bayar, diskon_nominal?, uang_diterima?, catatan?
}

// inventory.ts
CreateProductSchema: { kode_produk, nama, category_id?, harga_beli, harga_jual, stok, stok_minimum, satuan }
StockMutationSchema: { tipe, qty, catatan? }

// expense.ts
CreateExpenseSchema: { kategori, deskripsi, jumlah, tgl_pengeluaran }

// booking.ts
CreateBookingSchema: { slot_id, nama_hewan, spesies, keluhan?, nama_guest?, no_hp_guest? }
```

---

## ROLE ACCESS MATRIX

```
HALAMAN/FITUR                      OWNER  DOKTER  STAFF  CUSTOMER
─────────────────────────────────────────────────────────────────
Dashboard ringkasan                  ✅     ✅      ✅      ✅
Laporan keuangan lengkap             ✅     ❌      ❌      ❌
Manajemen user                       ✅     ❌      ❌      ❌
Pengaturan klinik                    ✅     ❌      ❌      ❌
Kelola booking slots                 ✅     ❌      ❌      ❌

List semua appointment               ✅     ✅      ✅      ❌
Buat appointment                     ✅     ✅      ✅      ❌
Update status appointment            ✅     ✅      ✅      ❌

Input rekam medis                    ✅     ✅      ❌      ❌
Lihat semua rekam medis              ✅     ✅      ❌      ❌
Lihat rekam medis hewan sendiri      ✅     ✅      ❌      ✅

Admisi rawat inap                    ✅     ✅      ❌      ❌
Input log monitoring                 ✅     ✅      ✅      ❌
Lihat monitoring hewan sendiri       ✅     ✅      ✅      ✅
Update status rawat inap             ✅     ✅      ❌      ❌

Akses POS                            ✅     ❌      ✅      ❌
Batal transaksi                      ✅     ❌      ❌      ❌
Lihat riwayat transaksi sendiri      ✅     ❌      ✅      ✅

Kelola inventory                     ✅     ❌      ✅      ❌
Input pengeluaran                    ✅     ❌      ✅      ❌
Hapus pengeluaran                    ✅     ❌      ❌      ❌

Konfirmasi booking online            ✅     ❌      ✅      ❌
Lihat hewan sendiri                  ✅     ✅      ✅      ✅
Tambah hewan                         ✅     ✅      ✅      ✅
```

---

## BUSINESS LOGIC KRITIS

### POS — Proses Transaksi
```
1. Validasi semua item: produk aktif, stok cukup
2. Hitung subtotal per item: (harga_satuan × qty) - diskon item
3. Hitung total: subtotal semua item - diskon_nominal
4. Jika tunai: kembalian = uang_diterima - total (wajib >= 0)
5. Insert transaction (status: lunas)
6. Insert semua transaction_items (snapshot nama & harga)
7. Untuk setiap produk: UPDATE stok - qty
8. Insert stock_mutations tipe 'keluar' per produk
9. Semua langkah 5-8 dalam SATU database transaction (atomic)
10. Return: transaction_id untuk generate struk
```

### STRUK — Format Wajib
```
[NAMA KLINIK]
[ALAMAT] | [NO HP]
─────────────────────────
No    : INV-20240617-0001
Kasir : [nama staff]
Tgl   : 17 Jun 2024, 14:30
─────────────────────────
[nama item]      [qty]x[harga]
                     [subtotal]
─────────────────────────
Subtotal         : Rp xxx
Diskon           : Rp xxx
Total            : Rp xxx
Bayar ([metode]) : Rp xxx
Kembali          : Rp xxx
─────────────────────────
[footer_struk dari clinic_info]
```

### BOOKING — Flow Konfirmasi
```
1. Guest/customer submit booking → status: menunggu
2. slot.terisi++ jika terisi >= kuota → is_available = false
3. Staff konfirmasi → status: dikonfirmasi
4. Auto create appointment dari data booking
5. Staff tolak → status: ditolak, isi alasan_tolak
6. Slot yang ditolak: slot.terisi--
```

### MONITORING — Upload Foto
```
1. Validasi file: hanya jpg/png/webp, max 5MB per file, max 5 foto
2. Upload ke Supabase Storage bucket: clinic-uploads
3. Path: inpatient-logs/{inpatient_id}/{timestamp}-{filename}
4. Simpan array URL public ke foto_urls jsonb
5. URL bersifat permanen (public bucket)
```

### LAPORAN — Kalkulasi Laba
```
total_pemasukan  = SUM(transactions.total) WHERE status='lunas' AND periode
total_pengeluaran = SUM(expenses.jumlah) WHERE periode
laba_bersih      = total_pemasukan - total_pengeluaran
```

---

## ENVIRONMENT VARIABLES

```env
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=        # hanya di server, untuk admin actions
NEXT_PUBLIC_APP_URL=              # untuk redirect auth
```

---

## STANDAR KODE

```
TypeScript   : strict: true, no any, no implicit any
API Route    : wajib auth check → role check → zod validate → try/catch → return typed response
Server Comp  : default semua page, gunakan 'use client' hanya untuk interaktivitas
Error        : selalu return { error: string, status: number } yang konsisten
Success      : selalu return { data: T, message?: string }
Upload       : validasi tipe + ukuran sebelum upload, hapus file lama jika replace
Stok         : selalu atomic dengan transaksi, tidak boleh stok negatif
Harga        : selalu simpan snapshot nama & harga di transaction_items
Soft delete  : user tidak dihapus, hanya is_active = false
```

---

## TRIGGER SQL WAJIB

```sql
-- Auto create profile saat register
CREATE OR REPLACE FUNCTION handle_new_user()
RETURNS trigger AS $$
BEGIN
  INSERT INTO profiles (id, role, nama_lengkap)
  VALUES (NEW.id, NEW.raw_user_meta_data->>'role', NEW.raw_user_meta_data->>'nama_lengkap');
  RETURN NEW;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;

CREATE TRIGGER on_auth_user_created
  AFTER INSERT ON auth.users
  FOR EACH ROW EXECUTE FUNCTION handle_new_user();

-- Auto generate no_transaksi
CREATE OR REPLACE FUNCTION generate_no_transaksi()
RETURNS trigger AS $$
DECLARE
  urutan int;
BEGIN
  SELECT COUNT(*) + 1 INTO urutan
  FROM transactions
  WHERE DATE(tgl_transaksi) = CURRENT_DATE;
  NEW.no_transaksi := 'INV-' || TO_CHAR(NOW(), 'YYYYMMDD') || '-' || LPAD(urutan::text, 4, '0');
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER before_insert_transaction
  BEFORE INSERT ON transactions
  FOR EACH ROW EXECUTE FUNCTION generate_no_transaksi();
```

---

## SUPABASE STORAGE

```
Bucket name  : clinic-uploads
Access       : public
Policy       : authenticated user bisa upload
               public bisa read

Folder structure:
clinic-uploads/
├── pets/               {pet_id}-{timestamp}.jpg
├── inpatient-logs/     {inpatient_id}/{timestamp}-{n}.jpg
├── products/           {product_id}-{timestamp}.jpg
├── expenses/           {expense_id}-{timestamp}.jpg
├── profiles/           {user_id}-{timestamp}.jpg
└── clinic/             logo.jpg
```
