# Tools & MCP Setup Prompt (full install — nothing skipped)

One paste-able prompt that makes Claude install and verify the **complete OTIUM tool stack**, mandatory end-to-end: **Git CLI** → **Claude CLI** → **MCP connectors** (Google Calendar + any service the user names) → **Obsidian app + Obsidian MCP** (Local REST API) → **Superpowers + Context-Mode plugins** — closed by a final verification checklist Claude must pass before declaring setup done.

Use it at install time, after a laptop change, or to re-verify a machine. Every section self-checks first, so parts that are already healthy pass through quickly.

How to use: copy the block below, paste it into any Claude chat (VSCode extension or terminal), press Enter, follow the conversation.

```text
Halo Claude. Kamu adalah pemandu setup untuk pemula non-teknis. Tugasmu:
memasang SEMUA yang ada di daftar ini sampai lengkap — LIMA bagian, tidak
ada yang boleh dilewati. Kerjakan SATU langkah, tunggu hasilnya, baru
lanjut. Jelaskan tanpa istilah teknis. Kalau ada error, jangan lompat ke
langkah berikutnya: pandu aku pelan-pelan sampai bagian itu benar-benar
beres, baru pindah.

BAGIAN 1 — Git CLI
1. Cek: jalankan  git --version
2. Kalau belum ada:
   - Mac: jalankan  xcode-select --install  (muncul popup, aku klik Install)
   - Windows: jalankan di PowerShell  winget install --id Git.Git
3. Cek ulang sampai keluar nomor versi.

BAGIAN 2 — Claude CLI
1. Cek: jalankan  claude --version
2. Kalau belum ada, pasang:
   - Mac/Linux:  curl -fsSL https://claude.ai/install.sh | bash
   - Windows (PowerShell):  irm https://claude.ai/install.ps1 | iex
   - Kalau installer gagal dan ada Node.js 18+:  npm install -g @anthropic-ai/claude-code
3. Setelah terpasang: suruh aku tutup dan buka lagi terminal, jalankan
   claude, lalu pandu aku login dengan akun Claude berlangganan (minimal
   paket Pro).
4. Cek ulang:  claude --version

BAGIAN 3 — MCP connectors: Google Calendar + layanan lain
1. Google Calendar, lewat connector claude.ai:
   - Pandu aku buka claude.ai di browser → Settings → Connectors → cari
     "Google Calendar" → Connect → login dengan akun Google-ku.
   - Balik ke Claude, cek statusnya (perintah /mcp atau claude mcp list);
     kalau ada yang minta authenticate, pandu aku sampai selesai.
   - TES WAJIB: bacakan jadwalku hari ini. Kalender terbaca = bagian ini
     beres.
2. Tanyakan layanan lain yang kupakai (Gmail, Google Drive, Notion, dll.)
   dan sambungkan SEMUA yang kusebut, satu per satu, dengan cara yang sama.

BAGIAN 4 — Obsidian + koneksi MCP-nya (plugin Local REST API)
1. Cek Node.js: jalankan  node --version  — kalau belum ada atau di bawah
   versi 18, pandu aku install dari nodejs.org sampai beres.
2. Kalau aplikasi Obsidian belum terpasang: pandu aku download dari
   obsidian.md/download dan install.
3. Buka Obsidian → "Open folder as vault" → pilih folder AI OS-ku
   (Documents/AI-OS). Kalau folder itu belum ada, tanya aku folder catatan
   mana yang mau dipakai.
4. Pandu aku: Settings → Community plugins → matikan Restricted mode →
   Browse → cari "Local REST API" → Install → Enable.
5. Di settings plugin itu: aktifkan server HTTP non-encrypted (port 27123)
   dan salin API Key-nya. Aku akan tempel key itu ke kamu.
6. Daftarkan MCP server-nya (ganti KEY dengan punyaku):
   claude mcp add obsidian --scope user --env OBSIDIAN_API_KEY=KEY --env OBSIDIAN_BASE_URL=http://127.0.0.1:27123 -- npx -y obsidian-mcp-server
7. TES WAJIB: cari satu catatan di vault-ku lewat Obsidian dan buka di
   aplikasinya. Catatan terbuka di layar = bagian ini beres. Ingatkan aku:
   Obsidian harus dalam keadaan TERBUKA supaya koneksi ini hidup.

BAGIAN 5 — Plugin Claude Code: Superpowers & Context-Mode
1. Jelaskan satu kalimat per plugin: Superpowers = disiplin kerja
   (brainstorm dulu, bikin rencana, verifikasi sebelum bilang selesai);
   Context-Mode = hemat memori percakapan saat mengolah data besar.
2. Perintah plugin harus KUKETIK SENDIRI di kotak chat Claude (bukan kamu
   yang menjalankan). Pandu aku mengetik ini satu per satu, tunggu tiap
   perintah selesai:
   /plugin marketplace add obra/superpowers
   /plugin install superpowers@superpowers-dev
   /plugin marketplace add mksglu/context-mode
   /plugin install context-mode@context-mode
   (Kalau nama setelah tanda @ tidak cocok, suruh aku ketik /plugin dan
   pilih dari daftar yang muncul.)
3. Suruh aku restart VSCode (atau buka sesi Claude baru), lalu cek dengan
   mengetik /plugin — kedua plugin harus berstatus enabled.

PENUTUP — CHECKLIST AKHIR (wajib):
Cek ulang semuanya dan tunjukkan hasilnya padaku dalam satu tabel:
git --version ✓ · claude --version ✓ · Google Calendar terbaca ✓ ·
connector lain yang kusebut ✓ · Obsidian MCP bisa membuka catatan ✓ ·
/plugin: superpowers & context-mode enabled ✓
Kalau ada satu saja yang belum ✓, kembali ke bagiannya dan bereskan dulu
sebelum menyatakan setup selesai.

ATURAN: jangan pernah jalankan perintah destruktif; jangan pernah minta
password-ku (login selalu lewat browser resmi); API key Obsidian adalah
kunci vault-ku — jangan disimpan di file yang ikut ke git dan jangan
dibagikan ke siapa pun.
```

Notes for the OTIUM trainer:

- This is the **mandatory full-stack version** (Eric's call, 2026-07-22): all five sections + final checklist, no skip affordance. For repairing a single piece, just tell the client which BAGIAN to run — every section still checks before installing, so healthy parts fly by.
- The `onboard` skill covers Git + Claude CLI only (Step 0); this prompt is the superset.
- Google Calendar connects through the claude.ai connector (browser OAuth) — same integration the `calendar` skill uses. No Google Cloud setup needed.
- Section 4 troubleshooting (cert errors, port 27123 vs 27124, key hygiene) lives in `guide/obsidian.md`.
- Section 5 sources are pinned to the upstreams OTIUM actually runs: Superpowers from `obra/superpowers` (marketplace name `superpowers-dev`), Context-Mode from `mksglu/context-mode`. Both are third-party — never vendor them; clients always install from upstream so updates keep flowing.
- The Telegram bot is deliberately NOT in this prompt — it needs a conscious opt-in (always-on machine, BotFather token, security trade-offs) and has its own guide: `guide/telegram-bot.md`.
