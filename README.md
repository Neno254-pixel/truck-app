# Route Compliance Manager

A professional logistics compliance tracking system that generates Hours of Service (HOS) compliant logs with interactive route visualization and comprehensive reporting.

## Overview

Route Compliance Manager is a local development application designed for managing driver hours, route planning, and compliance tracking. The system automatically enforces HOS regulations and provides detailed logging capabilities.

## Features

### Core Functionality

- **Intelligent Route Planning** - Calculate optimal routes between multiple locations
- **HOS Compliance Engine** - Automatic enforcement of 70hrs/8days rule
- **Interactive Route Visualization** - Real-time map display with Leaflet integration
- **Professional PDF Reports** - Generate DOT-compliant log sheets
- **Sleeper Berth Management** - Support for split sleeper berth provisions
- **Smart Stop Planning** - Automatic fueling stops every 1000 miles
- **Entry Optimization** - Intelligent merging of adjacent log entries

### Technical Highlights

- Real-time route calculation using OSRM API
- Advanced geocoding with Nominatim
- RESTful API architecture with Django REST Framework
- Modern React-based user interface
- SQLite database for local development

## Architecture

```
truck-log-app/
├── backend/              # Django REST API
│   ├── core/            # Core application logic
│   │   ├── models.py    # Data models (Trip, LogEntry)
│   │   ├── services.py  # Business logic & HOS compliance
│   │   ├── routing.py   # OSRM route integration
│   │   ├── geocoding.py # Location services
│   │   └── views.py     # API endpoints
│   └── trucklog/        # Django configuration
└── frontend/            # React application
    └── src/
        ├── components/  # UI components
        │   ├── TripForm.jsx
        │   ├── MapView.jsx
        │   ├── ResultPanel.jsx
        │   └── LogGridSvg.jsx
        └── App.js       # Main application
```

## Quick Start Guide

### Prerequisites

- Python 3.8 or higher
- Node.js 14 or higher
- npm or yarn package manager

### Backend Setup

1. Navigate to the backend directory:

```bash
cd backend
```

2. Create and activate a virtual environment:

```bash
# On Windows
python -m venv venv
venv\Scripts\activate

# On macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Run database migrations:

```bash
python manage.py migrate
```

5. Start the development server:

```bash
python manage.py runserver
```

The backend API will be available at: `http://localhost:8000`

### Frontend Setup

1. Navigate to the frontend directory:

```bash
cd frontend
```

2. Install dependencies:

```bash
npm install
```

3. Start the development server:

```bash
npm start
```

The frontend application will be available at: `http://localhost:3000`

## API Documentation

### Create Trip Endpoint

**Endpoint:** `POST /api/trip/`

**Request Body:**

```json
{
  "current_location": "Green Bay, WI",
  "pickup_location": "Fond du Lac, WI",
  "dropoff_location": "Edwardsville, IL",
  "current_cycle_hours": 5.0,
  "use_sleeper_berth": false
}
```

**Response:**

```json
{
  "trip_id": 1,
  "total_distance": 288.3,
  "total_time": 10.5,
  "logs": [
    {
      "date": "2025-01-15",
      "time": "08:00",
      "status": "ON_DUTY",
      "location": "Green Bay, WI",
      "remarks": "Starting trip"
    }
  ],
  "route_coordinates": [[43.0731, -89.4012], ...],
  "pdf_url": "/media/logs/trip_1_2025-01-15.pdf",
  "compliance_status": "COMPLIANT"
}
```

### Location Search Endpoint

**Endpoint:** `GET /api/locations/search/?q={query}`

**Example:**

```bash
curl "http://localhost:8000/api/locations/search/?q=Chicago"
```

## HOS Compliance Rules

### 70-Hour/8-Day Rule

- Maximum 70 hours on-duty in any 8 consecutive days
- System automatically tracks and enforces limits
- Prevents scheduling violations

### 11-Hour Driving Limit

- Maximum 11 hours of driving after 10 consecutive hours off-duty
- Automatically enforced per day

### 14-Hour Driving Window

- All driving must occur within 14 hours of coming on-duty
- Cannot be extended with off-duty periods

### 30-Minute Break Requirement

- Required after 8 cumulative hours of driving
- Must be at least 30 minutes off-duty or sleeper berth

### Sleeper Berth Provision (Optional)

- Split sleeper berth: 7 hours + 3 hours (or 8/2)
- Pauses the 14-hour driving window
- Can be enabled per trip

## Testing

### Backend Tests

Run the test suite:

```bash
cd backend
python manage.py test
```

### Manual API Testing

Test the trip creation endpoint:

```bash
curl -X POST http://localhost:8000/api/trip/ \
  -H "Content-Type: application/json" \
  -d '{
    "current_location": "Green Bay, WI",
    "pickup_location": "Fond du Lac, WI",
    "dropoff_location": "Edwardsville, IL",
    "current_cycle_hours": 5
  }'
```

## Environment Configuration

### Backend Environment Variables

Create a `.env` file in the `backend` directory (optional):

```env
DEBUG=True
SECRET_KEY=your-secret-key-here
ALLOWED_HOSTS=localhost,127.0.0.1
```

### Frontend Environment Variables

Create a `.env` file in the `frontend` directory (optional):

```env
REACT_APP_API_URL=http://localhost:8000
```

## Technology Stack

### Backend

- **Framework:** Django 5.2
- **API:** Django REST Framework 3.16
- **PDF Generation:** ReportLab
- **Geocoding:** Geopy
- **HTTP Client:** Requests
- **Database:** SQLite (development)

### Frontend

- **Framework:** React 18
- **HTTP Client:** Axios
- **Mapping:** Leaflet & React-Leaflet
- **Styling:** Custom CSS with modern design system

## Project Structure

```
backend/
├── core/
│   ├── models.py          # Database models
│   ├── views.py           # API views
│   ├── serializers.py     # Data serialization
│   ├── services.py        # Business logic
│   ├── routing.py         # Route calculation
│   ├── geocoding.py       # Location services
│   └── urls.py            # URL routing
├── trucklog/
│   ├── settings.py        # Django settings
│   └── urls.py            # Main URL config
├── manage.py              # Django management
└── requirements.txt       # Python dependencies

frontend/
├── public/
│   └── index.html         # HTML template
├── src/
│   ├── components/        # React components
│   ├── App.js             # Main app component
│   ├── App.css            # Styling
│   └── index.js           # Entry point
└── package.json           # Node dependencies
```

## Troubleshooting

### Backend Issues

**Port already in use:**

```bash
# Use a different port
python manage.py runserver 8001
```

**Database errors:**

```bash
# Reset database
python manage.py flush
python manage.py migrate
```

### Frontend Issues

**Port already in use:**

```bash
# Set custom port
PORT=3001 npm start
```

**API connection errors:**

- Ensure backend is running on `http://localhost:8000`
- Check CORS settings in `backend/trucklog/settings.py`

## Development Tips

1. **Hot Reload:** Both frontend and backend support hot reload during development
2. **API Testing:** Use tools like Postman or curl for API testing
3. **Database:** SQLite database file is located at `backend/db.sqlite3`
4. **Logs:** Check console output for debugging information
5. **PDF Files:** Generated PDFs are stored in `backend/media/logs/`

## Contributing

This is a local development project. To contribute:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

MIT License - Free to use for personal and commercial projects.

## Hosting Guide

### Option 1: Local Network Hosting

To make the application accessible on your local network:

#### Backend:

```bash
cd backend
python manage.py runserver 0.0.0.0:8000
```

#### Frontend:

```bash
cd frontend
# Update package.json or use environment variable
HOST=0.0.0.0 npm start
```

Access from other devices on your network using your computer's IP address (e.g., `http://192.168.1.100:3000`)

### Option 2: Cloud Hosting (Production)

#### Backend Hosting Options:

**Railway:**

1. Install Railway CLI: `npm install -g @railway/cli`
2. Login: `railway login`
3. Initialize: `railway init`
4. Add environment variables in Railway dashboard:
   - `DEBUG=False`
   - `SECRET_KEY=your-production-secret-key`
   - `ALLOWED_HOSTS=your-domain.railway.app`
5. Deploy: `railway up`

**Heroku:**

1. Create `Procfile` in backend directory:
   ```
   web: gunicorn trucklog.wsgi --log-file -
   ```
2. Add gunicorn to requirements.txt: `gunicorn>=21.2.0`
3. Update settings.py for production:
   ```python
   import os
   DEBUG = os.environ.get('DEBUG', 'False') == 'True'
   ALLOWED_HOSTS = os.environ.get('ALLOWED_HOSTS', '').split(',')
   ```
4. Deploy:
   ```bash
   heroku login
   heroku create your-app-name
   git push heroku main
   ```

**DigitalOcean/AWS/Azure:**

1. Set up a virtual machine
2. Install Python, pip, and dependencies
3. Configure Nginx as reverse proxy
4. Use Gunicorn as WSGI server
5. Set up SSL with Let's Encrypt

#### Frontend Hosting Options:

**Vercel:**

1. Install Vercel CLI: `npm install -g vercel`
2. Update API URL in frontend code to your backend URL
3. Deploy:
   ```bash
   cd frontend
   vercel login
   vercel --prod
   ```

**Netlify:**

1. Build the app: `npm run build`
2. Deploy via Netlify CLI or drag-and-drop the `build` folder
3. Set environment variable: `REACT_APP_API_URL=your-backend-url`

**GitHub Pages:**

1. Install gh-pages: `npm install --save-dev gh-pages`
2. Add to package.json:
   ```json
   "homepage": "https://yourusername.github.io/repo-name",
   "scripts": {
     "predeploy": "npm run build",
     "deploy": "gh-pages -d build"
   }
   ```
3. Deploy: `npm run deploy`

### Production Configuration Checklist:

#### Backend:

- [ ] Set `DEBUG=False` in production
- [ ] Use strong `SECRET_KEY`
- [ ] Configure `ALLOWED_HOSTS` with your domain
- [ ] Set up CORS for your frontend domain
- [ ] Use PostgreSQL instead of SQLite for production
- [ ] Configure static file serving (WhiteNoise or CDN)
- [ ] Set up SSL/HTTPS
- [ ] Configure logging
- [ ] Set up database backups

#### Frontend:

- [ ] Update API URL to production backend
- [ ] Build optimized production bundle
- [ ] Configure environment variables
- [ ] Set up CDN for static assets
- [ ] Enable HTTPS
- [ ] Configure caching headers

### Environment Variables for Production:

**Backend (.env):**

```env
DEBUG=False
SECRET_KEY=your-very-secure-secret-key-here
ALLOWED_HOSTS=yourdomain.com,www.yourdomain.com
DATABASE_URL=postgresql://user:password@host:port/dbname
CORS_ALLOWED_ORIGINS=https://yourdomain.com,https://www.yourdomain.com
```

**Frontend (.env.production):**

```env
REACT_APP_API_URL=https://api.yourdomain.com
```

### Docker Deployment (Optional):

**Backend Dockerfile:**

```dockerfile
FROM python:3.9
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["gunicorn", "trucklog.wsgi:application", "--bind", "0.0.0.0:8000"]
```

**Frontend Dockerfile:**

```dockerfile
FROM node:16 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**docker-compose.yml:**

```yaml
version: "3.8"
services:
  backend:
    build: ./backend
    ports:
      - "8000:8000"
    environment:
      - DEBUG=False
      - DATABASE_URL=postgresql://postgres:password@db:5432/routemanager

  frontend:
    build: ./frontend
    ports:
      - "80:80"
    depends_on:
      - backend

  db:
    image: postgres:13
    environment:
      - POSTGRES_DB=routemanager
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

Deploy with: `docker-compose up -d`

## Support

For issues or questions:

- Check the troubleshooting section above
- Review the code documentation
- Test with the provided examples

---

**Route Compliance Manager - Professional logistics compliance tracking**
