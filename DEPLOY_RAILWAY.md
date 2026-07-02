# Deploy to Railway (quick steps)

1. Create a GitHub repository and push this project:

   ```bash
   git init
   git add .
   git commit -m "Initial commit - KGDownloader"
   git branch -M main
   git remote add origin git@github.com:YOUR_USERNAME/YOUR_REPO.git
   git push -u origin main
   ```

2. In Railway, create a new project and choose **Deploy from GitHub**. Connect your GitHub account and select the repository.

3. Configure services:
   - Backend service: set root to `backend`.
     - Build command: `pip install -r requirements.txt`
     - Start command: use the provided `Procfile` (Railway will run `gunicorn main:app --bind 0.0.0.0:$PORT --workers 1 --threads 4`).
   - Frontend service (optional): set root to project root, build command `npm install && npm run build`, and start command `vite preview --port $PORT --strictPort` or serve static output with a simple static server.

4. Set environment variables in Railway:
   - `COOKIES_CONTENT` (optional): paste cookies contents if you want the server to create `cookies.txt` on start. Prefer using Railway secrets.

5. Deploy and open the public URL Railway provides.

Notes:
- Do NOT commit `cookies.txt` or any sensitive cookies to GitHub. Use `COOKIES_CONTENT` env var on Railway instead.
- Start with placeholders for ads; integrate real ad network only after testing and ensuring policy compliance.
