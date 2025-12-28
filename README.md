# URL Shortener

A high-performance URL shortening service built with Go, featuring Redis caching and PostgreSQL storage.

## 🚀 Live Demo

**Try it:** [https://your-app.fly.dev](https://your-app.fly.dev)
```bash
# Shorten a URL
curl -X POST https://your-app.fly.dev/api/shorten \
  -H "Content-Type: application/json" \
  -d '{"url": "https://github.com/yourusername"}'

# Response
{
  "short_url": "https://your-app.fly.dev/aB3xK9"
}
```

## ✨ Features

- ⚡ **Lightning fast** - Redis caching for sub-millisecond redirects
- 🔒 **Secure** - Input validation and SQL injection prevention
- 📊 **Analytics** - Track click counts for each short URL
- 🎯 **Clean API** - RESTful design with JSON responses
- 📚 **API Documentation** - Interactive Swagger docs at `/swagger`
- 🐳 **Docker ready** - One-command deployment

## 🛠️ Tech Stack

- **Language:** Go 1.21+
- **Database:** PostgreSQL 15
- **Cache:** Redis 7
- **Router:** Chi
- **Deployment:** Docker + Fly.io

## 📁 Project Structure
```
url-shortener/
├── cmd/api/              # Application entry point
├── internal/
│   ├── database/         # Database connection & migrations
│   ├── models/           # Data structures
│   ├── repository/       # Data access layer
│   └── server/           # HTTP handlers & routing
├── migrations/           # SQL schema migrations
├── docs/                 # Swagger documentation
└── docker-compose.yml    # Local development setup
```

## 🚀 Getting Started

### Prerequisites

- Go 1.21+
- Docker & Docker Compose

### Local Development

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/url-shortener.git
cd url-shortener
```

2. **Set up environment**
```bash
cp .env.example .env
# Edit .env with your settings
```

3. **Start services**
```bash
docker-compose up -d
```

4. **Run the application**
```bash
go run cmd/api/main.go
```

5. **Access the API**
- Web UI: http://localhost:8080
- API: http://localhost:8080/api/shorten
- Swagger Docs: http://localhost:8080/swagger/index.html

## 📡 API Endpoints

### Shorten URL
```http
POST /api/shorten
Content-Type: application/json

{
  "url": "https://example.com/very/long/url"
}
```

**Response:**
```json
{
  "short_url": "http://localhost:8080/aB3xK9"
}
```

### Redirect
```http
GET /{shortCode}
```
Redirects to the original URL.

### Get Statistics (TODO)
```http
GET /api/stats/{shortCode}
```

## 🏗️ Architecture

### Caching Strategy
- Redis caches frequently accessed URLs (24h TTL)
- Cache-miss triggers PostgreSQL lookup
- Cache automatically populated on database reads

### Database Schema
```sql
CREATE TABLE urls (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    short_code VARCHAR(10) UNIQUE NOT NULL,
    original_url TEXT NOT NULL,
    user_id UUID,
    created_at TIMESTAMP DEFAULT NOW(),
    expires_at TIMESTAMP,
    click_count INTEGER DEFAULT 0
);
```

## 🧪 Testing
```bash
# Run tests
go test ./...

# With coverage
go test -cover ./...
```

## 🐳 Deployment

### Using Docker
```bash
docker build -t url-shortener .
docker run -p 8080:8080 url-shortener
```

### Using Fly.io
```bash
fly launch
fly deploy
```

## 🎯 Roadmap

- [x] Basic URL shortening
- [x] Redis caching
- [x] PostgreSQL storage
- [x] API documentation
- [ ] User authentication (JWT)
- [ ] Click analytics
- [ ] Custom short codes
- [ ] Rate limiting
- [ ] URL expiration

## 🤝 Contributing

Contributions welcome! Please open an issue first to discuss proposed changes.

## 📝 License

MIT License - see LICENSE file for details

## 👤 Author

**Your Name**
- GitHub: [@yourusername](https://github.com/yourusername)
- LinkedIn: [Your Name](https://linkedin.com/in/yourname)

---

Built with ❤️ using Go
