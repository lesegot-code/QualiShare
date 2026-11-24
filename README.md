# QualiShare

**Share every pixel.**

QualiShare (short for Quality Share) is a simple site for sharing photos and videos with family and friends in **full quality**. Upload from your phone or computer, and everyone can download the original file, exactly as it was taken. Files can also be deleted. Nothing is compressed.

No database: files are saved in a folder on the computer running the site.

## How it works

- **client/**: React + Vite + Tailwind. The page you see.
- **server/**: Express + Multer. Receives uploads and sends downloads.
- **server/data/**: everything that's been shared (created automatically):
  - `uploads/`: the original photos and videos
  - `posts.json`: who uploaded what, with captions

The client forwards `/api` and `/uploads` to the server, so you only need to share **one** link.

## Run it with Docker (recommended)

You need [Docker Desktop](https://www.docker.com/products/docker-desktop/).

```bash
docker compose up -d --build
```

Open http://localhost:5173.

To stop it:

```bash
docker compose down
```

Your uploads stay in `server/data/`, so they're all still there next time you start it.

## Run it without Docker

Two terminals:

```bash
# Terminal 1
cd server
npm install
npm start
```

```bash
# Terminal 2
cd client
npm install
npm run dev
```

Open http://localhost:5173.

## Share it with others (Cloudflare quick tunnel)

Install `cloudflared` once (Windows: `winget install Cloudflare.cloudflared`), then with the site running:

```bash
cloudflared tunnel --url http://localhost:5173
```

It prints a link like `https://some-random-words.trycloudflare.com`. Send that to whoever you want to share with.

- Keep the computer awake and both the site and the tunnel running while people are using it.
- The link changes every time you start the tunnel.
- Anyone with the link can upload, download **and delete**, so only share it with people you trust.
- Press **Ctrl+C** in the tunnel window to take the link down.

## Good to know

- **File size:** the server accepts up to 500 MB per file. Cloudflare's free tunnels limit each upload to about **100 MB**, so trim very long videos or split them into shorter clips.
- **iPhone photos:** when uploading from an iPhone, the photo picker may convert HEIC photos to JPEG. To keep the original format, tap **Options** in the picker and choose **Current**.
- **No preview?** Some files (HEIC photos, some iPhone `.mov` videos) can't be shown in every browser. The **Download** button still gives the full-quality original.
- **Location data:** photos keep their original info, including where they were taken.
- **Backups:** to back everything up, copy the `server/data` folder.
- **Deleting:** the **Delete** button removes the file from `server/data/uploads/` and from the list, for everyone. It can't be undone, so back up `server/data` if you're unsure.
