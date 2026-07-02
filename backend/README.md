# Video Downloader API (Flask)

Small Flask backend using `yt-dlp` to extract video download links and proxy downloads.

## Local Development

1. Create a virtual environment:
   ```bash
   cd backend
   python -m venv venv
   # On Windows:
   venv\Scripts\activate
   # On macOS / Linux:
   source venv/bin/activate
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Run the server for development (Flask builtin server):
   ```bash
   python main.py
   ```

4. Or use `gunicorn` (recommended for production/local testing):
   ```bash
   gunicorn main:app --bind 0.0.0.0:8000 --workers 1 --threads 4
   ```

5. The API will be available at `http://localhost:8000`

## API Endpoints

### GET /
Health check endpoint. Returns JSON with `message` and `status`.

### POST /api/download
Extract video download information from a URL.

Request body:
```json
{ "url": "https://www.tiktok.com/@.../video/..." }
```

Response (example):
```json
{
  "title": "Video Title",
  "thumbnail": "https://...",
  "download_url": "/api/proxy?url=https%3A%2F%2F..."
}
```

### GET /api/proxy?url=<encoded_url>
Proxies the video at `<encoded_url>` and streams it to the client with an `attachment` filename.

## Cookies

The server reads `cookies.txt` from the backend working directory if present. You may also provide cookies via the `COOKIES_CONTENT` environment variable (useful when deploying).

## Deployment (Railway)

1. Ensure `backend/requirements.txt` and `backend/Procfile` are present (they are included).
2. In Railway, add a new project and connect your Git repository.
3. Railway will detect Python; set the service to use the `backend` directory as the root, or create a separate project for the backend.
4. Set environment variables (optional):
   - `COOKIES_CONTENT`: (optional) contents of `cookies.txt` so the container can create the file on start.
5. Railway will run the `Procfile` command: `gunicorn main:app --bind 0.0.0.0:$PORT --workers 1 --threads 4`.

Notes:
- `yt-dlp` may require extra system dependencies for some extractors; if you encounter extractor-specific errors on Railway, check the build logs and install any missing packages.
- For production, consider running inside a container and increase `--workers` based on CPU.

## License / Notes
This project is intended for personal or internal use. Respect the terms of service of target platforms when downloading content.
