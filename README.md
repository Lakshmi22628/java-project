# VaultX

VaultX is a secure digital document vault web application built with Spring Boot, Spring Security, Hibernate/JPA, MySQL, Thymeleaf, Bootstrap, AES encryption, PDF text extraction, QR code generation, and admin analytics.

## Highlights

- User registration and secure login with BCrypt password hashing
- OTP-based email verification used as a second authentication factor
- AES-GCM encrypted file storage in MySQL as BLOB data
- Secure download with decryption and digital signature hash verification
- Document categories, versioning, sharing, recycle bin, QR download links
- AI-style PDF text search powered by Apache PDFBox extraction
- Admin dashboard with users, documents, audit logs, and Chart.js storage graph
- MVC + layered architecture: Controller -> Service -> Repository -> Database

## Default Credentials

- Admin email: `admin@vaultx.com`
- Admin password: `Admin@123`

## Setup

1. Create a MySQL database or allow `createDatabaseIfNotExist=true`.
2. Copy [.env.example](/C:/Users/Madhuri%20M/OneDrive/Documents/New%20project/.env.example) and set these environment variables before running the app:
   `DB_URL`
   `DB_USERNAME`
   `DB_PASSWORD`
   `MAIL_USERNAME`
   `MAIL_PASSWORD`
   `VAULTX_AES_SECRET`
   `VAULTX_BASE_URL`
   `SERVER_ADDRESS`
   `SERVER_PORT`
3. Run `mvn spring-boot:run`.
4. Open `http://localhost:8080`.

## Local Configuration Notes

- The project now reads secrets from environment variables instead of storing them in source files.
- For local development, `DB_USERNAME` defaults to `root` if you do not set it.
- `VAULTX_AES_SECRET` should be a strong private key and must be changed before production use.

## Shared Base URL Setup

Use these values depending on where the app will run:

- Local only:
  `VAULTX_BASE_URL=http://localhost:8080`
- Same Wi-Fi / LAN:
  `VAULTX_BASE_URL=http://YOUR-PC-IP:8080`
  `SERVER_ADDRESS=0.0.0.0`
- Public deployment:
  `VAULTX_BASE_URL=https://your-domain-or-host-url`
  `SERVER_ADDRESS=0.0.0.0`

## LAN Access First

1. Find your computer's IPv4 address in PowerShell:
   `ipconfig`
2. Look for the active adapter and note the IPv4 address, for example `192.168.1.25`.
3. Set the environment variables before starting the app:
   `SERVER_ADDRESS=0.0.0.0`
   `SERVER_PORT=8080`
   `VAULTX_BASE_URL=http://192.168.1.25:8080`
4. Start the application with `mvn spring-boot:run`.
5. Allow Java or port `8080` through Windows Firewall if prompted.
6. Share this link with people on the same network:
   `http://192.168.1.25:8080`

PowerShell example for the current terminal:

```powershell
$env:SERVER_ADDRESS="0.0.0.0"
$env:SERVER_PORT="8080"
$env:VAULTX_BASE_URL="http://192.168.1.25:8080"
$env:DB_URL="jdbc:mysql://localhost:3306/vaultx_new?createDatabaseIfNotExist=true&useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC"
$env:DB_USERNAME="root"
$env:DB_PASSWORD="your-password"
$env:MAIL_USERNAME="your-email@example.com"
$env:MAIL_PASSWORD="your-app-password"
$env:VAULTX_AES_SECRET="replace-with-a-strong-secret-key"
mvn spring-boot:run
```

When the app starts, it will log local and LAN URLs in the console.

## Deploy Online Step By Step

### Option 1: Render

1. Push this project to GitHub.
2. Create a MySQL database service, or preferably use a managed MySQL provider.
3. In Render, create a new `Web Service`.
4. Connect your GitHub repository.
5. Set:
   Build command: `mvn clean package -DskipTests`
   Start command: `java -jar target/codex_vaultx-1.0.0.jar`
6. Add environment variables in Render:
   `DB_URL`
   `DB_USERNAME`
   `DB_PASSWORD`
   `MAIL_USERNAME`
   `MAIL_PASSWORD`
   `VAULTX_AES_SECRET`
   `VAULTX_BASE_URL=https://your-render-url.onrender.com`
   `SERVER_ADDRESS=0.0.0.0`
   `SERVER_PORT=10000`
7. Set `DB_URL` to your hosted MySQL connection string, for example:
   `jdbc:mysql://HOST:3306/vaultx_new?useSSL=true&serverTimezone=UTC`
8. Deploy and open the generated Render URL.

### Option 2: Railway

1. Push the project to GitHub.
2. Create a new Railway project from the repository.
3. Add a MySQL service or connect an external MySQL database.
4. Set the same environment variables listed above.
5. Set `VAULTX_BASE_URL` to the Railway public domain.
6. Deploy and test registration, OTP email, upload, and download.

## Recommended Next Deployment Cleanup

- Move `spring.datasource.url` to an environment variable such as `DB_URL`.
- Add a production profile for cloud deployments.
- Use a real domain with HTTPS.
- Store secrets in the hosting platform secret manager only.

## Cloud Readiness

- Stateless-friendly controller/service design with database-backed persistence
- Environment-based config can be externalized for AWS, Azure, or GCP
- Suitable next deployment steps: RDS/Cloud SQL, object storage migration, SMTP secret manager, HTTPS ingress, and containerization
