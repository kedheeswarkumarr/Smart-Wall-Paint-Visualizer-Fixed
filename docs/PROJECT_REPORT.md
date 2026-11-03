# Smart Wall Paint Visualizer — Project Report

## 1. Introduction
HueRoom is a web-based application for previewing paint colors on user-provided room photographs. The first phase focuses on manual wall-area selection and translucent color overlays.

## 2. Problem Statement
Customers often struggle to predict how a paint shade will look in their own rooms. Printed shade cards and showroom lighting cannot fully represent the lighting, scale and surface texture of a real room.

## 3. Objectives
- Preview paint colors on room photos.
- Support manual wall selection and adjustable paint opacity.
- Compare color options and download a preview.
- Save designs for later reference.
- Provide user and administrator roles.

## 4. Scope
Included: authentication, photo upload, polygon wall selection, color palette, opacity control, preview, PNG download, saved projects, admin color management and basic usage counts. Excluded from phase one: AI wall detection, AR/VR, mobile app and direct paint purchasing.

## 5. Technology Stack
- Frontend: Angular 18, TypeScript, HTML, CSS and HTML Canvas.
- Backend: Node.js and Express REST API.
- Database: MongoDB with Mongoose.
- Security: bcrypt password hashing, JWT authentication, Helmet, rate limiting and upload validation.

## 6. Main Modules
1. Authentication: registration and login.
2. Room upload: JPG/PNG/WEBP upload with an 8 MB limit.
3. Wall selection: manual polygon points on a canvas.
4. Color preview: HEX colors and opacity overlay.
5. Project management: save, list and delete user designs.
6. Admin: add paint colors and view user/project/color totals.

## 7. Data Design
Users store name, email, password hash and role. Colors store name, HEX value, brand, finish and category. Projects store owner, title, image path, polygon coordinates, color and opacity.

## 8. User Flow
Register/sign in → upload a room photo → outline a wall → choose a shade → adjust opacity → preview → download or save.

## 9. Security and Constraints
Passwords are hashed. API routes use JWT authentication and admin checks. Upload MIME types and size are validated. Production requires HTTPS, strong environment secrets, a managed MongoDB database and persistent/object storage for images. The preview is illustrative, not a guarantee of the final painted appearance.

## 10. KPIs
The admin endpoint reports registered user count, saved project count and available color count. Additional analytics such as session duration and satisfaction surveys are not implemented in this phase.

## 11. Future Enhancements
AI wall segmentation, perspective-aware lighting, texture/pattern application, day/night simulation, AR preview, retailer integrations and richer analytics.

## 12. Deliverables
Source code, API, Angular web interface, MongoDB schemas, user guide, developer setup documentation and this report. A deployed public demo must be configured and tested separately using the deployment guide.

## AI-based wall detection module
The updated frontend integrates browser-side semantic segmentation with Transformers.js and the SegFormer ADE20K model (`Xenova/segformer-b0-finetuned-ade-512-512`). After a room image is uploaded, the user can run AI detection. The application extracts segmentation regions labelled `wall`, converts them into a pixel mask matching the displayed canvas, and applies the selected paint shade only to pixels in that mask. The existing manual polygon selection remains available as a fallback and correction method. This module requires an internet connection for initial model download and has model accuracy limitations; it is not a guarantee of perfectly bounded wall detection.
