# Obsidian Setup — see your AI OS as a knowledge graph

Obsidian is optional for daily work, and shines from Module 2 onward (knowledge wiki). Setup takes 3 minutes.

## Install and open the vault

1. Download Obsidian: https://obsidian.md/download → install → open.
2. On the welcome screen choose **"Open folder as vault"**.
3. Pick your AI OS folder: `Documents/AI-OS`.
4. If Obsidian asks about restricted mode / community plugins: keep **restricted mode ON** for now — basic use needs no plugins. (The MCP power-up below is the one exception — it walks you through turning it off safely.)

## What you'll see

- Every note in your OS (profile, tasks, guide, wiki pages) as browsable cards.
- **Graph view** (the icon that looks like connected dots, or Ctrl/Cmd+G): every `[[link]]` between notes drawn as a living map. Early on it looks sparse — that's normal.

## The payoff (Module 2)

Once the knowledge wiki is active, every fact you store links into this graph — the map literally grows with your business. Storing knowledge stays Claude's job in VSCode; Obsidian is where you *see* it.

## Rules of thumb

- Obsidian = reading and exploring. Claude in VSCode = writing and filing.
- Don't move or rename folders from inside Obsidian — `doctor` will flag it if structure drifts.

---

## Power-up: connect Obsidian to Claude (MCP via Local REST API)

Optional. Claude already reads and writes your vault as plain files — this power-up adds direct control of the Obsidian **app**: open a note on screen, search through Obsidian's own index, manage tags/frontmatter without file paths. Skip it until you actually want that.

**Needs:** Node.js 18+ (nodejs.org) · Obsidian installed with this vault open · Claude CLI (`onboard` set this up).

**Easiest way — paste this prompt into Claude and follow along:**

```text
Halo Claude. Bantu aku menyambungkan Obsidian ke Claude lewat MCP (plugin
Local REST API). Kerjakan satu langkah, tunggu hasilnya, baru lanjut:

1. Cek Node.js: jalankan  node --version  — kalau belum ada atau di bawah 18,
   pandu aku install dari nodejs.org dulu.

2. Pandu aku di aplikasi Obsidian: Settings → Community plugins → matikan
   Restricted mode → Browse → cari "Local REST API" → Install → Enable.

3. Masih di settings plugin "Local REST API": pandu aku mengaktifkan server
   HTTP non-encrypted (port 27123) dan menyalin API Key-nya. Aku akan
   menempelkan key itu ke kamu.

4. Daftarkan MCP server-nya dengan perintah ini (ganti KEY dengan punyaku):
   claude mcp add obsidian --scope user --env OBSIDIAN_API_KEY=KEY --env OBSIDIAN_BASE_URL=http://127.0.0.1:27123 -- npx -y obsidian-mcp-server

5. Cek koneksinya (perintah /mcp atau claude mcp list), lalu TES: cari satu
   catatan di vault-ku lewat Obsidian dan buka di aplikasinya. Kalau
   catatannya terbuka di layar = berhasil.

Catatan penting: jelaskan padaku bahwa Obsidian harus dalam keadaan TERBUKA
supaya koneksi ini hidup, dan API key ini kunci vault-ku — jangan pernah
disimpan di file yang ikut ke git atau dibagikan ke siapa pun.
```

**Troubleshooting:**

| Problem | Fix |
|---|---|
| Claude says the Obsidian server is unreachable | Obsidian must be open (the plugin only runs while the app runs) |
| Connection refused on 27123 | In plugin settings, "Enable Non-encrypted (HTTP) Server" must be on |
| Certificate errors | You used the HTTPS port (27124, self-signed cert). Either switch to HTTP 27123 as above, or add `--env OBSIDIAN_VERIFY_SSL=false` |
| Key leaked or lost | Plugin settings → regenerate the API key → re-run step 4 with the new key |
