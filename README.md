# Photography Drive Sync - Test Site

This is a **dummy showcase website** designed specifically to test and demonstrate the dynamic loading of images from a Google Drive folder.

## 🚀 Purpose
The primary goal of this project is to verify if a static HTML website can successfully fetch and display images from a public Google Drive folder automatically, without requiring any manual code updates when the folder content changes.

## 🛠 Features
- **Dynamic Google Drive Sync**: The "Featured Moments" section uses a custom JavaScript scraper to pull all image IDs from a specific Drive folder.
- **Auto-Update**: Any image added to the Drive folder will appear on the site upon refresh.
- **Premium UI**: Built with Bootstrap 5, AOS (Animate On Scroll), and custom CSS for a high-end photography portfolio look.
- **Skeleton Loading**: Includes animated placeholders that show while the cloud data is being synced.

## ⚠️ Important Notes for Testing
1. **CORS Security**: Due to browser security policies, the dynamic syncing feature will likely be blocked if you open `index.html` directly from your local computer (`file://` protocol).
2. **Hosting is Required**: To see the "Perfect Sync" in action, this project **must be hosted** on a web server (e.g., GitHub Pages, Netlify, or a local dev server like Live Server in VS Code).
3. **Folder Permissions**: The Google Drive folder MUST be set to **"Anyone with the link can view"** for the scraper to work.

## 📁 Project Structure
- `index.html`: Main website structure and dynamic sync logic.
- `style.css`: Custom design tokens and premium aesthetics.
- `assets/`: Local fallbacks for the main gallery.
- `assets/gallery/`: Local test images.

---
*Created as a technical proof-of-concept for dynamic cloud-to-static-site synchronization.*
