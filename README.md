# Simulasi SPT Tahunan PPh OP

Aplikasi web single-file (tanpa build step, berjalan 100% di browser) untuk simulasi perhitungan **SPT Tahunan Pajak Penghasilan Orang Pribadi (PPh OP)** sesuai ketentuan perpajakan Indonesia dan alur Coretax DJP.

Live demo: https://spt-op.pages.dev

## Fitur Utama

1. **Profil & Status Perpajakan (`TK`, `KK`, `PH`, `MT`, `HB`)**
   - Penentuan PTKP otomatis (`TK/0-3`, `K/0-3`, `K/I/0-3`) dan jumlah tanggungan.
   - Pengurang Zakat / Sumbangan Keagamaan Wajib (Pasal 9 ayat 1 huruf g UU PPh).
   - Penanganan status final istri 1 pemberi kerja vs gugur final (`KK_FINAL` vs `KK_MERGED`) serta pembagian proporsional 2 SPT untuk `PH` dan `MT` (Pasal 8 ayat 3 UU PPh).

2. **Menu Karyawan (Multi Bukti Potong 1721)**
   - Mendukung banyak bukti potong untuk Suami dan Istri.
   - **Pilihan Jenis Bukti Potong per kartu:** `1721-A1` (pegawai tetap swasta/BUMN/BUMD), `1721-A2` (pegawai tetap instansi pemerintah/ASN/TNI/Polri), `1721-VI` (bukan pegawai / pegawai tidak tetap / tenaga ahli), dan `1721-VII` (pesangon / manfaat pensiun / THT-TJHT, **final**).
   - Dua mode input: **Input Langsung** (Neto & PPh 21) dan **Dari Gaji Bruto** (otomatis menghitung Biaya Jabatan 5% maks. Rp 500 rb/bulan untuk 1721-A1/A2, DPP 50% × bruto untuk 1721-VI, atau tarif PPh 21 Final PP 68/2009 untuk 1721-VII).
   - Perlakuan per jenis: 1721-A1/A2/VI → penghasilan neto digabung ke SPT & PPh 21 dikreditkan; **1721-VII → final**, penghasilan tidak digabung ke PKP Pasal 17, PPh 21 tidak dikreditkan, dan dilaporkan pada kartu ringkasan Lampiran Penghasilan Final.

3. **Menu Usaha & Pekerjaan Bebas**
   - Mendukung 3 skema: **PPh Final UMKM 0,5%** (PP 55/2022 jo. PP 20/2026, kuota bebas pajak Rp 500 juta untuk OP), **Pencatatan NPPN** (preset KLU & 3 zona wilayah PER-17/PJ/2015 + custom %), dan **Pembukuan** (HPP/biaya atau langsung neto fiskal).
   - Guardrail otomatis: Pekerjaan Bebas tidak dapat memakai Final 0,5%, dan peringatan wajib Pembukuan saat agregasi omzet gabungan suami-istri mencapai Rp 4,8 Miliar.
   - Input omzet tahunan cepat atau rincian 12 bulan (Jan-Des).

4. **Menu Kredit Pajak (Pasal 28 UU PPh & Coretax)**
   - Ringkasan kredit pajak terintegrasi dari menu Karyawan, Usaha/Bebas, dan input mandiri (mencegah input ganda).
   - **Bukti Potong / Pungut Pihak Lain (Lampiran 1E):** PPh 21 Non-Final (nominal atau estimasi 50% bruto), PPh 22, PPh 23, PPh 26 ayat (5), PPh DTP Non-Final, dan PPh 15 Non-Final, dengan opsi penambahan penghasilan neto terkait bila belum diinput di menu lain.
   - **Kredit Pajak Luar Negeri / PPh Pasal 24 (Lampiran 2C):** Perhitungan otomatis batas maksimum KPLN per sumber/negara sesuai PMK 192/PMK.03/2018.
   - **PPh Dibayar Sendiri (Induk 10b-10d):** Angsuran PPh Pasal 25 (tahunan atau 12 bulan), Pokok Pajak STP PPh Pasal 25 (tanpa sanksi bunga/denda), dan penyesuaian pengembalian/pengurangan KPLN (Pasal 24 ayat 5).

5. **Panel Hasil, Perbandingan & Ekspor**
   - Status akhir SPT (**Kurang Bayar PPh 29**, **Lebih Bayar PPh 28A**, atau **Nihil**) beserta kode billing (`KAP 411125 - KJS 200` dan `KAP 411128 - KJS 420`).
   - Kertas Kerja Perhitungan SPT, rincian lapisan tarif progresif UU HPP, rincian kredit pajak, estimasi angsuran PPh Pasal 25 tahun berikutnya (termasuk alternatif WP OPPT 0,75%), perbandingan skema Final 0,5% vs NPPN, dan perbandingan `KK` vs `PH/MT`.
   - Auto-save ke `localStorage`, tombol **Salin Ringkasan**, dan **Cetak / PDF**.

6. **Header & Footer**
   - Header: tombol **Reset isian** dan tombol dukungan **Saweria** (`https://saweria.co/Hansdev`, terbuka di tab baru).
   - Footer: kanal kirim kritik, saran, masukan, atau laporan bug ke email **email@hansdev.web.id** (tautan `mailto:` dengan subjek otomatis).
   - Tombol donasi dan footer disembunyikan saat cetak / ekspor PDF (`no-print`), karena hasil cetak difokuskan pada kertas kerja SPT.

## Struktur Proyek

```text
spt-op/
├── dist/
│   └── index.html    # Aplikasi lengkap (HTML, CSS, dan JS dalam 1 file)
├── .gitignore
└── README.md
```

## Menjalankan Secara Lokal

Karena seluruh aplikasi berada dalam satu file statis tanpa dependensi runtime:

```bash
cd dist
python3 -m http.server 8765
```

Lalu buka `http://localhost:8765` di browser.

## Deploy ke Cloudflare Pages

```bash
npx wrangler pages deploy dist --project-name=spt-op --branch=main
```

## Catatan

Aplikasi ini ditujukan sebagai sarana edukasi dan simulasi mandiri perpajakan. Pelaporan resmi tetap mengacu pada ketentuan peraturan perundang-undangan perpajakan dan sistem Coretax DJP.
