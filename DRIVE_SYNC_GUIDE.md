# Google Drive Image Sync Guide

This guide explains how to implement a fully dynamic image synchronization system between a Google Drive folder and a static HTML website. This solution requires **zero maintenance**—simply add photos to your Drive folder, and they appear on your site instantly.

## 1. Prerequisites (Drive Setup)
For the sync to work, your Google Drive folder must be public:
1.  Right-click your folder in Google Drive.
2.  Select **Share** > **Get link**.
3.  Change permission to **"Anyone with the link can view"**.
4.  Copy the **Folder ID** from the URL (the long string of letters and numbers after `/folders/`).

---

## 2. The Implementation (Code Snippet)
Copy this logic into your `index.html` file before the closing `</body>` tag.

```javascript
async function loadDriveImages() {
    // 1. YOUR CONFIGURATION
    const folderId = 'YOUR_FOLDER_ID_HERE'; 
    const container = document.getElementById('your-container-id');
    
    // 2. RELIABLE PROXIES (Bypasses CORS blocks)
    const target = `https://drive.google.com/drive/folders/${folderId}`;
    const proxies = [
        `https://api.codetabs.com/v1/proxy?quest=${encodeURIComponent(target)}`,
        `https://corsproxy.io/?${encodeURIComponent(target)}`,
        `https://api.allorigins.win/get?url=${encodeURIComponent(target)}`
    ];

    let html = null;
    for (let proxy of proxies) {
        try {
            const response = await fetch(proxy);
            html = proxy.includes('allorigins') ? (await response.json()).contents : await response.text();
            if (html && html.includes('image/')) break; 
        } catch (e) { console.warn("Proxy attempt failed"); }
    }

    if (!html) return;

    // 3. UNIVERSAL REGEX (Extracts File IDs from Google's data structure)
    const regex = /(?:"|&quot;)([a-zA-Z0-9_-]{28,})(?:"|&quot;)\]\s*,\s*null\s*,\s*null\s*,\s*null\s*,\s*(?:"|&quot;)image\//g;
    const ids = [];
    let match;
    while ((match = regex.exec(html)) !== null) {
        ids.push(match[1]);
    }

    // 4. DISPLAY IMAGES
    const uniqueIds = [...new Set(ids)];
    if (uniqueIds.length > 0) {
        container.innerHTML = ''; // Clear loaders
        uniqueIds.forEach(id => {
            const imgUrl = `https://lh3.googleusercontent.com/u/0/d/${id}`;
            container.innerHTML += `<div class="item"><img src="${imgUrl}"></div>`;
        });
    }
}
window.addEventListener('load', loadDriveImages);
```

---

## 3. How It Works (The "Secret Sauce")
-   **Scraping vs API**: This solution uses "Scraping" which doesn't require a paid Google API Key or OAuth. It reads the public data Google uses to show the folder in a browser.
-   **Multi-Proxy Fallback**: Browsers block requests from one website to another (CORS). We use public proxies to hide our identity and "trick" the browser into allowing the data fetch.
-   **High-Resolution Thumbnails**: Instead of loading the full multi-megabyte photo, we use the `lh3.googleusercontent.com` endpoint which is optimized for web performance.

---

## 4. Common Issues & Fixes
| Issue | Cause | Fix |
| :--- | :--- | :--- |
| **"No images found"** | Folder is private. | Set folder to "Anyone with the link can view". |
| **Broken Images** | Hotlinking Block. | Ensure you are using the `lh3.googleusercontent.com` URL format. |
| **Sync delayed locally** | Browser Security (CORS). | Host the project on GitHub Pages, Vercel, or Netlify. |
| **Slow loading** | Large image count. | Google Drive returns a max of ~50-100 images per folder view. |

---

## 5. Reusing in New Projects
1.  **Paste the CSS**: Ensure your container has `display: flex` or a grid layout.
2.  **Paste the JS**: Update the `folderId` and `container` ID.
3.  **Host it**: Push to GitHub/Vercel to activate the proxies.

*Documented by Antigravity for future use.*
