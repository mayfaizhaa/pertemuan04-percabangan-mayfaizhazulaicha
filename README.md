# Tugas Pertemuan 4

> **Mata Kuliah:** Pemrograman Basis Data  
> **Dosen:** Abdul Malik, S.Kom., M.Cs.  
> **Nama:** Mayfaizha Zulaicha Latuconsina  
> **NIM:** IK2411041  
> **Program Studi:** Informatika  
> **Tahun Akademik:** 2025/2026

---

## 📝 Deskripsi

Proyek ini berisi sebuah **stored procedure** MySQL bernama `cek_predikat_mahasiswa` yang dibuat sebagai implementasi struktur percabangan **IF-THEN-ELSE** pada MySQL. Procedure ini digunakan untuk:
- Menentukan **predikat mahasiswa** berdasarkan nilai yang diinputkan
- Menentukan **status kelulusan** (Lulus / Tidak Lulus)

---

## 🗂️ Struktur Data

### Parameter Input
| Parameter | Tipe | Keterangan |
|-----------|------|------------|
| `p_nilai` | INT | Nilai mahasiswa yang dimasukkan |

### Variabel Lokal
| Variabel | Tipe | Keterangan |
|----------|------|------------|
| `v_predikat` | VARCHAR(50) | Menyimpan hasil predikat berdasarkan nilai |
| `v_status` | VARCHAR(20) | Menyimpan status kelulusan |

> Data diproses langsung di dalam procedure menggunakan logika percabangan **tanpa melibatkan tabel database**.

---

## 🔀 Logika Percabangan

### Penentuan Predikat
| Rentang Nilai | Predikat |
|---------------|----------|
| ≥ 90 | Sangat Memuaskan |
| ≥ 80 | Memuaskan |
| ≥ 70 | Baik |
| ≥ 60 | Cukup |
| < 60 | Kurang |

### Penentuan Status Kelulusan
| Kondisi | Status |
|---------|--------|
| Nilai ≥ 70 | Lulus |
| Nilai < 70 | Tidak Lulus |

---

## 💾 Kode SQL

```sql
DELIMITER //

CREATE PROCEDURE cek_predikat_mahasiswa(IN p_nilai INT)
BEGIN
    DECLARE v_predikat VARCHAR(50);
    DECLARE v_status VARCHAR(20);

    -- Penentuan Predikat
    IF p_nilai >= 90 THEN
        SET v_predikat = 'Sangat Memuaskan';
    ELSEIF p_nilai >= 80 THEN
        SET v_predikat = 'Memuaskan';
    ELSEIF p_nilai >= 70 THEN
        SET v_predikat = 'Baik';
    ELSEIF p_nilai >= 60 THEN
        SET v_predikat = 'Cukup';
    ELSE
        SET v_predikat = 'Kurang';
    END IF;

    -- Penentuan Status Kelulusan
    IF p_nilai >= 70 THEN
        SET v_status = 'Lulus';
    ELSE
        SET v_status = 'Tidak Lulus';
    END IF;

    SELECT
        p_nilai AS nilai,
        v_predikat AS predikat,
        v_status AS status;
END //

DELIMITER ;
```

---

## ▶️ Cara Menjalankan

### 1. Buat Procedure
Jalankan kode SQL di atas pada database yang diinginkan (contoh: `db_percabangan`).

### 2. Eksekusi Procedure
```sql
-- Format pemanggilan
CALL cek_predikat_mahasiswa(nilai_input);

-- Contoh
CALL cek_predikat_mahasiswa(95);
CALL cek_predikat_mahasiswa(50);
```

---

## 🧪 Hasil Pengujian

### Uji 1 — Nilai Tinggi
```sql
CALL cek_predikat_mahasiswa(95);
```
| nilai | predikat | status |
|-------|----------|--------|
| 95 | Sangat Memuaskan | Lulus |

**Analisis:** Nilai 95 berada pada rentang 90–100 → predikat **"Sangat Memuaskan"**. Karena nilai ≥ 70, status adalah **"Lulus"**.

---

### Uji 2 — Nilai Rendah
```sql
CALL cek_predikat_mahasiswa(50);
```
| nilai | predikat | status |
|-------|----------|--------|
| 50 | Kurang | Tidak Lulus |

**Analisis:** Nilai 50 berada pada rentang 1–59 → predikat **"Kurang"**. Karena nilai < 70, status adalah **"Tidak Lulus"**.

---

## ✅ Kesimpulan

Implementasi struktur percabangan **IF-THEN-ELSE** pada MySQL melalui stored procedure `cek_predikat_mahasiswa` telah berhasil dijalankan. Sistem mampu mengotomatisasi penentuan predikat dan status kelulusan secara akurat sesuai dengan kondisi logika yang ditetapkan.

Hasil pengujian menunjukkan validitas kode yang tinggi:
- Input nilai **95** → Status **Lulus**, Predikat **Sangat Memuaskan** ✅
- Input nilai **50** → Status **Tidak Lulus**, Predikat **Kurang** ✅

Penggunaan *stored procedure* terbukti efektif untuk menangani logika pemrosesan data langsung di sisi server basis data.

---

