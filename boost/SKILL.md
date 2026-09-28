---
name: module-training
description: Use when working with diklat (training) master data — training types (jenis), clusters (rumpun), competency registers, training histories from the Simceria/external API; note the module has no routes or menu and its tables are consumed by module-profile
---

# Training Module

## Status (diverifikasi 2026-09-28)
- Modul aktif di `module.json`, tetapi **tidak terpasang di RBAC** (tidak ada `system_modules`/ability/halaman) dan `routes/api.php` **kosong**. Controller (`Type`, `Cluster`, `Category`, `Register`, `History`, `Dashboard`), resource, dan policy ada tetapi tidak di-route. Frontend hanya `dashboard`.
- **Tabelnya tetap dipakai**, sebagai master data dan penampung riwayat diklat dari integrasi eksternal (lihat "Siapa yang memakai").

## Key Models
| Model | Tabel | Baris di live | Keterangan |
|---|---|---|---|
| `TrainingType` | `training_types` | 26 | Jenis diklat (SiASN) |
| `TrainingCluster` | `training_clusters` | 71 | Rumpun diklat (SiASN) |
| `TrainingRegister` | `training_registers` | 508 | Daftar kompetensi (SKJ) |
| `TrainingHistory` | `training_histories` | 279 | Riwayat diklat pegawai yang masuk lewat API eksternal (`storeFromApi`) |
| `TrainingCategory` | `training_categories` | 0 | Belum dipakai |

Master diisi dari `database/masters/data-seeder.xlsx` (`Imports/DataImport`, `TypeImport`, `ClusterImport`, `RegisterImport`).

## Database
- Connection: `platform` (PostgreSQL)
- Table prefix: `training_*`
- Migrations: 5 files (2025-10-17)

## API Routes (`/training/api`)
0 route.

## Siapa yang memakai
- **module-profile — API eksternal `/api/v2`** (`routes/external.php`, `ApiController`): `GET training-cluster`, `GET training-register` (combo master), `POST training/{profileBiodata}` (`storeTraining` → `TrainingHistory::storeFromApi`; validasi `diklat_jenis` ∈ `training_types.name`, `diklat_rumpun` ∈ `training_clusters.name`, `diklat_kompetensi` ∈ `training_registers.name`, sertifikat PDF ≤ 2 MB). Diduga dipakai sistem diklat eksternal "Simceria".
- **module-profile — admin**: `resource biodata.simceria` + `POST biodata/{id}/simceria/{trainingHistory}/validate` (`BiodataSimceriaController`) untuk melihat & memvalidasi `training_histories`.
- `ProfileBiodata` punya relasi ke `TrainingHistory`.
- **Bukan** dipakai module-talentpool (tabelnya sendiri: `talentpool_trackof_trainings`) dan module-history (disabled).

## Keamanan API eksternal `/api/v2` (module-profile)
Sejak 2026-09-28 group `/api/v2` memakai middleware `integration`: hanya user ber-role `thirdparty` yang memanggil dengan token Sanctum (bukan sesi) yang diterima; `validate-credentials` di-throttle 10/menit. Sebelumnya cukup `auth:sanctum`, sehingga setiap pegawai bisa mencatat diklat atas nama orang lain, membaca profil siapa pun, dan menebak password akun lain. Detail, daftar klien, dan cara menerbitkan token: `modules/module-profile/boost/SKILL.md` → "API eksternal `/api/v2`".

## Paired Module
Tidak ada paired `my*` module.
