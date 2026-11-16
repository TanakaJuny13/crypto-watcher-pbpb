# Crypto Watcher

Aplikasi mobile sederhana untuk memantau harga mata uang kripto secara real-time. Dibuat dengan **Ionic Vue** dan **Capacitor**, aplikasi ini menampilkan daftar koin kripto beserta peringkat, nama, simbol, dan harganya dalam USD, diambil dari [CoinLore API](https://www.coinlore.com/cryptocurrency-data-api).

> Tugas Sesi 7 — Pemrograman Berbasis Perangkat Bergerak

## Fitur

- Menampilkan daftar koin kripto (rank, nama, simbol, harga USD)
- Tombol **Refresh** untuk memuat ulang data terbaru
- Indikator loading saat data sedang diambil
- Pesan error jika gagal memuat data (misalnya tidak ada koneksi internet)
- Dapat dijalankan di browser maupun sebagai aplikasi Android

## Teknologi

| Kategori        | Teknologi                                 |
| --------------- | ----------------------------------------- |
| Framework UI    | [Ionic Vue](https://ionicframework.com/docs/vue/overview) 9, Vue 3 |
| Bahasa          | TypeScript                                |
| Native runtime  | [Capacitor](https://capacitorjs.com/) 8 (Android) |
| HTTP client     | Axios                                     |
| Build tool      | Vite                                      |
| Testing         | Vitest (unit), Playwright (e2e)           |
| Sumber data     | CoinLore API — `https://api.coinlore.net/api/tickers/` |

## Struktur Proyek

```
crypto-watcher/
├── android/               # Proyek native Android (Capacitor)
├── public/                # Aset statis
├── src/
│   ├── main.ts            # Entry point aplikasi
│   ├── App.vue            # Root component
│   ├── router/index.ts    # Konfigurasi routing
│   ├── theme/variables.css
│   └── views/
│       └── MainPage.vue   # Halaman utama: daftar harga kripto
├── tests/
│   ├── unit/              # Unit test (Vitest)
│   └── e2e/               # End-to-end test (Playwright)
├── capacitor.config.ts    # Konfigurasi Capacitor (appId: com.cryptowatcher.app)
├── vite.config.ts
└── package.json
```

## Prasyarat

- [Node.js](https://nodejs.org/) versi 20 atau lebih baru
- npm (bawaan Node.js) **atau** [Bun](https://bun.sh/) versi 1.x
- Untuk build Android: [Android Studio](https://developer.android.com/studio) dan Android SDK

## Instalasi

```bash
git clone <url-repository>
cd crypto-watcher
```

Lalu install dependency dengan salah satu package manager berikut:

```bash
# npm
npm install

# Bun
bun install
```

## Menjalankan Aplikasi

### Di browser (mode development)

```bash
# npm
npm run dev

# Bun
bun run dev
```

Buka alamat yang tampil di terminal (biasanya `http://localhost:5173`).

### Di perangkat / emulator Android

Dengan npm:

```bash
npm run build        # build aplikasi web ke folder dist/
npx cap sync android # salin hasil build ke proyek Android
npx cap open android # buka di Android Studio
```

Dengan Bun:

```bash
bun run build
bunx cap sync android
bunx cap open android
```

Setelah Android Studio terbuka, jalankan aplikasi ke emulator atau perangkat yang terhubung. Alternatifnya, jalankan langsung dengan `npx cap run android` atau `bunx cap run android`.

## Script yang Tersedia

| npm                 | Bun                 | Keterangan                                     |
| ------------------- | ------------------- | ---------------------------------------------- |
| `npm run dev`       | `bun run dev`       | Menjalankan development server                 |
| `npm run build`     | `bun run build`     | Type-check dan build untuk produksi ke `dist/` |
| `npm run preview`   | `bun run preview`   | Preview hasil build produksi                   |
| `npm run lint`      | `bun run lint`      | Cek ESLint, type-check e2e, dan Prettier       |
| `npm run lint:fix`  | `bun run lint:fix`  | Memperbaiki masalah lint dan format otomatis   |
| `npm run test:unit` | `bun run test:unit` | Menjalankan unit test (Vitest)                 |
| `npm run test:e2e`  | `bun run test:e2e`  | Menjalankan end-to-end test (Playwright)       |
| `npm test`          | `bun run test`      | Menjalankan semua test                         |

> Gunakan `bun run test`, bukan `bun test`. Perintah `bun test` menjalankan test runner bawaan Bun, bukan Vitest yang dipakai project ini.


## Cara Kerja

Saat halaman utama dibuka, [MainPage.vue](src/views/MainPage.vue) memanggil fungsi `loadCoins()` yang mengirim request `GET` ke CoinLore API menggunakan Axios. Respons berisi array koin yang kemudian ditampilkan dalam bentuk baris. Tombol **Refresh** memanggil ulang fungsi yang sama, dan dinonaktifkan selama proses loading berlangsung.

Contoh data yang digunakan dari API:

```json
{
  "id": "90",
  "rank": 1,
  "name": "Bitcoin",
  "symbol": "BTC",
  "price_usd": "65000.00"
}
```

## Penulis

Tanaka Junyartha
