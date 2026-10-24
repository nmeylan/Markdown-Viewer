# Markdown Viewer

A Markdown editor and viewer with live preview, run locally in Docker and used in your browser (Chromium or Firefox).

A folder from your computer is mounted read-only into the container. Its Markdown files show in the **Disk** workspace and update when they change on disk.

## Requirements

- Docker with Docker Compose v2 (on Windows: Docker Desktop with the WSL 2 backend).

## Run

1. Create a `.env` file next to `docker-compose.yml` and set the folder to mount:

   ```env
   # Linux
   MARKDOWN_DIR=/home/you/notes

   # Windows (use forward slashes)
   MARKDOWN_DIR=C:/Users/you/notes
   ```

   If you do not set `MARKDOWN_DIR`, Docker mounts `./docs` (it creates the folder if it does not exist).

2. Build and start the container:

   ```sh
   docker compose up -d --build
   ```

3. Open <http://localhost:8080>.

The port is bound to `127.0.0.1` only, so other machines on your network cannot open the app.

## Use the mounted folder

- **Disk workspace** (sidebar): shows the `.md` and `.markdown` files of the mounted folder and its subfolders (dotfiles are skipped, maximum 2000 files). Added, removed, and renamed files show in about 5 seconds. Disk documents are read only. To edit one, use **Copy to Workspace** in its menu.
- **Open local file (watch)** (**+** menu, Chromium only): opens any Markdown file on your computer as an editable document. It does not use the mounted folder. The document follows the file: a change on disk updates it in about 2 seconds and replaces your local edits (use Ctrl+Z to get them back). After a page reload, click **Reconnect** to let the browser read the file again. Use **Stop watching file** in the tab menu to unlink it.

The container cannot write to the mounted folder. Your edits stay in the browser storage (IndexedDB) of the browser you use.

## Common tasks

| Task | Command |
|---|---|
| Stop | `docker compose down` |
| Change the mounted folder | Edit `MARKDOWN_DIR` in `.env`, then `docker compose up -d` |
| Update after `git pull` | `docker compose up -d --build` |
| Show logs | `docker compose logs -f` |

To use another port, change `127.0.0.1:8080:80` in `docker-compose.yml`.

## Run without Docker Compose

```sh
docker build -t markdown-viewer .

# Linux
docker run -d --name markdown-viewer -p 127.0.0.1:8080:80 \
  -v /home/you/notes:/usr/share/nginx/html/files:ro markdown-viewer

# Windows (PowerShell)
docker run -d --name markdown-viewer -p 127.0.0.1:8080:80 `
  -v C:/Users/you/notes:/usr/share/nginx/html/files:ro markdown-viewer
```

## License

See [LICENSE](LICENSE).
