# Instagram Menfess

## Deskripsi Proyek

Instagram Menfess adalah sistem otomatis untuk mengelola akun menfess Instagram. Pengguna mengirim pesan menfess melalui DM ke akun Instagram, kemudian pesan diterima oleh backend melalui Instagram/Meta API dan webhook. Pesan disimpan, dimoderasi, dan dapat disetujui oleh admin sebelum diubah menjadi konten gambar dan dipublikasikan kembali ke feed Instagram secara otomatis.

### Tujuan Utama

- Menerima DM menfess melalui Instagram Messaging API.
- Menyimpan dan mengelola menfess melalui backend.
- Menyediakan dashboard admin untuk moderasi.
- Mengubah teks menfess menjadi gambar dengan template.
- Mempublikasikan menfess yang disetujui ke Instagram.
- Menjaga keamanan token, webhook, dan data pengguna.
- Menyediakan fondasi yang dapat dikembangkan menjadi sistem menfess publik.

### Target Alur Sistem

User DM Instagram
→ Meta/Instagram API
→ Webhook
→ Rails Backend
→ Database
→ Moderasi/Admin
→ Generate Gambar
→ Instagram Content Publishing API
→ Feed Instagram

---

# Milestone

## Fase 0 — Persiapan Proyek

**Tujuan:** Menyiapkan seluruh lingkungan pengembangan.

Checklist:
- [ ] Buat akun Instagram khusus menfess.
- [ ] Ubah akun menjadi Instagram Professional.
- [ ] Buat akun Meta for Developers.
- [ ] Buat Meta App.
- [ ] Install Git.
- [ ] Install Ruby.
- [ ] Install Rails.
- [ ] Install PostgreSQL.
- [ ] Buat repository GitHub.
- [ ] Clone dan jalankan `instagram_connect`.
- [ ] Pastikan Rails dapat berjalan secara lokal.

**Target selesai:** Rails application dapat dibuka dan berjalan di browser.

---

## Fase 1 — Instagram dan Meta App

**Tujuan:** Menghubungkan akun Instagram dengan Meta App.

Checklist:
- [ ] Konfigurasi Instagram API.
- [ ] Tentukan authentication flow.
- [ ] Konfigurasi App ID.
- [ ] Konfigurasi App Secret.
- [ ] Konfigurasi OAuth Redirect URI.
- [ ] Siapkan permission Instagram yang diperlukan.
- [ ] Jalankan OAuth.
- [ ] Hubungkan akun Instagram.
- [ ] Dapatkan access token.
- [ ] Simpan access token secara aman.

**Target selesai:** Rails berhasil melakukan autentikasi dan mendapatkan akses ke akun Instagram.

---

## Fase 2 — Webhook

**Tujuan:** Membuat Meta dapat mengirim event DM ke backend Rails.

Checklist:
- [ ] Buat endpoint webhook Rails.
- [ ] Siapkan server/tunnel dengan HTTPS publik.
- [ ] Tentukan Callback URL.
- [ ] Buat Verify Token.
- [ ] Konfigurasi webhook di Meta.
- [ ] Lakukan webhook verification.
- [ ] Pastikan `hub.challenge` berhasil dikembalikan.
- [ ] Subscribe event `messages`.
- [ ] Test pengiriman DM.
- [ ] Pastikan POST webhook diterima Rails.
- [ ] Verifikasi `X-Hub-Signature-256`.
- [ ] Parse payload DM.

**Target selesai:**

Instagram DM
→ Meta
→ Webhook
→ Rails

dan isi pesan DM dapat terlihat di Rails/log.

---

## Fase 3 — Database Menfess

**Tujuan:** Menyimpan DM sebagai data menfess yang terstruktur.

Checklist:
- [ ] Setup PostgreSQL.
- [ ] Buat model `Menfess`.
- [ ] Buat model `InstagramAccount` jika diperlukan.
- [ ] Buat model `WebhookEvent` jika diperlukan.
- [ ] Simpan isi DM.
- [ ] Simpan sender ID.
- [ ] Simpan Instagram message ID.
- [ ] Tambahkan status menfess.
- [ ] Tambahkan timestamp.
- [ ] Cegah duplicate webhook/event.
- [ ] Test beberapa DM.

Status awal:

`pending → approved → posted`

atau

`pending → rejected`

**Target selesai:** DM yang masuk otomatis tersimpan sebagai menfess dengan status `pending`.

---

## Fase 4 — Admin Dashboard

**Tujuan:** Memberikan kontrol kepada admin untuk memoderasi menfess.

Stack frontend MVP:
- Rails
- Hotwire
- Turbo
- Stimulus

Checklist:
- [ ] Buat autentikasi admin.
- [ ] Buat dashboard.
- [ ] Tampilkan daftar menfess pending.
- [ ] Tampilkan detail menfess.
- [ ] Approve menfess.
- [ ] Reject menfess.
- [ ] Hapus menfess.
- [ ] Filter berdasarkan status.
- [ ] Tambahkan pagination.

**Target selesai:**

DM
→ Pending
→ Dashboard Admin
→ Approve/Reject

---

## Fase 5 — Generator Postingan

**Tujuan:** Mengubah teks menfess menjadi gambar siap posting.

Checklist:
- [ ] Buat desain template.
- [ ] Tentukan ukuran konten Instagram.
- [ ] Generate gambar dari teks.
- [ ] Handle teks panjang.
- [ ] Handle line break.
- [ ] Handle emoji.
- [ ] Tambahkan nomor menfess.
- [ ] Tambahkan identitas akun menfess.
- [ ] Simpan gambar sementara.
- [ ] Test berbagai panjang pesan.

**Target selesai:**

Menfess
→ Image Generator
→ Gambar siap dipublikasikan.

---

## Fase 6 — Instagram Content Publishing

**Tujuan:** Mengirim gambar menfess yang telah disetujui ke feed Instagram.

Checklist:
- [ ] Aktifkan permission content publishing.
- [ ] Pastikan access token memiliki permission yang diperlukan.
- [ ] Buat media container.
- [ ] Publish media.
- [ ] Ambil `media_id`.
- [ ] Simpan `media_id`.
- [ ] Update status menjadi `posted`.
- [ ] Buat error handling.
- [ ] Tangani token/access error.
- [ ] Test posting.

**Target selesai:**

Admin Approve
→ Generate Image
→ Instagram API
→ Published Post

---

## Fase 7 — Background Job

**Tujuan:** Memindahkan proses berat ke background sehingga webhook dapat merespons dengan cepat.

Stack:
- Active Job
- Solid Queue

Checklist:
- [ ] Setup Solid Queue.
- [ ] Buat job pemrosesan webhook jika diperlukan.
- [ ] Buat `GenerateMenfessImageJob`.
- [ ] Buat `PublishMenfessJob`.
- [ ] Tambahkan retry.
- [ ] Tambahkan logging.
- [ ] Tambahkan error handling.
- [ ] Cegah double-posting.

**Target selesai:** Proses generate dan publish berjalan asynchronous tanpa membuat webhook menunggu terlalu lama.

---

## Fase 8 — Moderation

**Tujuan:** Mengurangi spam dan konten yang tidak layak dipublikasikan.

Checklist:
- [ ] Filter spam.
- [ ] Filter link yang tidak diperlukan.
- [ ] Filter kata tertentu.
- [ ] Batasi panjang pesan.
- [ ] Rate limiting.
- [ ] Duplicate detection.
- [ ] Flag konten untuk review.
- [ ] Admin override.

AI moderation dapat ditambahkan setelah sistem dasar stabil.

**Target selesai:** Menfess dapat disaring sebelum masuk tahap publikasi.

---

## Fase 9 — Security

**Tujuan:** Mengamankan aplikasi sebelum digunakan publik.

Checklist:
- [ ] HTTPS.
- [ ] Secret tidak masuk Git.
- [ ] Access token disimpan secara aman/terenkripsi.
- [ ] Webhook signature verification.
- [ ] Admin authentication.
- [ ] Rate limiting.
- [ ] Input sanitization.
- [ ] SQL injection protection.
- [ ] Security logging.
- [ ] Database backup.
- [ ] Error monitoring.

**Target selesai:** Tidak ada credential sensitif yang terekspos dan endpoint utama memiliki perlindungan dasar.

---

## Fase 10 — Production Deployment

**Tujuan:** Menjalankan sistem pada server yang dapat diakses Meta dan digunakan secara stabil.

Stack awal:

- Linux VPS
- Nginx
- Ruby on Rails
- PostgreSQL
- Solid Queue
- Cloudflare/HTTPS
- Instagram Graph/API

Checklist:
- [ ] Siapkan VPS/server.
- [ ] Siapkan domain.
- [ ] Konfigurasi DNS.
- [ ] Konfigurasi HTTPS.
- [ ] Konfigurasi Nginx.
- [ ] Deploy Rails.
- [ ] Setup PostgreSQL production.
- [ ] Setup Solid Queue.
- [ ] Konfigurasi environment variables.
- [ ] Setup database backup.
- [ ] Setup monitoring.
- [ ] Setup logging.

**Target selesai:** Backend production dapat menerima webhook Meta secara stabil.

---

## Fase 11 — Meta App Review

**Tujuan:** Mempersiapkan aplikasi agar dapat digunakan secara publik sesuai requirement Meta.

Checklist:
- [ ] Privacy Policy publik.
- [ ] Terms of Service jika diperlukan.
- [ ] Data deletion flow.
- [ ] Deskripsi penggunaan aplikasi.
- [ ] Jelaskan penggunaan messaging permission.
- [ ] Jelaskan penggunaan content publishing permission.
- [ ] Business Verification jika diperlukan.
- [ ] Request Advanced Access jika diperlukan.
- [ ] Submit App Review.
- [ ] Perbaiki requirement jika diminta Meta.
- [ ] Aktifkan App Live setelah memenuhi requirement.

**Target selesai:** Aplikasi siap digunakan oleh pengguna publik sesuai access yang diberikan Meta.

---

## Fase 12 — Launch

**Tujuan:** Menjalankan akun menfess secara penuh.

Final flow:

User
→ DM akun Instagram
→ Instagram/Meta
→ Webhook
→ Rails
→ Database
→ Moderation
→ Admin Approval
→ Generate Image
→ Instagram Content Publishing API
→ Feed Instagram

Checklist:
- [ ] Semua fase sebelumnya selesai.
- [ ] Testing end-to-end.
- [ ] Testing error dan duplicate event.
- [ ] Testing moderation.
- [ ] Testing publishing.
- [ ] Backup aktif.
- [ ] Monitoring aktif.
- [ ] Privacy Policy tersedia.
- [ ] App/API access production sudah sesuai.
- [ ] Launch.

---

# Tech Stack Awal

Backend:
- Ruby
- Ruby on Rails
- `instagram_connect`

Database:
- PostgreSQL

Frontend/Admin:
- Rails Views
- Hotwire
- Turbo
- Stimulus

Background Processing:
- Active Job
- Solid Queue

Instagram:
- Meta/Instagram API
- Instagram Messaging
- Instagram Webhooks
- Instagram Content Publishing

Deployment:
- Linux
- Nginx
- HTTPS
- VPS
- Cloudflare (opsional)

Redis belum diperlukan untuk MVP. Sidekiq juga belum diperlukan jika Solid Queue sudah mencukupi.

---

# Fokus Saat Ini

Jangan mengerjakan seluruh milestone sekaligus.

Prioritas:

**Fase 0 → Fase 1 → Fase 2**

Checkpoint pertama yang harus dicapai:

> Mengirim DM ke akun Instagram → Meta mengirim event → Rails menerima webhook → isi DM berhasil terbaca.

Setelah checkpoint tersebut berhasil, baru lanjut ke database dan sistem menfess.
