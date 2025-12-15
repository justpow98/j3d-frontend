# J3D - Etsy Orders & Filament Tracker

![Backend Build](https://github.com/justpow98/j3d-backend/actions/workflows/docker-build.yml/badge.svg)
![Frontend Build](https://github.com/justpow98/j3d-frontend/actions/workflows/docker-build.yml/badge.svg)

Full-stack application for managing Etsy orders and tracking 3D printer filament inventory.

## 🚀 Quick Start with Docker

### Prerequisites
- Docker and Docker Compose installed
- Etsy API credentials ([Get them here](https://www.etsy.com/developers))

### One-Command Deploy

```bash
# Clone both repositories (or use this if they're in one repo)
# Create .env file with your credentials
cat > .env << EOF
ETSY_CLIENT_ID=your_client_id
ETSY_CLIENT_SECRET=your_client_secret
SECRET_KEY=$(openssl rand -base64 32)
ETSY_REDIRECT_URI=http://localhost:4200/oauth-callback
EOF

# Start everything
docker-compose up -d

# Access the app
open http://localhost:4200
```

That's it! The app is now running at `http://localhost:4200`

## 📦 Docker Images

Both frontend and backend automatically build on push to main:

**Backend:** `ghcr.io/justpow98/j3d-backend:latest`
**Frontend:** `ghcr.io/justpow98/j3d-frontend:latest`

## 🏗️ Architecture

```
┌─────────────────────┐
│   Angular Frontend  │  Port 4200
│   (nginx + SPA)     │
└──────────┬──────────┘
           │ /api/* proxy
           ▼
┌─────────────────────┐
│   Flask Backend     │  Port 5000
│   (Python + SQLite) │
└─────────────────────┘
           │
           ▼
┌─────────────────────┐
│   Etsy API          │
│   (OAuth + Orders)  │
└─────────────────────┘
```

## ✨ Features

- ✅ **Etsy Integration** - 3-legged OAuth with automatic token refresh
- ✅ **Order Management** - Sync and track orders from last 6 months
- ✅ **Filament Tracking** - Inventory management for multiple spools
- ✅ **Usage Logging** - Track filament used per order
- ✅ **Auto-Deploy** - Push to main → automatic Docker build
- ✅ **Multi-Arch** - Works on Intel and ARM (Raspberry Pi, M1/M2)

## 🛠️ Tech Stack

**Backend:**
- Python 3.11 + Flask
- SQLAlchemy ORM
- JWT authentication
- Etsy API v3

**Frontend:**
- Angular 17 (standalone components)
- TypeScript
- SCSS styling
- RxJS

**DevOps:**
- Docker multi-stage builds
- GitHub Actions CI/CD
- GitHub Container Registry
- Nginx reverse proxy

## 📖 Documentation

- [Backend Docker Setup](./j3d-backend/README.md)
- [Frontend Docker Setup](./j3d-frontend/FRONTEND_DOCKER_SETUP.md)
- [Complete Setup Guide](./COMPLETE_SETUP.md)

## 🚢 Deployment Options

### Development
```bash
# Backend
cd j3d-backend
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
python app.py

# Frontend
cd j3d-frontend
npm install && npm start
```

### Docker Compose (Recommended)
```bash
docker-compose up -d
```

### Kubernetes
```bash
kubectl apply -f k8s/
```

### Cloud Platforms
- AWS ECS
- Google Cloud Run
- Azure Container Instances
- DigitalOcean App Platform

Pull the images and deploy anywhere Docker runs!

## 🔧 Configuration

### Required Environment Variables

```env
ETSY_CLIENT_ID=your_etsy_client_id
ETSY_CLIENT_SECRET=your_etsy_client_secret
ETSY_REDIRECT_URI=http://localhost:4200/oauth-callback
SECRET_KEY=your-super-secret-key
```

### Optional Variables

```env
FLASK_ENV=production
DATABASE_URL=sqlite:///j3d.db  # or postgresql://...
```

## 🔐 Security

- JWT tokens for session management
- Non-root Docker containers
- Security headers configured
- HTTPS ready (add reverse proxy)
- Secrets via environment variables

## 📊 Database

Default: SQLite (file-based, included)
Production: PostgreSQL or MySQL supported

Update `DATABASE_URL` in `.env` to switch.

## 🐛 Troubleshooting

### Backend won't start?
```bash
docker logs j3d-backend
# Check ETSY credentials in .env
```

### Frontend can't reach API?
```bash
docker logs j3d-frontend
# Check if both containers are on same network
docker network inspect j3d_default
```

### OAuth redirect fails?
Ensure `ETSY_REDIRECT_URI` matches exactly in:
1. Your `.env` file
2. Etsy developer console

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Push to main → Docker image auto-builds!
5. Create a Pull Request

## 📝 License

GNU General Public License v3.0

## 🙏 Credits

Built with:
- [Flask](https://flask.palletsprojects.com/)
- [Angular](https://angular.io/)
- [Etsy API](https://www.etsy.com/developers)
- [Docker](https://www.docker.com/)

---

**Made with ❤️ for 3D printing enthusiasts**

Questions? Open an issue!