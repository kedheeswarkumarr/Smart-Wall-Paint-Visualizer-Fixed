# HueRoom — Smart Wall Paint Visualizer

A MEAN-stack starter implementation for the Unified Mentor Smart Wall Paint Visualizer project. It includes an Angular 18 frontend, Express/Node REST API, MongoDB/Mongoose models, JWT authentication, manual polygon wall selection, paint overlay preview, image upload, saved designs, color library and admin color/statistics endpoints.

## Features
- Register and sign in; passwords are hashed with bcrypt.
- Upload JPG, PNG or WEBP room images (8 MB maximum).
- Click wall corners on the canvas; double-click to finish the polygon.
- Choose a paint shade and opacity, preview, download the PNG preview.
- Save designs to the signed-in user's account.
- Admin role can add shades and view user/project/color counts.
- Responsive desktop/tablet/mobile layout.

## Requirements
- Node.js 20 or 22 LTS
- MongoDB local instance or MongoDB Atlas database

## Run locally
1. Install MongoDB and ensure it is running, or create a MongoDB Atlas database.
2. In `server`, copy `.env.example` to `.env` and fill in `MONGODB_URI`, a random `JWT_SECRET` of at least 32 characters, and the admin credentials.
3. Install dependencies:
   ```bash
   cd server && npm install
   ```
4. Start API:
   ```bash
   npm run dev
   ```
   API health check: http://localhost:5000/api/health
5. In another terminal:
   ```bash
   cd client && npm install && npm start
   ```
6. Open http://localhost:4200.

## First admin account
At API startup, if `ADMIN_EMAIL` and `ADMIN_PASSWORD` are set and the email does not already exist, the server creates an admin account. Use a unique password of at least 12 characters. Do not commit `.env` or share secrets.

## Deploy
Deploy the API and Angular frontend separately:

### API (Render)
- Create a Web Service from the GitHub repository.
- Root Directory: `server`
- Build Command: `npm install`
- Start Command: `npm start`
- Environment variables: `MONGODB_URI` (Atlas connection string), `JWT_SECRET` (32+ random characters), `CLIENT_ORIGIN` (deployed frontend URL), `ADMIN_EMAIL`, `ADMIN_PASSWORD`, optional `ADMIN_NAME`.
- Add a persistent disk mounted at `/opt/render/project/src/uploads` if you want uploaded image files to survive service restarts. For a production system, use object storage (S3/Cloudinary) rather than local disk.

### Frontend (Netlify or Vercel)
- Build from repository root `client`.
- Build command: `npm install && npm run build`.
- Publish directory: `dist/visualizer/browser`.
- Edit `client/src/assets/config.js` before deployment and set `window.__HUEROOM_API__` to the deployed API origin, e.g. `https://your-api.onrender.com` (no trailing `/api`).
- Set the API's `CLIENT_ORIGIN` to the frontend's exact HTTPS URL.

## API overview
- `POST /api/auth/register`, `POST /api/auth/login`
- `GET /api/colors`; admin: `POST/PUT/DELETE /api/colors`
- `POST /api/projects/upload`, `POST /api/projects`, `GET /api/projects`, `DELETE /api/projects/:id`
- admin: `GET /api/admin/stats`
- `GET /api/health`

## Important implementation notes / limitations
- Wall selection is manual polygon selection (Phase 1), not AI wall detection.
- The preview uses a translucent flat color overlay inside the selected polygon. It does not yet perform perspective/lighting/texture-aware rendering; screen calibration, room lighting and wall texture affect actual paint appearance.
- Pattern textures, automatic wall segmentation, AR/VR, payments and retailer integrations are future enhancements.
- The included image upload stores files on the API server. Configure persistent storage or object storage for production deployments.
- This is a project implementation starter. Before public production use, add email verification, password reset, stronger upload scanning, cloud object storage, audit logging, automated tests and a privacy/retention policy.

## AI wall detection (added)

The Visualizer Studio includes browser-based semantic segmentation using the Hugging Face Transformers.js package and the `Xenova/segformer-b0-finetuned-ade-512-512` ADE20K model. Choose **Detect walls with AI** after uploading a room image. The model identifies pixels labelled `wall`, builds a wall mask, and the paint preview blends the selected shade into that mask while retaining the underlying image texture. The first detection requires an internet connection to download the model and may take longer. If the model cannot load or does not identify a wall, users can continue with manual polygon selection.

The model is a general indoor-scene segmentation model, not a purpose-trained paint application model. Results may include incomplete or incorrect wall boundaries; users should inspect the mask and use manual selection where needed. Images are processed in the browser for this feature; the model is downloaded from the Hugging Face model repository. No AI API key is required.
