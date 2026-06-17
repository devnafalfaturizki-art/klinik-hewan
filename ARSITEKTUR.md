# ARSITEKTUR PLATFORM KLINIK HEWAN — SUMBER KEBENARAN MUTLAK

---

## IDENTITAS PLATFORM

```
Nama         : VetCare Platform
Versi        : 1.0.0
Stack        : Next.js 14 + TypeScript + Supabase + TailwindCSS + shadcn/ui
Runtime      : Node.js 20 LTS
Package Mgr  : npm
Repo         : GitHub (monorepo)
Deploy FE    : Vercel
Deploy DB    : Supabase Cloud
Environment  : development | production
```

---

## PRINSIP ARSITEKTUR

```
1. Server First       → semua komponen server component by default
2. Type Safe          → TypeScript strict, tidak ada 'any'
3. Atomic Transaction → operasi multi-tabel dalam satu DB transaction
4. Role Based Access  → setiap layer wajib cek role (middleware + API + RLS)
5. Fail Fast          → validasi input di edge sebelum masuk business logic
6. Single Source      → satu schema Zod = satu validasi untuk FE dan BE
7. Snapshot Harga     → harga item selalu disalin ke transaction_items
8. Soft Delete        → data tidak pernah dihapus fisik kecuali expenses
9. Audit Trail        → semua mutasi stok dan transaksi tercatat permanen
10. Mobile First      → UI responsive, POS minimal 1024px
```

---

## STRUKTUR DIREKTORI LENGKAP

```
vetcare-platform/
├── .devcontainer/
│   └── devcontainer.json
├── .github/
│   └── workflows/
│       └── ci.yml
├── public/
│   ├── favicon.ico
│   └── logo.png
├── src/
│   ├── app/
│   │   ├── (auth)/
│   │   │   ├── layout.tsx
│   │   │   └── login/
│   │   │       └── page.tsx
│   │   ├── (public)/
│   │   │   ├── layout.tsx
│   │   │   ├── page.tsx
│   │   │   ├── layanan/
│   │   │   │   └── page.tsx
│   │   │   ├── dokter/
│   │   │   │   └── page.tsx
│   │   │   └── booking/
│   │   │       └── page.tsx
│   │   ├── (dashboard)/
│   │   │   ├── layout.tsx
│   │   │   ├── sidebar.tsx
│   │   │   ├── topbar.tsx
│   │   │   ├── owner/
│   │   │   │   ├── dashboard/page.tsx
│   │   │   │   ├── users/page.tsx
│   │   │   │   ├── users/[id]/page.tsx
│   │   │   │   ├── laporan/page.tsx
│   │   │   │   ├── hewan/page.tsx
│   │   │   │   ├── appointment/page.tsx
│   │   │   │   ├── rawat-inap/page.tsx
│   │   │   │   ├── booking/page.tsx
│   │   │   │   ├── booking/slots/page.tsx
│   │   │   │   └── settings/page.tsx
│   │   │   ├── dokter/
│   │   │   │   ├── dashboard/page.tsx
│   │   │   │   ├── appointment/page.tsx
│   │   │   │   ├── rekam-medis/page.tsx
│   │   │   │   ├── rekam-medis/[id]/page.tsx
│   │   │   │   ├── rawat-inap/page.tsx
│   │   │   │   ├── rawat-inap/[id]/page.tsx
│   │   │   │   └── hewan/page.tsx
│   │   │   ├── staff/
│   │   │   │   ├── dashboard/page.tsx
│   │   │   │   ├── pos/page.tsx
│   │   │   │   ├── inventory/page.tsx
│   │   │   │   ├── appointment/page.tsx
│   │   │   │   ├── rawat-inap/page.tsx
│   │   │   │   ├── rawat-inap/[id]/page.tsx
│   │   │   │   ├── pengeluaran/page.tsx
│   │   │   │   └── booking/page.tsx
│   │   │   └── customer/
│   │   │       ├── dashboard/page.tsx
│   │   │       ├── hewan/page.tsx
│   │   │       ├── hewan/[id]/page.tsx
│   │   │       ├── rekam-medis/page.tsx
│   │   │       └── monitoring/page.tsx
│   │   └── api/
│   │       ├── auth/
│   │       │   ├── login/route.ts
│   │       │   ├── logout/route.ts
│   │       │   └── me/route.ts
│   │       ├── users/
│   │       │   ├── route.ts
│   │       │   └── [id]/route.ts
│   │       ├── pets/
│   │       │   ├── route.ts
│   │       │   ├── [id]/route.ts
│   │       │   └── [id]/vaccines/route.ts
│   │       ├── appointments/
│   │       │   ├── route.ts
│   │       │   └── [id]/route.ts
│   │       ├── medical-records/
│   │       │   ├── route.ts
│   │       │   └── [id]/route.ts
│   │       ├── inpatients/
│   │       │   ├── route.ts
│   │       │   ├── [id]/route.ts
│   │       │   └── [id]/logs/route.ts
│   │       ├── pos/
│   │       │   ├── catalog/route.ts
│   │       │   └── transactions/
│   │       │       ├── route.ts
│   │       │       └── [id]/route.ts
│   │       ├── inventory/
│   │       │   ├── products/route.ts
│   │       │   ├── products/[id]/route.ts
│   │       │   ├── products/[id]/stock/route.ts
│   │       │   ├── mutations/route.ts
│   │       │   ├── services/route.ts
│   │       │   ├── services/[id]/route.ts
│   │       │   ├── categories/route.ts
│   │       │   └── categories/[id]/route.ts
│   │       ├── expenses/
│   │       │   ├── route.ts
│   │       │   └── [id]/route.ts
│   │       ├── reports/
│   │       │   ├── summary/route.ts
│   │       │   ├── revenue/route.ts
│   │       │   ├── breakdown/route.ts
│   │       │   ├── top-products/route.ts
│   │       │   ├── payment-methods/route.ts
│   │       │   ├── transactions/route.ts
│   │       │   ├── stock-alert/route.ts
│   │       │   └── doctors/route.ts
│   │       ├── booking/
│   │       │   ├── route.ts
│   │       │   ├── [id]/route.ts
│   │       │   ├── list/route.ts
│   │       │   └── slots/
│   │       │       ├── route.ts
│   │       │       └── [id]/route.ts
│   │       └── settings/
│   │           └── clinic/route.ts
│   ├── components/
│   │   ├── ui/
│   │   │   ├── input.tsx
│   │   │   ├── select.tsx
│   │   │   ├── modal.tsx
│   │   │   ├── table.tsx
│   │   │   ├── badge.tsx
│   │   │   ├── card.tsx
│   │   │   ├── button.tsx
│   │   │   ├── page-header.tsx
│   │   │   ├── loading-spinner.tsx
│   │   │   ├── empty-state.tsx
│   │   │   ├── confirm-dialog.tsx
│   │   │   ├── image-upload.tsx
│   │   │   ├── date-range-picker.tsx
│   │   │   └── print-wrapper.tsx
│   │   ├── layout/
│   │   │   ├── sidebar.tsx
│   │   │   └── topbar.tsx
│   │   ├── modules/
│   │   │   ├── pos/
│   │   │   │   ├── catalog-grid.tsx
│   │   │   │   ├── cart.tsx
│   │   │   │   ├── payment-modal.tsx
│   │   │   │   └── receipt.tsx
│   │   │   ├── klinik/
│   │   │   │   ├── medical-record-form.tsx
│   │   │   │   ├── medical-record-detail.tsx
│   │   │   │   ├── inpatient-card.tsx
│   │   │   │   ├── monitoring-timeline.tsx
│   │   │   │   └── monitoring-log-form.tsx
│   │   │   ├── inventory/
│   │   │   │   ├── product-form.tsx
│   │   │   │   ├── stock-mutation-form.tsx
│   │   │   │   └── mutation-table.tsx
│   │   │   ├── laporan/
│   │   │   │   ├── summary-cards.tsx
│   │   │   │   ├── revenue-chart.tsx
│   │   │   │   ├── breakdown-chart.tsx
│   │   │   │   ├── top-products-chart.tsx
│   │   │   │   └── payment-pie-chart.tsx
│   │   │   └── booking/
│   │   │       ├── slot-grid.tsx
│   │   │       ├── booking-form.tsx
│   │   │       └── booking-table.tsx
│   │   └── shared/
│   │       ├── pet-search.tsx
│   │       ├── customer-search.tsx
│   │       ├── dokter-select.tsx
│   │       └── status-badge.tsx
│   ├── lib/
│   │   ├── supabase/
│   │   │   ├── client.ts
│   │   │   └── server.ts
│   │   ├── auth.ts
│   │   ├── constants.ts
│   │   ├── types.ts
│   │   ├── validations/
│   │   │   ├── user.ts
│   │   │   ├── pet.ts
│   │   │   ├── appointment.ts
│   │   │   ├── medical-record.ts
│   │   │   ├── inpatient.ts
│   │   │   ├── pos.ts
│   │   │   ├── inventory.ts
│   │   │   ├── expense.ts
│   │   │   ├── booking.ts
│   │   │   └── settings.ts
│   │   └── utils/
│   │       ├── invoice.ts
│   │       ├── report.ts
│   │       ├── upload.ts
│   │       ├── format.ts
│   │       └── csv-export.ts
│   ├── hooks/
│   │   ├── use-auth.ts
│   │   ├── use-pos.ts
│   │   ├── use-upload.ts
│   │   └── use-print.ts
│   └── middleware.ts
├── supabase/
│   ├── schema.sql
│   ├── rls.sql
│   ├── triggers.sql
│   └── storage.sql
├── .env.local
├── .env.example
├── next.config.js
├── tailwind.config.ts
├── tsconfig.json
└── package.json
```

---

## DATABASE SCHEMA LENGKAP

```sql
-- ============================================
-- TABLE: profiles
-- ============================================
CREATE TABLE profiles (
  id              UUID PRIMARY KEY REFERENCES auth.users ON DELETE CASCADE,
  role            TEXT NOT NULL CHECK (role IN ('owner','dokter','staff','customer')),
  nama_lengkap    TEXT NOT NULL,
  no_hp           TEXT,
  alamat          TEXT,
  foto_profil     TEXT,
  is_active       BOOLEAN DEFAULT TRUE,
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: dokter_profiles
-- ============================================
CREATE TABLE dokter_profiles (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id         UUID NOT NULL REFERENCES profiles ON DELETE CASCADE,
  no_str          TEXT,
  spesialisasi    TEXT,
  bio             TEXT,
  jadwal_praktik  JSONB DEFAULT '[]',
  -- [{ hari: "Senin", jam_mulai: "08:00", jam_selesai: "16:00" }]
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: pets
-- ============================================
CREATE TABLE pets (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  owner_id        UUID NOT NULL REFERENCES profiles ON DELETE RESTRICT,
  nama            TEXT NOT NULL,
  spesies         TEXT NOT NULL,
  ras             TEXT,
  jenis_kelamin   TEXT CHECK (jenis_kelamin IN ('jantan','betina')),
  tgl_lahir       DATE,
  berat_kg        NUMERIC(5,2),
  warna           TEXT,
  ciri_khas       TEXT,
  foto            TEXT,
  status          TEXT DEFAULT 'aktif' CHECK (status IN ('aktif','meninggal')),
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: pet_vaccines
-- ============================================
CREATE TABLE pet_vaccines (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  pet_id          UUID NOT NULL REFERENCES pets ON DELETE CASCADE,
  nama_vaksin     TEXT NOT NULL,
  tgl_vaksin      DATE NOT NULL,
  tgl_berikutnya  DATE,
  dokter_id       UUID REFERENCES profiles,
  keterangan      TEXT,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: categories
-- ============================================
CREATE TABLE categories (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  nama            TEXT NOT NULL,
  tipe            TEXT NOT NULL CHECK (tipe IN ('produk','layanan')),
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: products
-- ============================================
CREATE TABLE products (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  category_id     UUID REFERENCES categories ON DELETE SET NULL,
  kode_produk     TEXT UNIQUE NOT NULL,
  nama            TEXT NOT NULL,
  deskripsi       TEXT,
  foto            TEXT,
  harga_beli      NUMERIC(12,2) DEFAULT 0,
  harga_jual      NUMERIC(12,2) NOT NULL,
  stok            INTEGER DEFAULT 0 CHECK (stok >= 0),
  stok_minimum    INTEGER DEFAULT 5,
  satuan          TEXT DEFAULT 'pcs',
  is_active       BOOLEAN DEFAULT TRUE,
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: services
-- ============================================
CREATE TABLE services (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  category_id     UUID REFERENCES categories ON DELETE SET NULL,
  nama            TEXT NOT NULL,
  deskripsi       TEXT,
  harga           NUMERIC(12,2) NOT NULL,
  durasi_menit    INTEGER,
  dokter_required BOOLEAN DEFAULT FALSE,
  is_active       BOOLEAN DEFAULT TRUE,
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: appointments
-- ============================================
CREATE TABLE appointments (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  pet_id          UUID NOT NULL REFERENCES pets ON DELETE RESTRICT,
  dokter_id       UUID REFERENCES profiles ON DELETE SET NULL,
  customer_id     UUID NOT NULL REFERENCES profiles ON DELETE RESTRICT,
  tgl_janji       TIMESTAMPTZ NOT NULL,
  jenis           TEXT NOT NULL CHECK (jenis IN ('konsultasi','grooming','operasi','vaksin','lainnya')),
  status          TEXT DEFAULT 'pending' CHECK (status IN ('pending','konfirmasi','selesai','batal')),
  keluhan         TEXT,
  catatan_staff   TEXT,
  sumber          TEXT DEFAULT 'langsung' CHECK (sumber IN ('langsung','online')),
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: medical_records
-- ============================================
CREATE TABLE medical_records (
  id                   UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  pet_id               UUID NOT NULL REFERENCES pets ON DELETE RESTRICT,
  dokter_id            UUID NOT NULL REFERENCES profiles ON DELETE RESTRICT,
  appointment_id       UUID REFERENCES appointments ON DELETE SET NULL,
  tanggal              TIMESTAMPTZ DEFAULT NOW(),
  berat_saat_periksa   NUMERIC(5,2),
  suhu                 NUMERIC(4,1),
  keluhan              TEXT,
  anamnesis            TEXT,
  diagnosis            TEXT NOT NULL,
  tindakan             TEXT,
  resep                JSONB DEFAULT '[]',
  -- [{ nama_obat, dosis, frekuensi, durasi, qty, aturan_pakai }]
  catatan_followup     TEXT,
  is_visible_customer  BOOLEAN DEFAULT TRUE,
  created_at           TIMESTAMPTZ DEFAULT NOW(),
  updated_at           TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: inpatients
-- ============================================
CREATE TABLE inpatients (
  id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  pet_id            UUID NOT NULL REFERENCES pets ON DELETE RESTRICT,
  dokter_id         UUID NOT NULL REFERENCES profiles ON DELETE RESTRICT,
  medical_record_id UUID REFERENCES medical_records ON DELETE SET NULL,
  no_kandang        TEXT NOT NULL,
  tgl_masuk         TIMESTAMPTZ DEFAULT NOW(),
  tgl_keluar        TIMESTAMPTZ,
  diagnosis_awal    TEXT,
  tindakan_awal     TEXT,
  status            TEXT DEFAULT 'aktif' CHECK (status IN ('aktif','sembuh','dirujuk','meninggal')),
  created_at        TIMESTAMPTZ DEFAULT NOW(),
  updated_at        TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: inpatient_logs
-- ============================================
CREATE TABLE inpatient_logs (
  id                   UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  inpatient_id         UUID NOT NULL REFERENCES inpatients ON DELETE CASCADE,
  staff_id             UUID NOT NULL REFERENCES profiles ON DELETE RESTRICT,
  timestamp            TIMESTAMPTZ DEFAULT NOW(),
  kondisi              TEXT NOT NULL CHECK (kondisi IN ('kritis','lemah','stabil','baik','sangat_baik')),
  berat                NUMERIC(5,2),
  suhu                 NUMERIC(4,1),
  nafsu_makan          TEXT CHECK (nafsu_makan IN ('tidak_mau','sedikit','normal','lahap')),
  catatan_kondisi      TEXT NOT NULL,
  tindakan_hari_ini    TEXT,
  obat_hari_ini        TEXT,
  foto_urls            JSONB DEFAULT '[]',
  -- ["https://...supabase.../clinic-uploads/inpatient-logs/..."]
  is_visible_customer  BOOLEAN DEFAULT TRUE,
  created_at           TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: transactions
-- ============================================
CREATE TABLE transactions (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  no_transaksi    TEXT UNIQUE NOT NULL,
  -- auto: INV-YYYYMMDD-XXXX via trigger
  customer_id     UUID REFERENCES profiles ON DELETE SET NULL,
  kasir_id        UUID NOT NULL REFERENCES profiles ON DELETE RESTRICT,
  tgl_transaksi   TIMESTAMPTZ DEFAULT NOW(),
  subtotal        NUMERIC(12,2) NOT NULL,
  diskon_nominal  NUMERIC(12,2) DEFAULT 0,
  total           NUMERIC(12,2) NOT NULL,
  metode_bayar    TEXT NOT NULL CHECK (metode_bayar IN ('tunai','transfer','qris','debit')),
  uang_diterima   NUMERIC(12,2),
  kembalian       NUMERIC(12,2) DEFAULT 0,
  status          TEXT DEFAULT 'lunas' CHECK (status IN ('draft','lunas','batal')),
  catatan         TEXT,
  appointment_id  UUID REFERENCES appointments ON DELETE SET NULL,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: transaction_items
-- ============================================
CREATE TABLE transaction_items (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  transaction_id  UUID NOT NULL REFERENCES transactions ON DELETE CASCADE,
  tipe_item       TEXT NOT NULL CHECK (tipe_item IN ('produk','layanan')),
  product_id      UUID REFERENCES products ON DELETE SET NULL,
  service_id      UUID REFERENCES services ON DELETE SET NULL,
  nama_item       TEXT NOT NULL,
  -- snapshot nama saat transaksi
  harga_satuan    NUMERIC(12,2) NOT NULL,
  -- snapshot harga saat transaksi
  qty             INTEGER NOT NULL DEFAULT 1 CHECK (qty > 0),
  diskon          NUMERIC(12,2) DEFAULT 0,
  subtotal        NUMERIC(12,2) NOT NULL,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: stock_mutations
-- ============================================
CREATE TABLE stock_mutations (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  product_id      UUID NOT NULL REFERENCES products ON DELETE RESTRICT,
  tipe            TEXT NOT NULL CHECK (tipe IN ('masuk','keluar','adjustment')),
  qty_sebelum     INTEGER NOT NULL,
  qty_perubahan   INTEGER NOT NULL,
  qty_sesudah     INTEGER NOT NULL,
  referensi       TEXT,
  -- no_transaksi jika dari POS, 'Manual' jika manual
  staff_id        UUID NOT NULL REFERENCES profiles ON DELETE RESTRICT,
  catatan         TEXT,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: expenses
-- ============================================
CREATE TABLE expenses (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  kategori        TEXT NOT NULL,
  deskripsi       TEXT NOT NULL,
  jumlah          NUMERIC(12,2) NOT NULL CHECK (jumlah > 0),
  tgl_pengeluaran DATE NOT NULL,
  bukti_foto      TEXT,
  staff_id        UUID NOT NULL REFERENCES profiles ON DELETE RESTRICT,
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: booking_slots
-- ============================================
CREATE TABLE booking_slots (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  dokter_id       UUID NOT NULL REFERENCES profiles ON DELETE CASCADE,
  tanggal         DATE NOT NULL,
  jam_mulai       TIME NOT NULL,
  jam_selesai     TIME NOT NULL,
  kuota           INTEGER DEFAULT 1 CHECK (kuota > 0),
  terisi          INTEGER DEFAULT 0 CHECK (terisi >= 0),
  is_available    BOOLEAN DEFAULT TRUE,
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE (dokter_id, tanggal, jam_mulai)
);

-- ============================================
-- TABLE: online_bookings
-- ============================================
CREATE TABLE online_bookings (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  slot_id         UUID NOT NULL REFERENCES booking_slots ON DELETE RESTRICT,
  customer_id     UUID REFERENCES profiles ON DELETE SET NULL,
  nama_guest      TEXT,
  no_hp_guest     TEXT,
  nama_hewan      TEXT NOT NULL,
  spesies         TEXT NOT NULL,
  keluhan         TEXT,
  status          TEXT DEFAULT 'menunggu' CHECK (status IN ('menunggu','dikonfirmasi','ditolak','selesai')),
  catatan_klinik  TEXT,
  alasan_tolak    TEXT,
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================
-- TABLE: clinic_info
-- ============================================
CREATE TABLE clinic_info (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  nama_klinik     TEXT NOT NULL,
  logo            TEXT,
  alamat          TEXT,
  no_hp           TEXT,
  email           TEXT,
  jam_buka        TIME,
  jam_tutup       TIME,
  hari_buka       JSONB DEFAULT '[]',
  -- ["Senin","Selasa","Rabu","Kamis","Jumat","Sabtu"]
  sosmed          JSONB DEFAULT '{}',
  -- { instagram, facebook, whatsapp }
  footer_struk    TEXT DEFAULT 'Terima kasih telah mempercayai kami',
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);
```

---

## TRIGGERS SQL

```sql
-- ============================================
-- TRIGGER 1: Auto create profile saat register
-- ============================================
CREATE OR REPLACE FUNCTION handle_new_user()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO profiles (id, role, nama_lengkap)
  VALUES (
    NEW.id,
    COALESCE(NEW.raw_user_meta_data->>'role', 'customer'),
    COALESCE(NEW.raw_user_meta_data->>'nama_lengkap', NEW.email)
  );
  RETURN NEW;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;

CREATE TRIGGER on_auth_user_created
  AFTER INSERT ON auth.users
  FOR EACH ROW EXECUTE FUNCTION handle_new_user();

-- ============================================
-- TRIGGER 2: Auto generate no_transaksi
-- ============================================
CREATE OR REPLACE FUNCTION generate_no_transaksi()
RETURNS TRIGGER AS $$
DECLARE
  urutan INTEGER;
BEGIN
  SELECT COUNT(*) + 1 INTO urutan
  FROM transactions
  WHERE DATE(tgl_transaksi) = CURRENT_DATE;
  NEW.no_transaksi := 'INV-' ||
    TO_CHAR(NOW(), 'YYYYMMDD') || '-' ||
    LPAD(urutan::TEXT, 4, '0');
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER before_insert_transaction
  BEFORE INSERT ON transactions
  FOR EACH ROW
  WHEN (NEW.no_transaksi IS NULL)
  EXECUTE FUNCTION generate_no_transaksi();

-- ============================================
-- TRIGGER 3: Auto update updated_at
-- ============================================
CREATE OR REPLACE FUNCTION update_updated_at()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER update_profiles_updated_at
  BEFORE UPDATE ON profiles
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_pets_updated_at
  BEFORE UPDATE ON pets
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_appointments_updated_at
  BEFORE UPDATE ON appointments
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_medical_records_updated_at
  BEFORE UPDATE ON medical_records
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_inpatients_updated_at
  BEFORE UPDATE ON inpatients
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_products_updated_at
  BEFORE UPDATE ON products
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_expenses_updated_at
  BEFORE UPDATE ON expenses
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

CREATE TRIGGER update_online_bookings_updated_at
  BEFORE UPDATE ON online_bookings
  FOR EACH ROW EXECUTE FUNCTION update_updated_at();

-- ============================================
-- TRIGGER 4: Auto update booking_slots.terisi
-- ============================================
CREATE OR REPLACE FUNCTION update_slot_terisi()
RETURNS TRIGGER AS $$
BEGIN
  IF TG_OP = 'INSERT' AND NEW.status = 'menunggu' THEN
    UPDATE booking_slots
    SET terisi = terisi + 1,
        is_available = CASE WHEN terisi + 1 >= kuota THEN FALSE ELSE TRUE END
    WHERE id = NEW.slot_id;
  END IF;

  IF TG_OP = 'UPDATE' AND OLD.status = 'menunggu' AND NEW.status = 'ditolak' THEN
    UPDATE booking_slots
    SET terisi = GREATEST(terisi - 1, 0),
        is_available = TRUE
    WHERE id = NEW.slot_id;
  END IF;

  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER on_booking_change
  AFTER INSERT OR UPDATE ON online_bookings
  FOR EACH ROW EXECUTE FUNCTION update_slot_terisi();
```

---

## RLS POLICIES

```sql
-- Enable RLS semua tabel
ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE dokter_profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE pets ENABLE ROW LEVEL SECURITY;
ALTER TABLE pet_vaccines ENABLE ROW LEVEL SECURITY;
ALTER TABLE categories ENABLE ROW LEVEL SECURITY;
ALTER TABLE products ENABLE ROW LEVEL SECURITY;
ALTER TABLE services ENABLE ROW LEVEL SECURITY;
ALTER TABLE appointments ENABLE ROW LEVEL SECURITY;
ALTER TABLE medical_records ENABLE ROW LEVEL SECURITY;
ALTER TABLE inpatients ENABLE ROW LEVEL SECURITY;
ALTER TABLE inpatient_logs ENABLE ROW LEVEL SECURITY;
ALTER TABLE transactions ENABLE ROW LEVEL SECURITY;
ALTER TABLE transaction_items ENABLE ROW LEVEL SECURITY;
ALTER TABLE stock_mutations ENABLE ROW LEVEL SECURITY;
ALTER TABLE expenses ENABLE ROW LEVEL SECURITY;
ALTER TABLE booking_slots ENABLE ROW LEVEL SECURITY;
ALTER TABLE online_bookings ENABLE ROW LEVEL SECURITY;
ALTER TABLE clinic_info ENABLE ROW LEVEL SECURITY;

-- Helper function role check
CREATE OR REPLACE FUNCTION get_my_role()
RETURNS TEXT AS $$
  SELECT role FROM profiles WHERE id = auth.uid();
$$ LANGUAGE sql SECURITY DEFINER STABLE;

-- ============================================
-- PROFILES
-- ============================================
CREATE POLICY "profiles_select_own" ON profiles
  FOR SELECT USING (id = auth.uid());

CREATE POLICY "profiles_select_staff" ON profiles
  FOR SELECT USING (get_my_role() IN ('owner','dokter','staff'));

CREATE POLICY "profiles_update_own" ON profiles
  FOR UPDATE USING (id = auth.uid());

CREATE POLICY "profiles_update_owner" ON profiles
  FOR UPDATE USING (get_my_role() = 'owner');

CREATE POLICY "profiles_insert_owner" ON profiles
  FOR INSERT WITH CHECK (get_my_role() = 'owner');

-- ============================================
-- PETS
-- ============================================
CREATE POLICY "pets_select_customer" ON pets
  FOR SELECT USING (
    owner_id = auth.uid() OR
    get_my_role() IN ('owner','dokter','staff')
  );

CREATE POLICY "pets_insert_auth" ON pets
  FOR INSERT WITH CHECK (auth.uid() IS NOT NULL);

CREATE POLICY "pets_update_customer" ON pets
  FOR UPDATE USING (
    owner_id = auth.uid() OR
    get_my_role() IN ('owner','dokter','staff')
  );

-- ============================================
-- MEDICAL RECORDS
-- ============================================
CREATE POLICY "medical_records_select_staff" ON medical_records
  FOR SELECT USING (get_my_role() IN ('owner','dokter'));

CREATE POLICY "medical_records_select_customer" ON medical_records
  FOR SELECT USING (
    get_my_role() = 'customer' AND
    is_visible_customer = TRUE AND
    pet_id IN (SELECT id FROM pets WHERE owner_id = auth.uid())
  );

CREATE POLICY "medical_records_insert" ON medical_records
  FOR INSERT WITH CHECK (get_my_role() IN ('owner','dokter'));

CREATE POLICY "medical_records_update" ON medical_records
  FOR UPDATE USING (get_my_role() IN ('owner','dokter'));

-- ============================================
-- INPATIENTS
-- ============================================
CREATE POLICY "inpatients_select_staff" ON inpatients
  FOR SELECT USING (get_my_role() IN ('owner','dokter','staff'));

CREATE POLICY "inpatients_select_customer" ON inpatients
  FOR SELECT USING (
    get_my_role() = 'customer' AND
    pet_id IN (SELECT id FROM pets WHERE owner_id = auth.uid())
  );

CREATE POLICY "inpatients_insert" ON inpatients
  FOR INSERT WITH CHECK (get_my_role() IN ('owner','dokter'));

CREATE POLICY "inpatients_update" ON inpatients
  FOR UPDATE USING (get_my_role() IN ('owner','dokter'));

-- ============================================
-- INPATIENT LOGS
-- ============================================
CREATE POLICY "logs_select_staff" ON inpatient_logs
  FOR SELECT USING (get_my_role() IN ('owner','dokter','staff'));

CREATE POLICY "logs_select_customer" ON inpatient_logs
  FOR SELECT USING (
    get_my_role() = 'customer' AND
    is_visible_customer = TRUE AND
    inpatient_id IN (
      SELECT i.id FROM inpatients i
      JOIN pets p ON p.id = i.pet_id
      WHERE p.owner_id = auth.uid()
    )
  );

CREATE POLICY "logs_insert" ON inpatient_logs
  FOR INSERT WITH CHECK (get_my_role() IN ('owner','dokter','staff'));

-- ============================================
-- TRANSACTIONS
-- ============================================
CREATE POLICY "transactions_select_customer" ON transactions
  FOR SELECT USING (
    customer_id = auth.uid() OR
    get_my_role() IN ('owner','staff')
  );

CREATE POLICY "transactions_insert" ON transactions
  FOR INSERT WITH CHECK (get_my_role() IN ('owner','staff'));

CREATE POLICY "transactions_update" ON transactions
  FOR UPDATE USING (get_my_role() IN ('owner','staff'));

-- ============================================
-- TRANSACTION ITEMS
-- ============================================
CREATE POLICY "items_select" ON transaction_items
  FOR SELECT USING (
    get_my_role() IN ('owner','staff') OR
    transaction_id IN (
      SELECT id FROM transactions WHERE customer_id = auth.uid()
    )
  );

CREATE POLICY "items_insert" ON transaction_items
  FOR INSERT WITH CHECK (get_my_role() IN ('owner','staff'));

-- ============================================
-- PRODUCTS & SERVICES (public read)
-- ============================================
CREATE POLICY "products_select_all" ON products
  FOR SELECT USING (TRUE);

CREATE POLICY "products_write_staff" ON products
  FOR ALL USING (get_my_role() IN ('owner','staff'));

CREATE POLICY "services_select_all" ON services
  FOR SELECT USING (TRUE);

CREATE POLICY "services_write_staff" ON services
  FOR ALL USING (get_my_role() IN ('owner','staff'));

CREATE POLICY "categories_select_all" ON categories
  FOR SELECT USING (TRUE);

CREATE POLICY "categories_write" ON categories
  FOR ALL USING (get_my_role() IN ('owner','staff'));

-- ============================================
-- STOCK MUTATIONS
-- ============================================
CREATE POLICY "mutations_select" ON stock_mutations
  FOR SELECT USING (get_my_role() IN ('owner','staff'));

CREATE POLICY "mutations_insert" ON stock_mutations
  FOR INSERT WITH CHECK (get_my_role() IN ('owner','staff'));

-- ============================================
-- EXPENSES
-- ============================================
CREATE POLICY "expenses_select" ON expenses
  FOR SELECT USING (get_my_role() IN ('owner','staff'));

CREATE POLICY "expenses_insert" ON expenses
  FOR INSERT WITH CHECK (get_my_role() IN ('owner','staff'));

CREATE POLICY "expenses_update" ON expenses
  FOR UPDATE USING (get_my_role() IN ('owner','staff'));

CREATE POLICY "expenses_delete" ON expenses
  FOR DELETE USING (get_my_role() = 'owner');

-- ============================================
-- BOOKING SLOTS (public read)
-- ============================================
CREATE POLICY "slots_select_public" ON booking_slots
  FOR SELECT USING (TRUE);

CREATE POLICY "slots_write_owner" ON booking_slots
  FOR ALL USING (get_my_role() = 'owner');

-- ============================================
-- ONLINE BOOKINGS
-- ============================================
CREATE POLICY "bookings_select_customer" ON online_bookings
  FOR SELECT USING (
    customer_id = auth.uid() OR
    get_my_role() IN ('owner','staff')
  );

CREATE POLICY "bookings_insert_public" ON online_bookings
  FOR INSERT WITH CHECK (TRUE);

CREATE POLICY "bookings_update_staff" ON online_bookings
  FOR UPDATE USING (get_my_role() IN ('owner','staff'));

-- ============================================
-- CLINIC INFO (public read)
-- ============================================
CREATE POLICY "clinic_info_select_public" ON clinic_info
  FOR SELECT USING (TRUE);

CREATE POLICY "clinic_info_write_owner" ON clinic_info
  FOR ALL USING (get_my_role() = 'owner');
```

---

## SUPABASE STORAGE

```sql
-- Bucket: clinic-uploads (public)
INSERT INTO storage.buckets (id, name, public)
VALUES ('clinic-uploads', 'clinic-uploads', TRUE);

-- Policy: authenticated upload
CREATE POLICY "authenticated_upload" ON storage.objects
  FOR INSERT WITH CHECK (
    bucket_id = 'clinic-uploads' AND
    auth.uid() IS NOT NULL
  );

-- Policy: public read
CREATE POLICY "public_read" ON storage.objects
  FOR SELECT USING (bucket_id = 'clinic-uploads');

-- Policy: owner delete own file
CREATE POLICY "owner_delete" ON storage.objects
  FOR DELETE USING (
    bucket_id = 'clinic-uploads' AND
    auth.uid() IS NOT NULL
  );

-- Folder convention:
-- pets/{pet_id}-{timestamp}.jpg
-- inpatient-logs/{inpatient_id}/{timestamp}-{n}.jpg
-- products/{product_id}-{timestamp}.jpg
-- expenses/{expense_id}-{timestamp}.jpg
-- profiles/{user_id}-{timestamp}.jpg
-- clinic/logo.jpg
```

---

## API ROUTE CONTRACT

```typescript
// Standard response wrapper — WAJIB semua route
type ApiSuccess<T> = { data: T; message?: string }
type ApiError = { error: string; code?: string }

// Standard HTTP status:
// 200 → OK
// 201 → Created
// 400 → Validation error / bad request
// 401 → Tidak terautentikasi
// 403 → Tidak punya akses (role)
// 404 → Data tidak ditemukan
// 409 → Conflict (stok tidak cukup, slot penuh)
// 500 → Server error

// Wajib di setiap API route:
// 1. Ambil session → 401 jika tidak ada
// 2. Cek role → 403 jika tidak sesuai
// 3. Parse & validasi body/query dengan Zod → 400 jika invalid
// 4. Business logic dalam try/catch
// 5. Return typed response
```

---

## MIDDLEWARE ROUTING

```typescript
// src/middleware.ts
// Route protection map:
const ROLE_ROUTES: Record<string, string[]> = {
  '/owner':    ['owner'],
  '/dokter':   ['dokter', 'owner'],
  '/staff':    ['staff', 'owner'],
  '/customer': ['customer'],
}

// Flow:
// 1. Request masuk
// 2. Jika /api/* → skip middleware (handle di route)
// 3. Jika /(public)/* → skip (public)
// 4. Jika /login → skip
// 5. Ambil session Supabase
// 6. Jika tidak ada session → redirect /login
// 7. Ambil role dari profiles
// 8. Cek role vs route → 403 jika tidak sesuai
// 9. Lanjut ke page
```

---

## BUSINESS LOGIC KRITIS

### POS — Atomic Transaction
```
URUTAN WAJIB (semua dalam satu Supabase RPC atau sequential):
1. Validasi semua product_id: is_active=true, stok >= qty
2. Hitung subtotal per item: (harga_jual × qty) - diskon_item
3. Hitung total: SUM(subtotal) - diskon_nominal
4. Jika metode=tunai: assert uang_diterima >= total
5. Hitung kembalian: uang_diterima - total
6. INSERT transactions → dapat id
7. INSERT transaction_items[] (snapshot nama + harga)
8. Untuk setiap produk:
   a. SELECT stok FOR UPDATE (lock row)
   b. UPDATE products SET stok = stok - qty
   c. INSERT stock_mutations (masuk/keluar/referensi=no_transaksi)
9. Jika appointment_id ada: UPDATE appointments SET status='selesai'
10. RETURN transaction dengan no_transaksi untuk struk

ROLLBACK jika:
- Stok tidak cukup di step 8b
- Insert gagal di langkah manapun
```

### BOOKING — Konfirmasi Flow
```
1. Staff konfirmasi online_booking
2. UPDATE online_bookings SET status='dikonfirmasi'
3. Auto INSERT appointments:
   - pet_id: cari/create berdasarkan nama_hewan + customer_id
   - dokter_id: dari booking_slots.dokter_id
   - customer_id: dari booking.customer_id
   - tgl_janji: dari slot tanggal + jam_mulai
   - jenis: 'konsultasi'
   - sumber: 'online'
4. UPDATE online_bookings SET status='selesai' setelah appointment selesai
```

### STOK — Invariant
```
TIDAK BOLEH:
- stok < 0 (constraint DB)
- transaksi tanpa lock row (race condition)
- adjustment tanpa catatan alasan
- keluar stok tanpa referensi transaksi

WAJIB:
- Setiap perubahan stok → INSERT stock_mutations
- qty_sesudah = qty_sebelum + qty_perubahan (verifikasi di aplikasi)
```

### LAPORAN — Query

```sql
-- Summary
SELECT
  COALESCE(SUM(t.total) FILTER (WHERE t.status = 'lunas'), 0) AS total_pemasukan,
  COALESCE(SUM(e.jumlah), 0) AS total_pengeluaran,
  COALESCE(SUM(t.total) FILTER (WHERE t.status = 'lunas'), 0)
    - COALESCE(SUM(e.jumlah), 0) AS laba_bersih
FROM transactions t
FULL OUTER JOIN expenses e ON DATE(e.tgl_pengeluaran) BETWEEN :from AND :to
WHERE DATE(t.tgl_transaksi) BETWEEN :from AND :to;

-- Revenue per hari
SELECT
  DATE(tgl_transaksi) AS tanggal,
  SUM(total) FILTER (WHERE status='lunas') AS pemasukan
FROM transactions
WHERE tgl_transaksi BETWEEN :from AND :to
GROUP BY DATE(tgl_transaksi)
ORDER BY tanggal;

-- Top produk
SELECT
  ti.nama_item,
  SUM(ti.qty) AS total_terjual,
  SUM(ti.subtotal) AS total_revenue
FROM transaction_items ti
JOIN transactions t ON t.id = ti.transaction_id
WHERE t.status = 'lunas'
  AND DATE(t.tgl_transaksi) BETWEEN :from AND :to
  AND ti.tipe_item = 'produk'
GROUP BY ti.nama_item
ORDER BY total_terjual DESC
LIMIT 10;
```

---

## VALIDASI ZOD LENGKAP

```typescript
// src/lib/validations/user.ts
export const CreateUserSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
  role: z.enum(['owner','dokter','staff','customer']),
  nama_lengkap: z.string().min(2).max(100),
  no_hp: z.string().optional(),
  alamat: z.string().optional(),
  dokter_profile: z.object({
    no_str: z.string().optional(),
    spesialisasi: z.string().optional(),
    bio: z.string().optional(),
    jadwal_praktik: z.array(z.object({
      hari: z.string(),
      jam_mulai: z.string(),
      jam_selesai: z.string()
    })).optional()
  }).optional()
})

// src/lib/validations/pet.ts
export const CreatePetSchema = z.object({
  owner_id: z.string().uuid(),
  nama: z.string().min(1).max(100),
  spesies: z.string().min(1),
  ras: z.string().optional(),
  jenis_kelamin: z.enum(['jantan','betina']).optional(),
  tgl_lahir: z.string().date().optional(),
  berat_kg: z.number().positive().optional(),
  warna: z.string().optional(),
  ciri_khas: z.string().optional(),
})

// src/lib/validations/medical-record.ts
const ResepItemSchema = z.object({
  nama_obat: z.string().min(1),
  dosis: z.string().min(1),
  frekuensi: z.string().min(1),
  durasi: z.string().min(1),
  qty: z.number().int().positive(),
  aturan_pakai: z.string().optional()
})

export const CreateMedicalRecordSchema = z.object({
  pet_id: z.string().uuid(),
  dokter_id: z.string().uuid(),
  appointment_id: z.string().uuid().optional(),
  berat_saat_periksa: z.number().positive().optional(),
  suhu: z.number().min(30).max(45).optional(),
  keluhan: z.string().optional(),
  anamnesis: z.string().optional(),
  diagnosis: z.string().min(1),
  tindakan: z.string().optional(),
  resep: z.array(ResepItemSchema).default([]),
  catatan_followup: z.string().optional(),
  is_visible_customer: z.boolean().default(true)
})

// src/lib/validations/inpatient.ts
export const CreateInpatientSchema = z.object({
  pet_id: z.string().uuid(),
  dokter_id: z.string().uuid(),
  medical_record_id: z.string().uuid().optional(),
  no_kandang: z.string().min(1),
  diagnosis_awal: z.string().optional(),
  tindakan_awal: z.string().optional()
})

export const CreateInpatientLogSchema = z.object({
  kondisi: z.enum(['kritis','lemah','stabil','baik','sangat_baik']),
  berat: z.number().positive().optional(),
  suhu: z.number().min(30).max(45).optional(),
  nafsu_makan: z.enum(['tidak_mau','sedikit','normal','lahap']).optional(),
  catatan_kondisi: z.string().min(1),
  tindakan_hari_ini: z.string().optional(),
  obat_hari_ini: z.string().optional(),
  foto_urls: z.array(z.string().url()).max(5).default([]),
  is_visible_customer: z.boolean().default(true)
})

// src/lib/validations/pos.ts
const TransactionItemSchema = z.object({
  tipe_item: z.enum(['produk','layanan']),
  product_id: z.string().uuid().optional(),
  service_id: z.string().uuid().optional(),
  qty: z.number().int().positive(),
  diskon: z.number().min(0).default(0)
}).refine(
  data => data.product_id || data.service_id,
  { message: 'product_id atau service_id wajib diisi' }
)

export const CreateTransactionSchema = z.object({
  customer_id: z.string().uuid().optional(),
  items: z.array(TransactionItemSchema).min(1),
  metode_bayar: z.enum(['tunai','transfer','qris','debit']),
  diskon_nominal: z.number().min(0).default(0),
  uang_diterima: z.number().positive().optional(),
  catatan: z.string().optional(),
  appointment_id: z.string().uuid().optional()
}).refine(
  data => data.metode_bayar !== 'tunai' || data.uang_diterima !== undefined,
  { message: 'uang_diterima wajib diisi untuk pembayaran tunai' }
)

// src/lib/validations/inventory.ts
export const CreateProductSchema = z.object({
  category_id: z.string().uuid().optional(),
  kode_produk: z.string().min(1).max(50),
  nama: z.string().min(1).max(200),
  deskripsi: z.string().optional(),
  harga_beli: z.number().min(0).default(0),
  harga_jual: z.number().positive(),
  stok: z.number().int().min(0).default(0),
  stok_minimum: z.number().int().min(0).default(5),
  satuan: z.string().default('pcs')
})

export const StockMutationSchema = z.object({
  tipe: z.enum(['masuk','adjustment']),
  qty: z.number().int().positive(),
  catatan: z.string().optional()
})

// src/lib/validations/booking.ts
export const CreateBookingSchema = z.object({
  slot_id: z.string().uuid(),
  customer_id: z.string().uuid().optional(),
  nama_guest: z.string().optional(),
  no_hp_guest: z.string().optional(),
  nama_hewan: z.string().min(1),
  spesies: z.string().min(1),
  keluhan: z.string().optional()
}).refine(
  data => data.customer_id || (data.nama_guest && data.no_hp_guest),
  { message: 'customer_id atau nama_guest + no_hp_guest wajib diisi' }
)
```

---

## CONSTANTS

```typescript
// src/lib/constants.ts
export const ROLES = {
  OWNER: 'owner',
  DOKTER: 'dokter',
  STAFF: 'staff',
  CUSTOMER: 'customer'
} as const

export const APPOINTMENT_STATUS = {
  PENDING: 'pending',
  KONFIRMASI: 'konfirmasi',
  SELESAI: 'selesai',
  BATAL: 'batal'
} as const

export const INPATIENT_STATUS = {
  AKTIF: 'aktif',
  SEMBUH: 'sembuh',
  DIRUJUK: 'dirujuk',
  MENINGGAL: 'meninggal'
} as const

export const KONDISI_HEWAN = {
  KRITIS: 'kritis',
  LEMAH: 'lemah',
  STABIL: 'stabil',
  BAIK: 'baik',
  SANGAT_BAIK: 'sangat_baik'
} as const

export const METODE_BAYAR = {
  TUNAI: 'tunai',
  TRANSFER: 'transfer',
  QRIS: 'qris',
  DEBIT: 'debit'
} as const

export const TRANSACTION_STATUS = {
  DRAFT: 'draft',
  LUNAS: 'lunas',
  BATAL: 'batal'
} as const

export const BOOKING_STATUS = {
  MENUNGGU: 'menunggu',
  DIKONFIRMASI: 'dikonfirmasi',
  DITOLAK: 'ditolak',
  SELESAI: 'selesai'
} as const

export const UPLOAD_CONFIG = {
  MAX_SIZE_MB: 5,
  MAX_FILES: 5,
  ALLOWED_TYPES: ['image/jpeg','image/png','image/webp'],
  BUCKET: 'clinic-uploads'
} as const

export const PAGINATION = {
  DEFAULT_LIMIT: 20,
  MAX_LIMIT: 100
} as const

export const SPESIES_OPTIONS = [
  'Kucing','Anjing','Kelinci','Hamster',
  'Burung','Ikan','Reptil','Lainnya'
] as const
```

---

## ENVIRONMENT VARIABLES

```env
# Supabase
NEXT_PUBLIC_SUPABASE_URL=https://xxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJ...
SUPABASE_SERVICE_ROLE_KEY=eyJ...

# App
NEXT_PUBLIC_APP_URL=https://vetcare.vercel.app

# Tidak ada payment gateway
# Tidak ada email service (gunakan Supabase Auth email)
```

---

## NEXT.CONFIG.JS

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: '*.supabase.co',
        pathname: '/storage/v1/object/public/**'
      }
    ]
  },
  typescript: { tsconfigPath: './tsconfig.json' },
  eslint: { ignoreDuringBuilds: false }
}

module.exports = nextConfig
```

---

## TSCONFIG.JSON

```json
{
  "compilerOptions": {
    "target": "ES2017",
    "lib": ["dom","dom.iterable","esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [{ "name": "next" }],
    "paths": { "@/*": ["./src/*"] }
  },
  "include": ["next-env.d.ts","**/*.ts","**/*.tsx",".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

---

## PACKAGE.JSON

```json
{
  "name": "vetcare-platform",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "type-check": "tsc --noEmit"
  },
  "dependencies": {
    "next": "14.2.0",
    "react": "^18",
    "react-dom": "^18",
    "@supabase/supabase-js": "^2",
    "@supabase/ssr": "^0.4",
    "zod": "^3.23",
    "react-hook-form": "^7.51",
    "@hookform/resolvers": "^3.3",
    "@tanstack/react-table": "^8.17",
    "recharts": "^2.12",
    "date-fns": "^3.6",
    "react-hot-toast": "^2.4",
    "lucide-react": "^0.383",
    "clsx": "^2.1",
    "tailwind-merge": "^2.3",
    "@radix-ui/react-dialog": "^1.0",
    "@radix-ui/react-dropdown-menu": "^2.0",
    "@radix-ui/react-select": "^2.0",
    "@radix-ui/react-tabs": "^1.0",
    "@radix-ui/react-switch": "^1.0",
    "@radix-ui/react-avatar": "^1.0",
    "@radix-ui/react-popover": "^1.0",
    "class-variance-authority": "^0.7"
  },
  "devDependencies": {
    "typescript": "^5",
    "@types/node": "^20",
    "@types/react": "^18",
    "@types/react-dom": "^18",
    "tailwindcss": "^3.4",
    "autoprefixer": "^10",
    "postcss": "^8",
    "eslint": "^8",
    "eslint-config-next": "14.2.0"
  }
}
```

---

## DEVCONTAINER

```json
{
  "name": "VetCare Platform",
  "image": "mcr.microsoft.com/devcontainers/javascript-node:20",
  "features": {
    "ghcr.io/devcontainers/features/github-cli:1": {}
  },
  "customizations": {
    "vscode": {
      "extensions": [
        "dbaeumer.vscode-eslint",
        "esbenp.prettier-vscode",
        "bradlc.vscode-tailwindcss",
        "Prisma.prisma",
        "GitHub.copilot",
        "GitHub.copilot-chat",
        "formulahendry.auto-rename-tag",
        "christian-kohler.path-intellisense"
      ],
      "settings": {
        "editor.formatOnSave": true,
        "editor.defaultFormatter": "esbenp.prettier-vscode",
        "typescript.preferences.importModuleSpecifier": "non-relative"
      }
    }
  },
  "postCreateCommand": "npm install",
  "forwardPorts": [3000],
  "portsAttributes": {
    "3000": { "label": "Next.js Dev", "onAutoForward": "openBrowser" }
  }
}
```

---

## ATURAN TIDAK BOLEH DILANGGAR

```
DATABASE
├── Tidak boleh hapus fisik: profiles, pets, medical_records,
│   transactions, inpatients, stock_mutations
├── Tidak boleh stok negatif (constraint DB + validasi aplikasi)
├── Tidak boleh ubah: no_transaksi, harga_satuan di transaction_items,
│   nama_item di transaction_items setelah lunas
└── Setiap perubahan stok WAJIB ada record di stock_mutations

API
├── Tidak boleh return data tanpa auth check
├── Tidak boleh return data lintas role tanpa cek
├── Tidak boleh skip validasi Zod
├── Tidak boleh expose SUPABASE_SERVICE_ROLE_KEY ke client
└── Tidak boleh proses transaksi POS tanpa lock stok

FRONTEND
├── Tidak boleh simpan role di localStorage (ambil dari session)
├── Tidak boleh render halaman sebelum role terverifikasi
├── Tidak boleh aksi destruktif tanpa confirm dialog
├── Tidak boleh upload file tanpa validasi tipe dan ukuran
└── Tidak boleh print tanpa CSS @media print

UPLOAD
├── Tipe: hanya jpg, png, webp
├── Ukuran: maksimal 5MB per file
├── Jumlah: maksimal 5 foto per log monitoring
└── Path: wajib ikuti konvensi folder storage
```
