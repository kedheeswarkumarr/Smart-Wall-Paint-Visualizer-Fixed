# Deployment checklist

- [ ] MongoDB Atlas database created; network access and database user configured.
- [ ] API deployed from `server/` with `MONGODB_URI`, `JWT_SECRET` (32+ chars), `CLIENT_ORIGIN`, `ADMIN_EMAIL`, `ADMIN_PASSWORD`.
- [ ] Persistent storage or object storage configured for `/uploads` (ephemeral files can disappear on restart/redeploy).
- [ ] Frontend `client/src/assets/config.js` points to the deployed API origin.
- [ ] Angular build deployed from `client/`, publish directory `dist/visualizer/browser`.
- [ ] API `CLIENT_ORIGIN` matches exact frontend HTTPS origin.
- [ ] Test registration, login, upload, polygon preview, download, save, delete, admin login and add color.
- [ ] Confirm no `.env`, credentials, or private room photos are committed to GitHub.
