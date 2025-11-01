# 🏋️ Fitness Tracker API

Fitness Tracker, kullanıcıların egzersiz aktivitelerini ve fitness hedeflerini takip etmelerini sağlayan modern bir REST API'dir. .NET 10.0, Entity Framework Core ve JWT authentication kullanılarak geliştirilmiştir.

## 📋 İçindekiler

- [Özellikler](#özellikler)
- [Teknolojiler](#teknolojiler)
- [Kurulum](#kurulum)
- [Kullanım](#kullanım)
- [API Endpoints](#api-endpoints)
- [Veritabanı Yapısı](#veritabanı-yapısı)
- [Güvenlik](#güvenlik)
- [Lisans](#lisans)

## ✨ Özellikler

- 🔐 **JWT Authentication** - Güvenli kullanıcı kimlik doğrulama
- 👤 **Kullanıcı Yönetimi** - Kayıt, giriş ve profil yönetimi
- 🏃 **Egzersiz Takibi** - Egzersiz aktivitelerini kaydetme ve görüntüleme
- 🎯 **Hedef Belirleme** - Fitness hedeflerini oluşturma ve takip etme
- 📊 **RESTful API** - Standart HTTP metodları ile CRUD işlemleri
- 📝 **Swagger UI** - Interaktif API dokümantasyonu
- 🗄️ **SQLite Database** - Hafif ve taşınabilir veritabanı
- 🔄 **AutoMapper** - Otomatik nesne dönüşümleri
- ⚡ **Global Exception Handler** - Merkezi hata yönetimi

## 🛠️ Teknolojiler

- **Framework**: .NET 10.0
- **ORM**: Entity Framework Core 9.0
- **Veritabanı**: SQLite
- **Authentication**: JWT Bearer Token
- **API Dokümantasyonu**: Swagger/OpenAPI
- **Mapping**: AutoMapper
- **Dil**: C# 12.0

## 📦 Kurulum

### Gereksinimler

- [.NET 10.0 SDK](https://dotnet.microsoft.com/download)
- Visual Studio 2022 / VS Code / JetBrains Rider

### Adımlar

1. **Projeyi klonlayın**

```bash
git clone https://github.com/mrkfks/fitness-tracker.git
cd fitness-tracker
```

2. **Bağımlılıkları yükleyin**

```bash
cd FitnessTracker
dotnet restore
```

3. **Veritabanını oluşturun**

```bash
dotnet ef database update
```

4. **Projeyi çalıştırın**

```bash
dotnet run
```

5. **Swagger UI'ya erişin**

```
http://localhost:5210/swagger
```

## 🚀 Kullanım

### Hızlı Başlangıç

1. **Kullanıcı Kaydı**

```http
POST /api/auth/register
Content-Type: application/json

{
  "username": "johndoe",
  "email": "john@example.com"
}
```

2. **Giriş Yapma ve Token Alma**

```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "password123"
}
```

3. **Egzersiz Ekleme (Token Gerekli)**

```http
POST /api/workout
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json

{
  "type": "Running",
  "durationMinutes": 30,
  "date": "2025-11-01T10:00:00Z"
}
```

### FitnessTracker.http Dosyası

Proje içinde bulunan `FitnessTracker.http` dosyasını kullanarak tüm endpoint'leri test edebilirsiniz. VS Code'da REST Client eklentisi ile kullanabilirsiniz.

## 📚 API Endpoints

### Authentication (🔓 Public)

| Method | Endpoint | Açıklama |
|--------|----------|----------|
| POST | `/api/auth/register` | Yeni kullanıcı kaydı |
| POST | `/api/auth/login` | Kullanıcı girişi ve JWT token alma |

### Users (🔒 Authentication Required)

| Method | Endpoint | Açıklama |
|--------|----------|----------|
| GET | `/api/user` | Tüm kullanıcıları listele |
| GET | `/api/user/{id}` | Belirli bir kullanıcıyı getir |
| POST | `/api/user` | Yeni kullanıcı oluştur |
| DELETE | `/api/user/{id}` | Kullanıcı sil |

### Workouts (🔒 Authentication Required)

| Method | Endpoint | Açıklama |
|--------|----------|----------|
| GET | `/api/workout` | Tüm egzersizleri listele |
| GET | `/api/workout/{id}` | Belirli bir egzersizi getir |
| POST | `/api/workout` | Yeni egzersiz kaydet |
| DELETE | `/api/workout/{id}` | Egzersiz sil |

### Goals (🔒 Authentication Required)

| Method | Endpoint | Açıklama |
|--------|----------|----------|
| GET | `/api/goal` | Tüm hedefleri listele |
| GET | `/api/goal/{id}` | Belirli bir hedefi getir |
| POST | `/api/goal` | Yeni hedef oluştur |
| DELETE | `/api/goal/{id}` | Hedef sil |

## 🗄️ Veritabanı Yapısı

### Users Tablosu

```sql
- Id (int, PK)
- Username (string)
- Email (string)
- PasswordHash (string)
- CreatedAt (DateTime)
```

### Workouts Tablosu

```sql
- Id (int, PK)
- Type (string)
- DurationMinutes (int)
- Date (DateTime)
- UserId (int, FK)
```

### Goals Tablosu

```sql
- Id (int, PK)
- Description (string)
- TargetDate (DateTime)
- IsCompleted (bool)
- UserId (int, FK)
```

## 🔐 Güvenlik

- **JWT Bearer Token**: Tüm korumalı endpoint'ler için token gereklidir
- **HTTPS Redirection**: Production'da HTTPS kullanılır
- **CORS**: İhtiyaç durumunda yapılandırılabilir
- **Global Exception Handler**: Hassas hata bilgilerinin gizlenmesi

### JWT Configuration

`appsettings.json` dosyasında JWT ayarlarını yapılandırabilirsiniz:

```json
{
  "Jwt": {
    "Key": "your-secret-key-here",
    "Issuer": "FitnessTracker",
    "Audience": "FitnessUsers"
  }
}
```

⚠️ **Önemli**: Production ortamında güçlü bir secret key kullanın ve environment variables'da saklayın!

## 🏗️ Proje Yapısı

```
FitnessTracker/
├── Controllers/          # API Controller'lar
│   ├── AuthController.cs
│   ├── UserController.cs
│   ├── WorkoutController.cs
│   └── GoalController.cs
├── Data/                 # Database Context
│   └── AppDbContext.cs
├── DTOs/                 # Data Transfer Objects
│   ├── UserDto.cs
│   ├── WorkoutDto.cs
│   └── GoalDto.cs
├── Entities/             # Database Models
│   ├── User.cs
│   ├── Workout.cs
│   └── Goal.cs
├── Services/             # Business Logic
│   ├── TokenService.cs
│   ├── UserService.cs
│   ├── WorkoutService.cs
│   └── GoalService.cs
├── Mappings/             # AutoMapper Profiles
│   └── MappingProfile.cs
├── Middleware/           # Custom Middleware
│   └── GlobalExceptionHandler.cs
├── Migrations/           # EF Core Migrations
├── Utils/                # Utility Classes
│   └── JWTConfiguration.cs
├── appsettings.json      # Configuration
└── Program.cs            # Application Entry Point
```

## 🧪 Testing

### Manual Testing

1. **Swagger UI kullanarak**:
   - `http://localhost:5210/swagger` adresini ziyaret edin
   - Endpoint'leri interaktif olarak test edin

2. **HTTP dosyası kullanarak**:
   - VS Code'da `FitnessTracker.http` dosyasını açın
   - REST Client eklentisi ile endpoint'leri test edin

3. **cURL kullanarak**:

```bash
# Register
curl -X POST "http://localhost:5210/api/auth/register" \
  -H "Content-Type: application/json" \
  -d '{"username":"test","email":"test@example.com"}'

# Login
curl -X POST "http://localhost:5210/api/auth/login" \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"pass123"}'
```

## 🚀 Deployment

### Production Checklist

- [ ] Strong JWT secret key kullanın
- [ ] Environment variables ile configuration yönetin
- [ ] HTTPS zorunlu hale getirin
- [ ] CORS politikalarını yapılandırın
- [ ] Logging ekleyin (Serilog, NLog vb.)
- [ ] Rate limiting uygulayın
- [ ] Veritabanı backup stratejisi oluşturun
- [ ] Health check endpoint'leri ekleyin

### Docker (Opsiyonel)

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src
COPY ["FitnessTracker/FitnessTracker.csproj", "FitnessTracker/"]
RUN dotnet restore "FitnessTracker/FitnessTracker.csproj"
COPY . .
WORKDIR "/src/FitnessTracker"
RUN dotnet build "FitnessTracker.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "FitnessTracker.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "FitnessTracker.dll"]
```

## 📄 Lisans

Bu proje MIT lisansı altında lisanslanmıştır. Detaylar için [LICENSE](LICENSE) dosyasına bakın.

## 👨‍💻 Geliştirici

**Ömer Kafkas**

## 🙏 Teşekkürler

Bu projeyi geliştirirken kullanılan harika araçlar ve kütüphaneler için teşekkürler:

- [.NET Team](https://github.com/dotnet)
- [Entity Framework Core](https://github.com/dotnet/efcore)
- [AutoMapper](https://github.com/AutoMapper/AutoMapper)
- [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)

---

⭐ Bu projeyi beğendiyseniz yıldız vermeyi unutmayın!
