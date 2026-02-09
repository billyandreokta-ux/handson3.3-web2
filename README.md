## Eksperimen 1
- Cookies: [masih ada]
- LocalStorage: [masih ada]
- SessionStorage: [hilang]

## Eksperimen 2
Setelah 5x di klik nilai counter adalah 5, tetapi setelah dibuka tab baru nilai counter menjadi 0

## Eksperimen 3
- Cookies:(kosong)idelan = 4 KB
- LocalStorage:(kosong)ideal = 10 MB
- SessionStorage:(kosong)ideal = 10 MB

## Eksperimen 4
1. User Theme Preference
Jawaban: LocalStorage 
Karena theme preference harus persist meski browser ditutup dan dibuka lagi. LocalStorage tidak hilang saat browser ditutup, dan data theme tidak perlu dikirim ke server setiap request (cukup disimpan client-side). SessionStorage salah karena akan hilang saat browser tutup. Cookies juga kurang tepat karena akan dikirim ke server setiap request (waste bandwidth untuk data yang gak perlu server tahu).

2. Multi-step Form (Wizard)
Jawaban: SessionStorage 
Form wizard data sensitif sebaiknya hilang saat tab ditutup untuk privacy. SessionStorage otomatis cleanup saat tab ditutup, jadi data tidak "tertinggal" di browser. Plus, SessionStorage tab-isolated jadi user bisa buka multiple forms di tab berbeda tanpa conflict. LocalStorage salah karena data akan persist dan bisa jadi privacy risk.

3. Session Authentication Token
Jawaban: Cookies (HttpOnly) 
Authentication token HARUS pakai Cookies dengan flag HttpOnly karena:

HttpOnly flag bikin JavaScript gak bisa akses cookie → protected dari XSS attacks
Browser otomatis kirim cookie ke server setiap request → gak perlu manual coding
Bisa tambah Secure flag (HTTPS only) dan SameSite (CSRF protection)

LocalStorage dan SessionStorage BAHAYA untuk auth token karena bisa diakses JavaScript, vulnerable to XSS. Kalau ada script injection, attacker bisa steal token.

4. Shopping Cart (tanpa login)
Jawaban: LocalStorage 
Shopping cart untuk guest user harus persist meski browser ditutup (supaya user bisa lanjut belanja besok). LocalStorage perfect karena data tetap ada permanently. Cart data juga tidak perlu dikirim ke server setiap request (cukup saat checkout). SessionStorage salah karena cart akan hilang saat browser ditutup (bad UX). Cookies juga salah karena cart data bisa besar (exceed 4KB limit) dan akan dikirim setiap request (waste bandwidth).

## Refleksi
1. Mengapa session token TIDAK boleh disimpan di LocalStorage?
Karena rentan XSS attack. LocalStorage bisa diakses JavaScript, jadi kalau ada script injection, attacker bisa steal token dengan localStorage.getItem('token'). Cookies dengan HttpOnly flag aman karena JavaScript tidak bisa akses sama sekali.
2. Apa keuntungan SessionStorage untuk multi-step form dibanding LocalStorage?
Privacy - Data otomatis terhapus saat tab ditutup, tidak "tertinggal" di browser untuk data sensitif. Tab isolation - User bisa isi multiple forms di tab berbeda tanpa conflict. Fresh start - User baru dapat form kosong, gak ada data bekas session sebelumnya.
3. Jika kamu membuat aplikasi todo list offline-first, storage mana yang akan kamu gunakan dan mengapa?
LocalStorage. Karena data harus persist meski browser ditutup (todo list tetap ada), aplikasi bisa jalan offline, dan API simple untuk CRUD operations. Buat sync queue di LocalStorage juga untuk track changes yang belum sync ke server saat online kembali.