# Eclipta Tent Circle

Landing page statis berbahasa Indonesia untuk komunitas camping Eclipta Tent Circle. Halaman ini memperkenalkan komunitas, aktivitas, galeri, jadwal camp, dan menyediakan formulir untuk bergabung.

## Fitur

- Navigasi responsif dan menu untuk perangkat seluler.
- Informasi komunitas, aktivitas camping, galeri foto dengan lightbox, dan jadwal acara.
- Formulir anggota dengan pilihan trip, nama, WhatsApp, Instagram, domisili, status gear/tenda, dan catatan.
- Formulir mengirim data ke webhook n8n. Pesan sukses hanya ditampilkan setelah webhook merespons berhasil; jika pengiriman gagal, formulir tetap tersedia untuk dicoba lagi.

## Menjalankan secara lokal

Tidak diperlukan proses build atau instalasi dependensi. Jalankan server statis dari direktori proyek:

```bash
python3 -m http.server 8000
```

Buka <http://localhost:8000> di browser.

Halaman juga dapat di-deploy sebagai situs statis menggunakan `index.html` sebagai halaman utama. Tailwind CSS, Google Fonts, Font Awesome, dan gambar eksternal memerlukan koneksi internet.

## Integrasi n8n

URL webhook saat ini dikonfigurasi di bagian JavaScript pada `index.html`. Formulir mengirim permintaan `POST` dengan tipe konten `application/x-www-form-urlencoded`. Field yang dikirim:

| Field | Isi |
| --- | --- |
| `selectedTrip` | Trip yang dipilih; default `General Join` |
| `name` | Nama lengkap atau panggilan |
| `whatsapp` | Nomor WhatsApp |
| `instagram` | Username Instagram |
| `city` | Domisili kota |
| `gearStatus` | Status gear/tenda |
| `notes` | Pesan atau catatan tambahan |
| `submittedAt` | Waktu pengiriman dalam format ISO 8601 |

Workflow n8n perlu dikonfigurasi sendiri untuk menyimpan data ke tabel dan mengirim notifikasi Telegram. URL yang digunakan saat ini memakai endpoint `/webhook-test/`, yang hanya aktif saat workflow sedang menunggu pengujian. Untuk penggunaan produksi, aktifkan workflow dan ganti URL tersebut dengan URL Production `/webhook/` di `index.html`.

**Perhatian:** pada pengujian terakhir, endpoint membalas `403` dari Cloudflare sebelum permintaan mencapai n8n. Aturan Cloudflare perlu mengizinkan permintaan ke webhook; pengiriman dari halaman juga harus diizinkan oleh kebijakan CORS endpoint.
