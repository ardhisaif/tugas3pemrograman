# Tugas 3 — Daftar Cryptocurrency

Aplikasi Ionic Vue yang menampilkan nama-nama cryptocurrency dari API Coinlore.

<img width="436" height="806" alt="image" src="https://github.com/user-attachments/assets/5caa3804-40b5-4a96-8c7f-33e5135e2216" />


Sumber data:

```text
https://api.coinlore.net/api/tickers/
```

Setiap baris menampilkan field yang diminta soal:

| Tampilan | Field API |
| --- | --- |
| Rank | `rank` |
| Nama koin di atas simbol | `name` |
| Simbol tebal | `symbol` |
| Harga di bawah label USD | `price_usd` |

Tombol **Refresh** mengambil ulang data. Saat halaman dibuka, data juga langsung dimuat.

## File yang menampilkan data

- `src/main.ts` memasang aplikasi Vue, Ionic, router, dan CSS.
- `src/App.vue` adalah kerangka aplikasi. Isinya `ion-router-outlet`, tempat halaman ditampilkan.
- `src/router/index.ts` mengarahkan `/` ke halaman `/home`.
- `src/services/EndPointAccess/index.ts` adalah class pemanggil API. Method `getRes()` memakai Axios untuk meminta URL yang diberikan.
- `src/views/HomePage.vue` memanggil `fetchData()`, membuat `EndPointAccess` dengan URL Coinlore, menyimpan hasil di `coins`, lalu menampilkan `rank`, `name`, `symbol`, dan `price_usd`.

## Menjalankan

Dari folder `tugas3`, pakai Node.js 22:

```bash
nvm use
npm install
npm run dev
```

Buka http://127.0.0.1:5173/home
