# Secundo — Local Watch Marketplace (prototype)

## Run it locally
```
npm install
npm run dev
```
Open the URL it prints (usually http://localhost:5173).

## Deploy for free (no credit card required)

### Option A: Vercel (recommended, easiest)
1. Create a free account at vercel.com (sign in with GitHub, Google, or email).
2. Create a free GitHub account if you don't have one, and create a new repository.
3. Upload this whole folder to that repository (GitHub.com lets you drag-and-drop files in the browser — no command line needed).
4. In Vercel, click "Add New Project," select your repo, and click Deploy.
5. Vercel auto-detects Vite + React and builds it. In ~1 minute you'll get a free live URL like `secundo.vercel.app`.

### Option B: Netlify (just as easy)
1. Create a free account at netlify.com.
2. Drag and drop this folder's contents directly onto the Netlify dashboard ("Deploy manually" / drag-and-drop area) — or connect it to GitHub like Vercel.
3. Netlify builds and gives you a free URL like `secundo.netlify.app`.

Both give you free HTTPS (the padlock) automatically.

## Connecting your real domain (secundowatches.com)
1. Buy the domain from Namecheap, Porkbun, or similar (~$10-15/year).
2. In Vercel or Netlify, go to your project's Domain settings and add your domain.
3. They'll give you DNS records (usually a CNAME or A record) to add at your domain registrar.
4. Add those records at Namecheap/Porkbun's DNS settings. It usually takes a few minutes to a few hours to go live.
