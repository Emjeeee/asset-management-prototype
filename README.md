# SSAMS — Interactive Prototype (Fase 1)

Prototipe interaktif System Support Asset Management System. Single-file vanilla HTML/CSS/JS, semua data **in-memory** (reset saat refresh) — tanpa backend, database, atau autentikasi nyata.

## Cara menjalankan

Buka [index.html](index.html) langsung di browser (double-click, atau drag ke tab browser). Tidak perlu install apa pun, build step, atau server.

> Butuh koneksi internet untuk memuat font Google (IBM Plex Sans/Mono); tanpa internet, app tetap jalan dengan font fallback sistem.

## Peta fitur

| Modul | Role yang bisa akses | Isi |
|---|---|---|
| **Dashboard** | admin, at, sa | KPI cards, chart requests/bulan, recent activity, shortcut "needing attention" |
| **Requests** | admin, at, sa | List + search/filter, form New Request (dengan approval-route preview & validasi inline), detail request |
| **Approvals** | semua role | Queue "Your turn" per role + History, detail dengan visualisasi rantai + Approve/Reject/Request Revision |
| **Settings** | admin, at | Master data read-mostly: Outlets, Asset Types, Approval Flow, Users & Roles |

## Cara demo alur inti

1. Klik avatar/nama di kanan atas topbar → **Role switcher** untuk ganti peran (menu & akses berubah sesuai RBAC).
2. Sebagai `admin`/`at`/`sa` → buka **Requests → New request**, isi form (type, outlet, quantity, priority), submit. Item baru langsung muncul di list Requests dan KPI Dashboard.
3. Ganti role ke `ss` (Sales Supervisor) → buka **Approvals**, request baru muncul di tab "Your turn" → Approve / Reject / Request revision. Rantai bergerak maju (SS → RSM → GRSM → Sales Admin → Asset Team); ulangi ganti role mengikuti rantai untuk mendemokan approval berjenjang sampai status **Completed**.
4. Toggle **theme** (light/dark) di topbar — semua styling berbasis token, jadi kontras tetap terjaga di kedua mode.

## Arsitektur

- Semua styling lewat **design tokens** (CSS custom properties) di bagian atas file — lihat [../docs/design-tokens.md](../docs/design-tokens.md). Ganti tema = ganti token, bukan menyunting tiap komponen.
- State aplikasi (`state`, `REQUESTS`) disimpan di variabel JS module-level, dirender ulang lewat `render()` setiap ada perubahan (bukan framework reaktif — vanilla `innerHTML` re-render, cukup untuk skala prototipe ini).
- Komponen reusable: Button, Input/Select (+error), Tag/StatusPill, Table, Card, **ApprovalChain** (`chainFull`/`chainMini`), Timeline, Tabs, Toast, Skeleton loader, RoleSwitcher, ThemeToggle.

## Batasan yang disengaja (sesuai scope Fase 1)

- Tidak ada server, database, ORM, API nyata, atau autentikasi nyata — role switcher hanya simulasi UI, bukan login.
- Settings bersifat read-mostly (tombol Add/Edit ada di UI namun belum menyimpan perubahan di Fase 1).
- Data reset setiap refresh browser (state in-memory, sesuai spesifikasi).
