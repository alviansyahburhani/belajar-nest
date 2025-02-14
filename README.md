LAPORAN LAB BACKEND NEST.JS
(Revisi Ringkas dengan Peningkatan Format dan Kejelasan)

1. PENDAHULUAN
Proyek ini merupakan implementasi backend menggunakan NestJS untuk sistem manajemen mahasiswa dengan fitur:

Autentikasi berbasis JWT

Komunikasi real-time via WebSocket

Manajemen profil mahasiswa dan pengguna

Database relational dengan Prisma dan PostgreSQL

Dokumentasi API menggunakan Swagger

Dibangun dengan prinsip skalabilitas dan keamanan, proyek ini cocok sebagai dasar pengembangan aplikasi edukasi modern.

2. STRUKTUR PROYEK
bash
Copy
lab-nestjs/
├── prisma/              # Konfigurasi database
│   └── schema.prisma    # Skema database
├── src/
│   ├── chat/            # Modul WebSocket chat
│   ├── mahasiswa-profile/ # Modul profil mahasiswa
│   ├── profile/         # Modul profil pengguna
│   ├── app.controller.ts # Endpoint utama
│   └── main.ts          # Entry point aplikasi
├── test/                # Unit test
└── .env                 # Environment variables
3. TEKNOLOGI UTAMA
Teknologi	Peran
NestJS	Framework backend TypeScript
Prisma	ORM untuk PostgreSQL
WebSocket	Komunikasi real-time (chat)
JWT	Autentikasi pengguna
Swagger	Dokumentasi API interaktif
4. DIAGRAM ARSITEKTUR
mermaid
Copy
flowchart TB
    Client -->|HTTP| API[NestJS API]
    Client -->|WebSocket| WS[Chat Gateway]
    API --> Prisma --> PostgreSQL
    API -->|Auth| JWT
    WS -->|Broadcast| Client
5. FITUR UTAMA
A. Autentikasi Pengguna
mermaid
Copy
sequenceDiagram
    participant Client
    participant Server
    Client->>Server: POST /register (username, password)
    Server->>Server: Enkripsi password
    Server-->>Client: Registrasi berhasil
    Client->>Server: POST /login (username, password)
    Server-->>Client: Token JWT
    Client->>Server: Request dengan token
    Server-->>Client: Akses diberikan
B. Manajemen Data Mahasiswa
CRUD data mahasiswa

Upload foto profil

Pencarian berdasarkan NIM/nama/jurusan

C. Chat Real-Time
mermaid
Copy
sequenceDiagram
    participant UserA
    participant Server
    participant UserB
    UserA->>Server: Join room "KelasC"
    Server->>UserB: Notifikasi UserA bergabung
    UserA->>Server: Kirim pesan ke "KelasC"
    Server->>UserB: Teruskan pesan
6. PANDUAN INSTALASI
Clone Repository

bash
Copy
git clone https://github.com/username/lab-nestjs.git
cd lab-nestjs
Instal Dependensi

bash
Copy
npm install
Setup Database

Buat database PostgreSQL

Konfigurasi .env:

env
Copy
DATABASE_URL="postgresql://user:password@localhost:5432/lab-nestjs"
JWT_SECRET="rahasia_kampus"
Migrasi Database

bash
Copy
npx prisma migrate dev --name init
Jalankan Aplikasi

bash
Copy
npm run start:dev
Akses API

Aplikasi: http://localhost:3000

Dokumentasi: http://localhost:3000/api-docs

7. DOKUMENTASI KODE
Contoh Endpoint: GET /mahasiswa
typescript
Copy
// src/app.controller.ts
@Get('mahasiswa')
@ApiOperation({ summary: 'Ambil semua data mahasiswa' })
async getMahasiswa() {
  return this.appService.getMahasiswa();
}
Contoh Service: Auth Guard
typescript
Copy
// src/auth.guard.ts
@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const request = context.switchToHttp().getRequest();
    // Validasi token JWT
    return validateRequest(request);
  }
}
8. BEST PRACTICES
Modular Design

Setiap fitur dipisah dalam modul independen

Error Handling

Gunakan exception filter untuk respon error konsisten

Security

Enkripsi password dengan bcrypt

Validasi input menggunakan class-validator

9. TESTING
Test Login Pengguna
typescript
Copy
// Contoh test dengan Jest
describe('AuthController', () => {
  it('should return token for valid credentials', async () => {
    const response = await request(app)
      .post('/login')
      .send({ username: 'admin', password: 'password' });
    expect(response.statusCode).toEqual(200);
    expect(response.body).toHaveProperty('token');
  });
});
10. KESIMPULAN
Proyek ini berhasil mengimplementasikan:

Arsitektur backend modular dengan NestJS

Autentikasi aman menggunakan JWT

Sistem chat real-time via WebSocket

Manajemen data terstruktur dengan Prisma

Dapat dikembangkan lebih lanjut dengan menambahkan fitur:

Notifikasi email

Integrasi dengan sistem akademik lain

Monitoring performa dengan Grafana/Prometheus
