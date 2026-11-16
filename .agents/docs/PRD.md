# PRODUCT REQUIREMENTS DOCUMENT
# Badmin Count

**Version:** 1.0  
**Status:** Business Rules Approved — MVP Specification  
**Product:** Badmin Count  
**Platform:** Responsive Web Application  
**Design Approach:** Mobile-First  
**Language:** Bahasa Indonesia  
**Currency:** IDR

---

## 1. Product Overview

Badmin Count adalah aplikasi berbasis web untuk membantu admin badminton mencatat pertandingan dan menghitung tagihan setiap pemain secara otomatis.

Sistem menggunakan dua komponen tarif:

1. Flat court fee — nominal tetap per pemain yang terdaftar dalam sesi.
2. Shuttlecock fee — tarif per kok yang dibebankan kepada setiap pemain yang mengikuti match.

Aplikasi harus mengutamakan kecepatan input, kesederhanaan antarmuka, dan transparansi tagihan.

### 1.1 Problem Statement

Admin badminton biasanya menghadapi beberapa masalah:

- Kesulitan mengingat siapa saja yang mengikuti setiap match.
- Kesulitan menghitung berapa kok yang digunakan masing-masing pemain.
- Penghitungan tagihan dilakukan manual.
- Tagihan berbeda karena partisipasi match berbeda.
- Proses penagihan setelah sesi memakan waktu.

Badmin Count harus menyelesaikan masalah tersebut tanpa menambah kompleksitas administrasi.

### 1.2 Product Goals

- Menyederhanakan pencatatan sesi badminton.
- Mengotomatisasi perhitungan biaya lapangan.
- Mengotomatisasi perhitungan biaya kok berdasarkan partisipasi match.
- Meminimalkan jumlah input dan navigasi.
- Menampilkan tagihan secara real-time.
- Mempermudah pencatatan pembayaran.
- Memudahkan admin membagikan rincian pembayaran.

### 1.3 Non-Goals

MVP tidak mencakup:

- Skor pertandingan.
- Set atau round.
- Peringkat pemain.
- Statistik kemenangan.
- Jadwal turnamen.
- Reservasi lapangan.
- Payment gateway.
- Pembayaran sebagian.
- Akun atau login individual pemain.
- Pembagian biaya kok aktual secara proporsional.

---

## 2. Confirmed Business Decisions

Seluruh keputusan berikut bersifat final untuk MVP.

### Decision 01 — Shuttlecock Billing

Setiap pemain yang mengikuti suatu match dikenakan biaya berdasarkan jumlah kok yang digunakan dalam match tersebut.

Jumlah kok tidak dibagi dengan jumlah peserta.

Contoh:

Match 1:
- Participants: Andi, Budi, Candra, Dimas.
- Shuttlecock used: 2.
- Shuttlecock tariff: Rp3.000.

Setiap pemain dikenakan:

`2 × Rp3.000 = Rp6.000`

Total tagihan kok dari match tersebut:

`4 × Rp6.000 = Rp24.000`

Jumlah kok fisik yang digunakan tetap 2.

### Decision 02 — Court Fee

Admin memasukkan nominal biaya lapangan per pemain.

Contoh:

`court_fee_per_player = Rp20.000`

Jika sesi berisi enam pemain:

`total_court_bill = 6 × Rp20.000`

Sistem tidak perlu meminta total harga sewa lapangan atau membaginya dengan jumlah pemain.

### Decision 03 — Late Arrival

Pemain yang datang terlambat tetap dikenakan biaya lapangan penuh.

Waktu kedatangan tidak memengaruhi nominal lapangan.

### Decision 04 — Payment Tracking

Status pembayaran:

- Unpaid
- Paid

Tidak ada pembayaran parsial.

Admin dapat mengubah status pembayaran secara manual.

---

## 3. User Roles

### Administrator

Admin bertanggung jawab atas:

- Membuat sesi.
- Menentukan tarif lapangan.
- Menentukan tarif kok.
- Mendaftarkan peserta.
- Membuat match.
- Memilih peserta match.
- Mencatat jumlah kok.
- Mengoreksi match.
- Melihat ringkasan biaya.
- Menandai pembayaran.
- Menutup sesi.

### Player

Pemain merupakan data yang dikelola admin.

Pemain tidak memerlukan login pada MVP.

---

## 4. Core User Flow

### Step 1 — Create Session

Admin membuat sesi baru.

Required inputs:

- Session date.
- Court fee per player.
- Shuttlecock tariff.

Optional inputs:

- Session name.
- Location.
- Notes.

Default session date: Today.

Session status: Active.

Setelah sesi dibuat, arahkan admin ke halaman pengelolaan sesi.

### Step 2 — Add Players

Admin mendaftarkan pemain yang hadir.

Requirements:

- Input nama pemain.
- Dapat menambahkan banyak pemain sekaligus.
- Mendukung edit nama.
- Mendukung penghapusan pemain yang belum memiliki match.
- Mencegah duplikasi peserta dalam sesi.
- Menampilkan jumlah pemain.

Setiap pemain yang berhasil ditambahkan langsung mendapatkan tagihan lapangan.

Formula:

`player_court_bill = session.court_fee_per_player`

Tidak bergantung pada jumlah match.

### Step 3 — Create Match

Admin menekan tombol "Add Match".

Form terdiri dari:

1. Player selection.
2. Shuttlecock quantity.
3. Save action.

#### Player Selection

- Tampilkan semua pemain terdaftar.
- Gunakan selectable chips atau checkbox.
- Pemain bisa dipilih dengan satu tap.
- Tampilkan status selected dengan jelas.
- Pemain boleh mengikuti banyak match.

#### Shuttlecock Quantity

Default: 1

Minimum: 0

Input: Integer

Interaction:

`[-] 1 [+]`

Input angka manual tetap didukung.

#### Match Number

Dibuat otomatis berdasarkan urutan.

Example:

Match 1, Match 2, Match 3.

Tidak ada skor, set, round, atau winner.

#### Saving Match

Setelah disimpan:

- Match ditambahkan ke daftar.
- Tagihan pemain diperbarui.
- Total kok diperbarui.
- Tidak perlu reload halaman.
- Form dapat segera digunakan untuk match berikutnya.

### Step 4 — Live Billing

Sistem menampilkan:

- Player name.
- Matches played.
- Billable shuttlecock units.
- Shuttlecock bill.
- Court bill.
- Total bill.
- Payment status.

Semua perhitungan diperbarui setiap kali data match berubah.

### Step 5 — Settlement

Admin melihat seluruh tagihan.

Admin dapat:

- Menandai Paid.
- Mengembalikan status menjadi Unpaid.
- Menyalin rincian pembayaran.
- Melihat total tagihan.
- Melihat total pembayaran diterima.
- Melihat outstanding balance.
- Menutup sesi.

---

## 5. Business Rules

### BR-001 — Flat Court Fee

Setiap peserta sesi dikenakan biaya lapangan yang sama.

Formula:

`player_court_bill = court_fee_per_player`

`session_court_bill = player_count × court_fee_per_player`

Jumlah match tidak memengaruhi biaya lapangan.

Pemain yang tidak mengikuti match tetap membayar lapangan.

### BR-002 — Shuttlecock Fee Per Match

Setiap peserta match dikenakan biaya sesuai jumlah kok yang digunakan dalam match.

Formula:

`player_match_shuttle_bill = match.shuttle_quantity × session.shuttle_unit_price`

Total tagihan kok per match:

`match_shuttle_bill = participant_count × match.shuttle_quantity × session.shuttle_unit_price`

Tarif kok harus sama untuk seluruh peserta match.

### BR-003 — Cumulative Shuttlecock Units

Setiap pemain mengumpulkan unit kok berdasarkan seluruh match yang diikutinya.

Formula:

`player_billable_shuttle_units = SUM(shuttle_quantity from player's matches)`

Contoh:

Andi mengikuti:

- Match 1: 1 kok.
- Match 2: 2 kok.
- Match 4: 1 kok.

Total unit kok yang ditagihkan kepada Andi:

`1 + 2 + 1 = 4`

Jika tarif Rp3.000:

`4 × Rp3.000 = Rp12.000`

### BR-004 — Total Player Bill

Formula:

`player_total_bill = player_court_bill + player_shuttle_bill`

`player_shuttle_bill = player_billable_shuttle_units × shuttle_unit_price`

### BR-005 — Session Revenue

Total seluruh tagihan:

`session_total_bill = SUM(player_total_bill)`

Alternatif verifikasi:

`session_total_bill = session_court_bill + session_shuttle_bill`

Di mana:

`session_shuttle_bill = SUM(match_participant_count × match_shuttle_quantity × shuttle_unit_price)`

Kedua pendekatan harus menghasilkan nilai identik.

### BR-006 — Physical Shuttlecock Usage

Sistem harus membedakan antara:

**Physical shuttlecock usage**

`physical_shuttles = SUM(matches.shuttle_quantity)`

**Billable shuttlecock units**

`billable_units = SUM(matches.shuttle_quantity × match_participant_count)`

Contoh:

Satu match dengan empat pemain menggunakan dua kok:

- Physical shuttles: 2.
- Billable units: 8.

Kedua metrik memiliki fungsi berbeda dan tidak boleh disamakan.

### BR-007 — Payment Status

Nilai status:

- unpaid
- paid

Default: unpaid.

Ketika status Paid:

- Simpan waktu pembayaran.
- Total received bertambah sebesar tagihan pemain saat ini.

Ketika status Unpaid:

- Hapus penanda waktu pembayaran.
- Tagihan pemain kembali dihitung sebagai outstanding.

Perhitungan:

`total_received = SUM(player_total_bill WHERE payment_status = paid)`

`total_outstanding = SUM(player_total_bill WHERE payment_status = unpaid)`

Jika tagihan pemain yang sudah Paid berubah, status harus otomatis menjadi Unpaid agar tidak terjadi pelunasan palsu.

Tampilkan notifikasi bahwa tagihan berubah dan perlu diverifikasi ulang.

MVP tidak mendukung pencatatan pembayaran parsial atau saldo lebih.

### BR-008 — Session Lifecycle

Session statuses:

- active
- closed

Active:

- Pemain dapat dikelola.
- Match dapat ditambahkan atau diedit.
- Tagihan dapat berubah.

Closed:

- Match tidak dapat diubah.
- Tarif sesi tidak dapat diubah.
- Pemain tidak dapat ditambah atau dihapus.
- Status pembayaran tetap dapat diubah.
- Summary dapat dilihat dan disalin.

Untuk melakukan koreksi permainan, admin harus membuka kembali sesi.

### BR-009 — Data Integrity

- Monetary fields wajib menggunakan integer IDR.
- Nilai uang tidak boleh negatif.
- Shuttlecock quantity wajib integer non-negatif.
- Match wajib memiliki minimal satu peserta.
- Pemain tidak boleh tercatat dua kali pada match yang sama.
- Pemain hanya boleh masuk match pada sesi tempat dirinya terdaftar.
- Penghapusan match memicu perhitungan ulang.
- Penghapusan peserta yang memiliki riwayat match harus ditolak sampai riwayatnya diselesaikan.
- Perubahan tarif sesi memicu perhitungan ulang seluruh tagihan.
- Perubahan tarif atau match yang memengaruhi pemain Paid harus membatalkan status Paid yang terdampak.

### BR-010 — Match Number

Nomor match dibuat otomatis.

Nomor match merupakan identifier tampilan, bukan dasar perhitungan biaya.

Tidak diperlukan penghitungan round atau set.

---

## 6. Database Design

Gunakan relational database dengan struktur berikut.

### 6.1 sessions

| Column | Type | Description |
|---|---|---|
| id | bigint | Primary key |
| name | varchar nullable | Session name |
| session_date | date | Session date |
| location | varchar nullable | Location |
| court_fee_per_player | unsigned bigint | Flat court fee |
| shuttle_unit_price | unsigned bigint | Shuttlecock tariff |
| status | enum/string | active, closed |
| notes | text nullable | Notes |
| created_at | timestamp | Creation time |
| updated_at | timestamp | Update time |

### 6.2 players

| Column | Type | Description |
|---|---|---|
| id | bigint | Primary key |
| name | varchar | Player name |
| created_at | timestamp | Creation time |
| updated_at | timestamp | Update time |

Players dapat digunakan kembali di sesi lain.

Nama pemain tidak wajib unik secara global.

### 6.3 session_players

| Column | Type | Description |
|---|---|---|
| id | bigint | Primary key |
| session_id | bigint | FK sessions |
| player_id | bigint | FK players |
| payment_status | string | unpaid / paid |
| paid_at | timestamp nullable | Payment timestamp |
| created_at | timestamp | Creation time |
| updated_at | timestamp | Update time |

Constraints:

- Unique `(session_id, player_id)`.
- Default payment_status = unpaid.
- Status harus divalidasi menggunakan nilai yang diizinkan.

### 6.4 matches

| Column | Type | Description |
|---|---|---|
| id | bigint | Primary key |
| session_id | bigint | FK sessions |
| match_number | unsigned int | Display order |
| shuttle_quantity | unsigned int | Physical shuttles used |
| created_at | timestamp | Creation time |
| updated_at | timestamp | Update time |

Constraints:

- Unique `(session_id, match_number)`.
- `shuttle_quantity >= 0`.

### 6.5 match_players

| Column | Type | Description |
|---|---|---|
| id | bigint | Primary key |
| match_id | bigint | FK matches |
| session_player_id | bigint | FK session_players |

Constraints:

- Unique `(match_id, session_player_id)`.
- The referenced session player must belong to the same session as the match.

### 6.6 Derived Financial Data

Jangan membuat tabel pembayaran untuk MVP jika hanya menggunakan status Paid/Unpaid.

Total tagihan dihitung melalui service layer.

Suggested calculated attributes:

- `court_bill`
- `billable_shuttle_units`
- `shuttle_bill`
- `total_bill`
- `matches_played`

Suggested session aggregates:

- `total_players`
- `total_matches`
- `physical_shuttle_quantity`
- `total_billable_shuttle_units`
- `total_court_bill`
- `total_shuttle_bill`
- `total_expected`
- `total_received`
- `total_outstanding`

Financial calculations should use a single authoritative billing service.

---

## 7. Functional Requirements

| ID | Requirement | Priority |
|---|---|---|
| FR-001 | Create session | Must |
| FR-002 | Configure flat court fee | Must |
| FR-003 | Configure shuttlecock tariff | Must |
| FR-004 | Register multiple players | Must |
| FR-005 | Display session players | Must |
| FR-006 | Edit/remove players with integrity rules | Must |
| FR-007 | Automatically calculate flat court bill | Must |
| FR-008 | Create match | Must |
| FR-009 | Select participants | Must |
| FR-010 | Input shuttlecock quantity | Must |
| FR-011 | Edit/delete match | Must |
| FR-012 | Calculate per-player shuttle bill | Must |
| FR-013 | Calculate cumulative billing | Must |
| FR-014 | Live billing summary | Must |
| FR-015 | Per-player breakdown | Must |
| FR-016 | Set Paid/Unpaid | Must |
| FR-017 | Display received/outstanding | Must |
| FR-018 | Copy WhatsApp-friendly billing text | Must |
| FR-019 | Close/reopen session | Must |
| FR-020 | View past sessions | Should |
| FR-021 | Reuse previous player names | Should |
| FR-022 | Duplicate previous session configuration | Could |
| FR-023 | Export summary PDF/image | Future |

---

## 8. UI/UX Specification

### 8.1 General Design

The UI must be:

- Mobile-first.
- Responsive.
- Minimal.
- Fast to operate.
- Easy to read during badminton sessions.
- Optimized for repeated match recording.

Avoid unnecessary pages, navigation, and decorative elements.

### 8.2 Session Creation

Use a compact form.

Fields:

- Session name (optional).
- Date.
- Court fee per player.
- Shuttlecock tariff.

Primary action:

**Create Session**

### 8.3 Player Registration

Provide:

- Quick-add name field.
- Multi-name input support.
- Registered player list.
- Remove/edit actions.

After registration, immediately show court billing totals.

### 8.4 Match Input

Primary interaction:

**Add Match**

Match form:

- Title: Match N.
- Select player chips.
- Kok counter.
- Save button.

Recommended:

- Open match form as bottom sheet on mobile.
- Keep player selection visually prominent.
- Do not require score or duration.
- Allow quick creation of the next match.

### 8.5 Match List

Each match card displays:

- Match number.
- Player names.
- Physical kok count.
- Edit action.
- Delete action.

Example:

**Match 2**

Andi, Budi, Candra, Dimas

Kok: 2

### 8.6 Billing Summary

Display:

| Player | Match | Kok Units | Court | Kok Bill | Total | Status |
|---|---|---|---|---|---|---|

On mobile:

Use compact billing cards with expandable breakdown.

Important:

`Kok Units` means accumulated billable quantity for the player, not physical shuttlecocks consumed by the session.

### 8.7 Settlement

Display financial summary:

- Expected.
- Received.
- Outstanding.

Enable:

- Paid/Unpaid toggle.
- Copy summary.
- Close session.

### 8.8 UX Requirements

- Avoid full-page refresh during match entry.
- Provide loading and success feedback.
- Prevent double submissions.
- Confirm destructive actions.
- Keep primary actions easily accessible.
- Use Indonesian currency formatting.
- Maintain stable values when switching UI sections.
- Support empty states.

---

## 9. Acceptance Criteria

### AC-001 — Flat Court Fee

Given:

- 6 players.
- Court fee Rp20.000/player.

Expected:

- Each player pays Rp20.000.
- Total court bill Rp120.000.

### AC-002 — No Match Participation

Given:

- A player is registered.
- The player joins zero matches.

Expected:

- Court fee remains Rp20.000.
- Shuttle bill Rp0.
- Total bill Rp20.000.

### AC-003 — One Match

Given:

- 4 players in a match.
- 2 shuttlecocks.
- Shuttle tariff Rp3.000.

Expected:

- Each match participant pays Rp6.000 in shuttle fees.
- Total match shuttle bill Rp24.000.
- Physical shuttlecock usage = 2.
- Billable shuttle units = 8.

### AC-004 — Multiple Matches

Given:

Andi participates in:

- Match 1: 1 shuttlecock.
- Match 2: 2 shuttlecocks.
- Match 3: 1 shuttlecock.

Shuttle tariff: Rp3.000.

Expected:

- Billable units = 4.
- Shuttle bill = Rp12.000.

### AC-005 — Complete Player Bill

Given:

- Court fee = Rp20.000.
- Shuttle bill = Rp12.000.

Expected:

`total_bill = Rp32.000`

### AC-006 — Late Arrival

Given:

- Andi arrives late.
- Flat court fee = Rp20.000.

Expected:

Andi's court bill remains Rp20.000.

### AC-007 — Recalculation

Given:

A match contains two kok.

When:

Admin changes kok quantity to three.

Expected:

Every match participant's bill increases by one unit of the shuttlecock tariff.

### AC-008 — Match Deletion

Given:

A match has existing participants and shuttlecock charges.

When:

The match is deleted.

Expected:

All associated shuttlecock charges disappear from affected player totals.

Court fees remain unchanged.

### AC-009 — Payment

Given:

A player's total bill is Rp32.000 and status is Unpaid.

When:

Admin changes status to Paid.

Expected:

- Paid timestamp is recorded.
- Received total increases by Rp32.000.
- Outstanding decreases by Rp32.000.

### AC-010 — Paid Bill Modification

Given:

A player has Paid status.

When:

A tariff or match modification changes the player's bill.

Expected:

- Payment status returns to Unpaid.
- Paid timestamp is cleared.
- Admin receives a warning.
- Received/outstanding totals are recalculated.

### AC-011 — Session Closure

Given:

Session status is Closed.

Expected:

- Match cannot be edited.
- New match cannot be created.
- Player registration cannot be modified.
- Payment status may still be updated.
- Summary remains accessible.

### AC-012 — Billing Consistency

For every valid session:

`SUM(player_total_bill) = total_court_bill + total_shuttle_bill`

### AC-013 — Zero Shuttlecock

Given:

A match with four participants uses zero kok.

Expected:

- Match can be saved.
- Shuttle bill = Rp0.
- Match participation count still increases.

---

## 10. Non-Functional Requirements

### Performance

- Fast interaction for typical badminton groups.
- Reactive billing updates.
- Avoid unnecessary database queries.
- Efficient aggregation.

### Data Integrity

- Integer monetary arithmetic.
- Database foreign key constraints.
- Unique participant constraints.
- Transaction-safe updates.
- Server-side validation.

### Security

- Administrator-only modification.
- Secure session access.
- Prevent unauthorized session mutation.
- CSRF protection where applicable.
- Input validation.

### Maintainability

- Dedicated billing service.
- Separate calculation logic from presentation.
- Modular session and match components.
- Automated unit tests for billing.

---

## 11. Recommended Development Phases

### Phase 1 — Foundation

Implement:

- Session model.
- Player model.
- Session-player relationship.
- Session creation.
- Player registration.
- Flat court calculation.

### Phase 2 — Match Recording

Implement:

- Match model.
- Match-participant relationship.
- Add/edit/delete match.
- Shuttlecock quantity.
- Per-match billing logic.

### Phase 3 — Financial Summary

Implement:

- Aggregated billing.
- Per-player breakdown.
- Session totals.
- Payment statuses.
- Settlement summary.
- Copyable bills.

### Phase 4 — Completion

Implement:

- Close/reopen session.
- Historical session list.
- Validation.
- Automated billing tests.
- Responsive polishing.

---

## 12. AI Agent Implementation Instructions

The AI Agent MUST follow these rules:

1. Treat this PRD as the authoritative MVP specification.
2. Do not implement actual-cost sharing.
3. Never divide shuttlecock charges by match participant count.
4. Do not divide court fees by session player count.
5. Court fee is a fixed amount charged to each registered player.
6. Every match participant is charged for the full shuttlecock quantity of that match, multiplied by the session's unit tariff.
7. Calculate each player's shuttlecock charges cumulatively across matches.
8. Distinguish physical shuttlecock consumption from billable shuttlecock units.
9. Do not implement scores, rounds, rankings, or tournament functionality.
10. Use a single authoritative billing service.
11. Do not duplicate financial logic across multiple layers.
12. Keep all financial calculations in integer IDR.
13. Preserve financial consistency when matches are updated or deleted.
14. Use Paid/Unpaid only. Do not create partial payment functionality.
15. Optimize UX to minimize repeated inputs.
16. Prioritize a single-session workspace layout.
17. Write automated tests for all business rules.
18. Do not introduce additional scope without approval.
19. Adapt code structure to the selected technology stack without changing business logic.
20. If an implementation decision is unspecified, choose the simplest maintainable solution that does not alter confirmed business rules.

---

## 13. Definition of Done

The MVP is complete when:

- Admin can create a session.
- Admin can register players.
- Flat court fee is automatically assigned.
- Admin can record multiple matches.
- Participants can be selected using checkboxes or chips.
- Shuttlecock quantity can be recorded.
- Every player's shuttlecock bill accumulates correctly.
- Total bill is automatically calculated.
- Every player can be marked Paid or Unpaid.
- Admin can view expected, received, and outstanding amounts.
- Admin can copy a payment summary.
- Admin can close and reopen sessions.
- All financial acceptance tests pass.
- The entire workflow works comfortably on mobile.

**Document Status: PRD v1.0 — Approved Business Rules, Ready for MVP Development.**