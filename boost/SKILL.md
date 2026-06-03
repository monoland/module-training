---
name: module-training
description: Use when working with diklat (training/education program) — training category, cluster, type, registration, or training history management
---

# Training Module

## Domain
Diklat (Pendidikan dan Pelatihan) — pengelolaan master data pelatihan/diklat pegawai: jenis diklat, klaster, kategori, pendaftaran, dan riwayat diklat.

## Key Models
| Model | Keterangan |
|---|---|
| `TrainingCategory` | Kategori diklat |
| `TrainingCluster` | Klaster/rumpun diklat |
| `TrainingType` | Jenis diklat |
| `TrainingRegister` | Pendaftaran diklat |
| `TrainingHistory` | Riwayat diklat pegawai |

## Database
- Connection: `platform` (PostgreSQL)
- Table prefix: `training_*`
- Migrations: 5 files

## API Routes (`/training/api`)
0 routes terdaftar saat ini.

## Notes
- Tidak ada paired `my*` module
- Data training histories juga di-referensikan dari `module-profile` (ProfileTraining) dan `module-history` (HistoryTraining)
- `module-talentpool` menggunakan `TalentpoolTrackTraining` yang menarik dari tabel ini
