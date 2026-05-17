# Virtualization and System Integration

## Getting Started

### Development
To start the project in development mode with hot-reloading:
```bash
docker compose -f docker-compose.dev.yml up --build
```
Access the application at [http://localhost:3000](http://localhost:3000).

### Production
To start the project in production mode with Nginx reverse proxy:
```bash
docker compose up --build
```
Access the application at [http://localhost](http://localhost) (or the port defined in `NGINX_PORT` in your `.env` file).

## Environment Configuration
Make sure to create a `.env` file in the root directory. You can use `example.env` as a template:
```bash
cp example.env .env
```
