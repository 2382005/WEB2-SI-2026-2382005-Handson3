Eksperimen 1: Session Cookie vs Persistent Cookie
sessionTest: [hilang/ada?] ada
persistentTest: [hilang/ada?] ada

Cookie dengan Path=/admin akan dikirim ke:
- /admin ✓ atau ✗? ✓
- /admin/dashboard ✓ atau ✗? ✓
- /profile ✓ atau ✗? ✗
- / ✓ atau ✗? ✗

Cookie Secure tersimpan di localhost HTTP? Ya (Ya/Tidak)
Mengapa bisa tersimpan? Karena browser exception

![alt text](image.png)

1. Mengapa Session Cookie (Tanpa Max-Age) Berguna untuk Login?Session cookie (atau transient cookie) tidak memiliki parameter Expires atau Max-Age. Hal ini sangat berguna karena:Keamanan Berbasis Sesi: Cookie ini disimpan di RAM, bukan di penyimpanan permanen (hard drive). Begitu pengguna menutup browser sepenuhnya, cookie tersebut dihapus.Mencegah Akses Tak Terbatas: Jika seseorang meminjam komputer pengguna setelah mereka selesai (dan menutup browser), mereka tidak akan bisa masuk kembali secara otomatis karena sesi telah berakhir secara lokal.
2. Atribut Cookie untuk Session ID di E-commerceJika saya membangun aplikasi e-commerce, saya akan menggunakan konfigurasi berikut untuk Session ID:AtributNilaiAlasanHttpOnlytrueMencegah skrip jahat (XSS) mencuri session ID.SecuretrueMemastikan cookie hanya dikirim melalui koneksi terenkripsi (HTTPS).SameSiteLax atau StrictMelindungi pengguna dari serangan CSRF (Cross-Site Request Forgery).Path/Memastikan sesi valid di seluruh bagian situs (keranjang, checkout, profil).
3. Risiko Jika Tidak Menggunakan HttpOnlyRisiko utamanya adalah Session Hijacking melalui serangan XSS (Cross-Site Scripting).Tanpa HttpOnly, penyerang bisa menyisipkan skrip JavaScript ke dalam halaman (misalnya melalui kolom komentar yang tidak difilter) yang berisi kode:document.write(document.cookie); atau mengirimkan cookie tersebut ke server mereka sendiri. Sekali penyerang mendapatkan Session ID Anda, mereka bisa "menyamar" sebagai Anda tanpa perlu tahu password Anda.
4. Kapan Menggunakan Expires vs Max-Age?Meskipun keduanya berfungsi untuk menentukan masa berlaku cookie, ada perbedaan teknis:Max-Age (Modern & Direkomendasikan): Menggunakan durasi dalam detik (misal: 3600 untuk 1 jam). Ini lebih akurat karena tidak bergantung pada sinkronisasi jam antara server dan komputer klien.Expires (Legacy): Menggunakan format tanggal/waktu yang spesifik. Digunakan jika Anda perlu mendukung browser yang sangat tua (seperti Internet Explorer lama).Tip: Jika keduanya ada, Max-Age akan diprioritaskan oleh browser modern.
5. Apa yang Terjadi Jika Domain di-set ke .example.com?Menambahkan tanda titik di depan domain (atau hanya menuliskan nama domain utama) membuat cookie tersebut menjadi Wildcard Cookie.Artinya, cookie tersebut akan dikirimkan ke domain utama dan semua sub-domainnya.Jika di-set di example.com, maka store.example.com, blog.example.com, dan dev.example.com juga bisa membaca cookie tersebut.Risiko: Jika salah satu sub-domain (misal: situs testing) memiliki celah keamanan, penyerang bisa mengakses cookie sesi yang seharusnya hanya untuk domain utama.

