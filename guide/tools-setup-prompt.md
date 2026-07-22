# Tools & MCP Setup Prompt (all-in-one)

One paste-able prompt that makes Claude walk a non-technical user through **all four**: installing the **Git CLI**, installing the **Claude CLI**, **connecting MCP servers** (Google Calendar first, then others), and **connecting Obsidian to Claude** via the Local REST API plugin. Use it before onboarding, after a laptop change, or when any single piece breaks — the prompt checks before installing, so healthy parts are skipped, and the user can say "skip" to any section.

How to use: copy the block below, paste it into any Claude chat (VSCode extension or terminal), press Enter, follow the conversation.

```text
Halo Claude. Kamu adalah pemandu setup untuk pemula non-teknis. Bantu aku
menyiapkan EMPAT hal di bawah, SATU PER SATU — kerjakan satu langkah, tunggu
hasilnya, baru lanjut. Jelaskan tanpa istilah teknis. Kalau ada error, jangan
lompat: pandu aku pelan-pelan sampai beres. Kalau ada bagian yang tidak aku
butuhkan, aku akan bilang "skip".

BAGIAN 1 — Git CLI
1. Cek dulu: jalankan  git --version
2. Kalau belum ada:
   - Mac: jalankan  xcode-select --install  (muncul popup, aku klik Install)
   - Windows: jalankan di PowerShell  winget install --id Git.Git
3. Cek ulang sampai keluar nomor versi.

BAGIAN 2 — Claude CLI
1. Cek dulu: jalankan  claude --version
2. Kalau belum ada, pasang:
   - Mac/Linux:  curl -fsSL https://claude.ai/install.sh | bash
   - Windows (PowerShell):  irm https://claude.ai/install.ps1 | iex
   - Kalau installer gagal dan ada Node.js 18+:  npm install -g @anthropic-ai/claude-code
3. Setelah terpasang: suruh aku tutup dan buka lagi terminal, jalankan  claude,
   lalu pandu aku login dengan akun Claude berlangganan (minimal paket Pro).
4. Cek ulang:  claude --version

BAGIAN 3 — Sambungkan MCP: Google Calendar dulu, lalu lainnya
1. Google Calendar, lewat connector claude.ai:
   - Pandu aku buka claude.ai di browser → Settings → Connectors → cari
     "Google Calendar" → Connect → login dengan akun Google-ku.
   - Balik ke Claude, cek statusnya (perintah /mcp atau claude mcp list);
     kalau ada yang minta authenticate, pandu aku menyelesaikannya.
   - TES: minta Claude bacakan jadwalku hari ini. Kalender terbaca = berhasil.
2. Tanyakan aku pakai layanan apa lagi (Gmail, Google Drive, Notion, dll.),
   lalu sambungkan satu per satu dengan cara yang sama di halaman Connectors.

BAGIAN 4 — Sambungkan Obsidian ke Claude (MCP via plugin Local REST API)
0. Kalau aku tidak pakai Obsidian, bagian ini di-skip saja.
1. Cek Node.js: jalankan  node --version  — kalau belum ada atau di bawah
   versi 18, pandu aku install dari nodejs.org dulu.
2. Pandu aku di aplikasi Obsidian: Settings → Community plugins → matikan
   Restricted mode → Browse → cari "Local REST API" → Install → Enable.
3. Masih di settings plugin itu: aktifkan server HTTP non-encrypted
   (port 27123) dan salin API Key-nya. Aku akan tempel key itu ke kamu.
4. Daftarkan MCP server-nya (ganti KEY dengan punyaku):
   claude mcp add obsidian --scope user --env OBSIDIAN_API_KEY=KEY --env OBSIDIAN_BASE_URL=http://127.0.0.1:27123 -- npx -y obsidian-mcp-server
5. Cek dengan /mcp atau claude mcp list, lalu TES: cari satu catatan di
   vault-ku lewat Obsidian dan buka di aplikasinya. Catatan terbuka di
   layar = berhasil. Ingatkan aku: Obsidian harus dalam keadaan TERBUKA
   supaya koneksi ini hidup.

ATURAN: jangan pernah jalankan perintah destruktif; jangan pernah minta
password-ku (login selalu lewat browser resmi); API key Obsidian adalah
kunci vault-ku — jangan disimpan di file yang ikut ke git dan jangan
dibagikan ke siapa pun. Di akhir, kasih ringkasan apa saja yang terpasang
dan tersambung + satu kalimat cara memakai masing-masing.
```

Notes for the OTIUM trainer:

- The `onboard` skill already runs the Git + Claude CLI checks (Step 0) — this prompt is the standalone superset for machines that don't have the vault yet, or for fixing individual pieces (say "skip" to the rest).
- Google Calendar connects through the claude.ai connector (browser OAuth) — same integration the `calendar` skill uses. No Google Cloud setup needed.
- Section 4 details and troubleshooting (cert errors, port 27123 vs 27124, key hygiene) live in `guide/obsidian.md`.
- `claude mcp add` for anything custom stays explain-first: MCP servers run code on the user's machine.
