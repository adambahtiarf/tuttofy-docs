# Dokumentasi Tuttofy

## Gambaran Umum

Tuttofy adalah platform tutor avatar AI online yang membantu guru, tutor, dan subject-matter expert mengubah persona, gaya mengajar, dan pengetahuan mereka menjadi tutor AI interaktif.

Pada versi produk saat ini, Tuttofy menggunakan Tavus sebagai penyedia percakapan avatar. Tavus menjalankan pengalaman avatar secara langsung, sementara Tuttofy tetap mengelola identitas pengguna, pengaturan tutor, struktur pembelajaran, materi, dan alur belajar untuk siswa.

Web app inti Tuttofy dibangun dengan Next.js. Clerk menangani autentikasi dan session, Neon menyimpan data aplikasi utama, Upstash Redis mendukung kebutuhan cache atau rate limiting, Cloudflare R2 digunakan untuk object storage seperti materi atau file pembelajaran, dan Stripe digunakan sebagai payment provider.

Portal atau sistem admin internal berada di aplikasi terpisah. Repositori dokumentasi ini berfokus pada perilaku produk untuk aplikasi inti Tuttofy yang digunakan tutor dan pembelajar.

Repositori dokumentasi ini adalah sumber acuan internal untuk menjelaskan cara kerja fitur-fitur Tuttofy. Dokumentasi ini ditujukan untuk membantu penyelarasan produk di antara founder, product, design, dan engineering.

## Siapa yang Dilayani Tuttofy

Tuttofy dirancang untuk:

- Guru yang ingin memperluas jangkauan pengajaran melalui tutor avatar AI
- Tutor yang ingin menyediakan pengalaman belajar terstruktur atau sesuai kebutuhan
- Pakar pengetahuan yang ingin mengemas keahlian mereka menjadi pengalaman belajar terpandu
- Siswa atau pembelajar yang ingin mengakses tutor dengan kemampuan pada bidang tertentu

Tutor di platform dapat mencakup mata pelajaran seperti matematika, bahasa Inggris, sejarah, coding, dan domain pengetahuan khusus lainnya.

## Model Platform Saat Ini

Setiap tutor di Tuttofy memiliki profil publik, kemampuan yang didefinisikan, knowledge scope, materi pembelajaran, dan guardrails percakapan. Namun pengalaman belajar utama kini berpusat pada `course`, bukan langsung pada tutor secara umum.

Pembelajar dapat menelusuri tutor yang tersedia, meninjau profil tutor seperti `about the teacher`, `experience`, dan `certificates`, lalu memilih `course` yang relevan. Sebelum benar-benar bergabung ke course, pembelajar wajib melewati tahap `exploration` agar Tuttofy dapat memahami goal belajar mereka dan menyiapkan konteks yang lebih personal.

## Arsitektur Belajar Berbasis Course

Di Tuttofy, tutor membuat `course` terlebih dahulu, misalnya `Matematika SD Kelas 1-3`. Setiap course memiliki tujuan belajar, scope, guardrail, dan jalur belajar yang spesifik.

Di dalam setiap course, pembelajar selalu memiliki dua jalur interaksi:

- `Module Step Learning`
- `Course Free Conversation`

Kedua jalur ini sama-sama memakai konteks course dan konteks personal pembelajar agar percakapan tidak melebar terlalu jauh dari goal belajar yang sudah ditetapkan.

## Tahap Exploration dan Learning Path

Sebelum join ke course, pembelajar wajib masuk ke tahap `exploration`.

Dalam tahap ini, pembelajar:

- Membaca profil tutor dan detail course terlebih dahulu
- Menuliskan goal belajar pada textarea
- Meminta AI menyusun `personalized learning path`
- Meninjau hasil learning path dalam bentuk teks panjang
- Mengunduh versi `PDF`
- Menonton video AI yang menjelaskan perjalanan belajar yang akan dilalui jika memakai course tersebut
- Memutuskan apakah ingin `join this course`

Learning path ini disimpan sebagai `user-course enrollment context`, sehingga tidak hanya berfungsi sebagai materi preview, tetapi juga menjadi konteks resmi pembelajaran user pada course tersebut.

## Mode Belajar 1: Module Step Learning

`Module Step Learning` adalah alur belajar terstruktur di dalam course.

Dalam mode ini, tutor menyusun urutan `module step` atau `milestone` dari dasar sampai akhir. Pembelajar bergerak mengikuti step satu per satu berdasarkan kurikulum course yang disiapkan tutor.

Setiap `module step` dapat mencakup:

- Sesi belajar berbasis avatar
- Materi pembelajaran opsional
- File yang dapat diunduh
- Pelacakan progres
- Status penyelesaian
- Summary setelah step selesai

Mode ini ditujukan untuk pembelajar yang menginginkan pengalaman belajar yang terpandu, berurutan, dan memiliki recap setelah setiap tahap.

Contoh:

Seorang tutor matematika membuat course bernama `Matematika SD Kelas 1-3` dengan step seperti:

- Pengenalan angka dan operasi dasar
- Penjumlahan dan pengurangan
- Perkalian dasar
- Pembagian sederhana
- Latihan akhir

Pembelajar menyelesaikan step satu per satu, lalu menerima summary setelah tiap step selesai.

## Mode Belajar 2: Course Free Conversation

`Course Free Conversation` adalah alur belajar fleksibel yang tetap berada di dalam batas course tertentu.

Dalam mode ini, pembelajar dapat memulai percakapan dengan avatar tutor AI berdasarkan pertanyaan, latihan, atau topik yang masih relevan dengan course yang sedang diikuti.

Percakapan tidak bersifat bebas secara global. Scope pembahasannya harus tetap berada dalam:

- Tujuan dan guardrail course
- Learning path personal user
- Progress user pada course tersebut

Mode ini berguna untuk:

- Bantuan pekerjaan rumah yang masih relevan dengan course
- Penjelasan yang lebih personal
- Dukungan latihan tambahan
- Klarifikasi topik tertentu dalam course
- Diskusi bebas yang tetap terpandu

Contoh:

Seorang siswa yang mengambil course `Matematika SD Kelas 1-3` bertanya, `Bisakah kamu bantu aku latihan perkalian sederhana?`

Tutor AI dapat menjawab, memberi latihan, menyesuaikan penjelasan dengan learning path user, dan menjaga interaksi tetap berada dalam ruang lingkup course tersebut.

## Konsep Inti Platform

### AI Tutor Personas

Tutor dapat membuat persona tutor AI yang merepresentasikan identitas, pendekatan mengajar, dan gaya belajar mereka. Persona ini membentuk cara avatar berkomunikasi dengan pembelajar.

### Tutor, Guru, dan Expert

Untuk keperluan dokumentasi, `tutor` adalah istilah produk utama. Seorang tutor dapat berupa guru, tutor privat, coach, atau subject-matter expert yang menyampaikan pengetahuan melalui Tuttofy.

### Course, Module Step, dan Summary

`Course` merepresentasikan wadah utama pengalaman belajar. `Module step` atau `milestone` merepresentasikan langkah belajar, sesi avatar, atau checkpoint individual di dalam course. Setelah satu step selesai, sistem menghasilkan `summary` sebagai recap hasil belajar sebelum user melanjutkan ke step berikutnya.

### Materi Pembelajaran

Tutor dapat melampirkan konten pembelajaran dan sumber daya yang dapat diunduh untuk mendukung sesi avatar dan proses belajar terstruktur.

### Guardrails dan Knowledge Scope

Setiap pengalaman belajar harus tetap berada dalam batas subjek yang telah disetujui. Guardrails menentukan apa yang boleh atau tidak boleh dilakukan avatar, sedangkan knowledge scope menentukan batas bidang dan konten yang dapat dibahas tutor. Pada model baru, guardrail utama berlaku di level `course`, lalu diperkuat lagi oleh `learning path` personal user.

## Tujuan Dokumentasi

Repositori ini digunakan untuk mendokumentasikan fitur-fitur Tuttofy secara konsisten agar setiap halaman fitur nantinya dapat menjelaskan:

- Apa fitur tersebut
- Mengapa fitur itu ada
- Siapa yang menggunakan fitur tersebut
- Bagaimana fitur itu bekerja
- Alur utama pengguna
- Aturan bisnis
- Data penting
- Edge case
- Fitur terkait
- Catatan produk atau teknis tambahan

## Konvensi Dokumentasi

Fondasi dokumentasi saat ini mengikuti konvensi berikut:

- Dokumentasi bahasa Inggris berada di `docs/en`
- Dokumentasi Bahasa Indonesia berada di `docs/id`
- Satu fitur sebaiknya didokumentasikan per satu halaman Markdown
- Halaman bahasa Inggris dan Bahasa Indonesia harus saling mencerminkan secara struktur
- Nama file fitur sebaiknya menggunakan format kebab-case yang stabil
- Halaman fitur harus mengikuti template bersama di `templates/feature-template.md`
- Setiap halaman fitur atau sistem harus menyertakan minimal satu diagram Mermaid
- Diagram Mermaid menjadi prioritas utama, bukan dekorasi opsional, karena penjelasan visual harus membantu pembaca memahami dokumen lebih cepat
- Gunakan `flowchart` untuk alur produk, navigasi, dan batas sistem
- Gunakan `sequenceDiagram` untuk interaksi aktor ke sistem seperti sign-in, pertukaran API, atau flow verifikasi
- Jika dokumen membahas perilaku proses dan interaksi sekaligus, sertakan flowchart dan sequence diagram bila memang membantu
- Jaga label Mermaid tetap ringkas agar diagram mudah dibaca di renderer Markdown
- Catatan teknis harus tetap ringkas kecuali fase berikutnya memperluasnya

## Dokumen yang Sudah Tersedia

- [Tech Stack](./tech-stack.md)
- [Authentication](./authentication.md)
- [Onboarding](./onboarding.md)
- [Family Account](./family-account.md)
- [Course Discovery and Join](./course-discovery-and-join.md)
- [Course Learning Experience](./course-learning-experience.md)
- [Teacher Personalization](./teacher-personalization.md)

## Rencana Dokumentasi Fitur

Fase pertama ini belum mendokumentasikan setiap fitur secara penuh. Area dokumentasi fitur yang direncanakan meliputi:

- Authentication
- Onboarding
- Family account
- User profile
- Teacher profile
- Teacher personalization
- Course discovery
- Create course
- Create module step
- Upload learning material
- Student learning progress
- Course free conversation
- Avatar conversation session
- Guardrails and knowledge scope
- Download material
- Admin management

Dokumentasi payment atau subscription sengaja di luar cakupan saat ini dan hanya perlu ditambahkan setelah fiturnya aktif.
