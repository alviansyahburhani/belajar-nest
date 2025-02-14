# 📌 Laporan Lab Backend NestJS

## 📖 Pendahuluan
Proyek ini merupakan implementasi backend menggunakan **NestJS** untuk sistem manajemen mahasiswa dengan fitur:

✅ **Autentikasi** berbasis **JWT**  
✅ **Komunikasi real-time** via **WebSocket**  
✅ **Manajemen profil** mahasiswa dan pengguna  
✅ **Database relational** dengan **Prisma** dan **PostgreSQL**  
✅ **Dokumentasi API** menggunakan **Swagger**  

Dibangun dengan prinsip **skalabilitas dan keamanan**, proyek ini cocok sebagai dasar pengembangan aplikasi edukasi modern.

---
## 📂 Struktur Proyek
```
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
```

---
## 🛠 Teknologi Utama
| Teknologi  | Peran |
|------------|----------------------------|
| **NestJS** | Framework backend TypeScript |
| **Prisma** | ORM untuk PostgreSQL |
| **WebSocket** | Komunikasi real-time (chat) |
| **JWT** | Autentikasi pengguna |
| **Swagger** | Dokumentasi API interaktif |

---
## 🏗 Diagram Arsitektur
```mermaid
graph TB
    Client -->|HTTP| API[NestJS API]
    Client -->|WebSocket| WS[Chat Gateway]
    API --> Prisma --> PostgreSQL
    API -->|Auth| JWT
    WS -->|Broadcast| Client
```

---
## ✨ Fitur Utama
### 🔐 Autentikasi Pengguna
```mermaid
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
```
### 🎓 Manajemen Data Mahasiswa
- **CRUD** data mahasiswa
- **Upload** foto profil
- **Pencarian** berdasarkan **NIM/nama/jurusan**

### 💬 Chat Real-Time
```mermaid
sequenceDiagram
    participant UserA
    participant Server
    participant UserB
    UserA->>Server: Join room "KelasC"
    Server->>UserB: Notifikasi UserA bergabung
    UserA->>Server: Kirim pesan ke "KelasC"
    Server->>UserB: Teruskan pesan
```

---
## ⚙️ Panduan Instalasi
### 1️⃣ Clone Repository
```bash
git clone https://github.com/username/lab-nestjs.git
cd lab-nestjs
```
### 2️⃣ Instal Dependensi
```bash
npm install
```
### 3️⃣ Setup Database
1. **Buat database PostgreSQL**  
2. **Konfigurasi `.env`**
```env
DATABASE_URL="postgresql://user:password@localhost:5432/lab-nestjs"
JWT_SECRET="rahasia_kampus"
```
3. **Migrasi Database**
```bash
npx prisma migrate dev --name init
```
### 4️⃣ Jalankan Aplikasi
```bash
npm run start:dev
```
### 5️⃣ Akses API
- **Aplikasi**: [`http://localhost:3000`](http://localhost:3000)
- **Dokumentasi API**: [`http://localhost:3000/api-docs`](http://localhost:3000/api-docs)

---
## 📌 Dokumentasi Kode
### 📌 Contoh Endpoint: **GET /mahasiswa**
```typescript
// src/app.controller.ts
@Get('mahasiswa')
@ApiOperation({ summary: 'Ambil semua data mahasiswa' })
async getMahasiswa() {
  return this.appService.getMahasiswa();
}
```
### 🔐 Contoh Service: **Auth Guard**
```typescript
// src/auth.guard.ts
@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const request = context.switchToHttp().getRequest();
    // Validasi token JWT
    return validateRequest(request);
  }
}
```

---
## 📌 Best Practices
✅ **Modular Design**  
  - Setiap fitur dipisah dalam modul independen  
✅ **Error Handling**  
  - Gunakan **exception filter** untuk respon error yang konsisten  
✅ **Security**  
  - **Enkripsi password** dengan `bcrypt`  
  - **Validasi input** menggunakan `class-validator`  

---
## 🧪 Testing
### ✅ Test Login Pengguna
```typescript
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
```

---
## 🏁 Kesimpulan
Proyek ini berhasil mengimplementasikan:
✅ **Arsitektur backend modular** dengan **NestJS**  
✅ **Autentikasi aman** menggunakan **JWT**  
✅ **Sistem chat real-time** via **WebSocket**  
✅ **Manajemen data terstruktur** dengan **Prisma**  

🚀 Dapat dikembangkan lebih lanjut dengan:
- **Notifikasi email** 📩
- **Integrasi dengan sistem akademik lain** 🏫
- **Monitoring performa dengan Grafana/Prometheus** 📊


