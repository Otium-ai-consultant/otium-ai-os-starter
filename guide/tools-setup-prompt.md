# Tools & MCP Setup Prompt

One paste-able prompt that makes Claude walk a non-technical user through: installing the **Git CLI**, installing the **Claude CLI**, and **connecting MCP servers** (Google Calendar first, then anything else). Use it any time a machine is missing tooling — before onboarding, after a laptop change, or when a connector breaks.

How to use: copy the block below, paste it into any Claude chat (VSCode extension or terminal), press Enter, follow the conversation.

```text
Halo Claude. Kamu adalah pemandu setup untuk pemula non-teknis. Bantu aku
menyiapkan TIGA hal di bawah, SATU PER SATU — kerjakan satu langkah, tunggu
hasilnya, baru lanjut. Jelaskan tanpa istilah teknis. Kalau ada error, jangan
lompat: pandu aku pelan-pelan sampai beres.

BAGIAN 1 — Git CLI
1. Cek dulu: jalankan  git --version
2. Kalau belum ada:
   - Mac: jalankan  xcode-select --install  (nanti muncul popup, aku klik Install)
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

BAGIAN 3 — Sambungkan MCP server (Google Calendar dulu, lalu lainnya)
1. Google Calendar (paling penting), lewat connector claude.ai:
   - Pandu aku buka claude.ai di browser → Settings → Connectors → cari
     "Google Calendar" → Connect → login dengan akun Google-ku.
   - Balik ke Claude, cek statusnya (perintah /mcp atau claude mcp list);
     kalau ada yang minta authenticate, pandu aku menyelesaikannya.
   - TES: minta Claude bacakan jadwalku hari ini. Kalender terbaca = berhasil.
2. Tanyakan aku pakai layanan apa lagi (Gmail, Google Drive, Notion, dll.),
   lalu sambungkan satu per satu dengan cara yang sama di halaman Connectors.
3. Kalau aku punya MCP server custom/lokal: tambahkan pakai  claude mcp add,
   tapi jelaskan dulu servernya ngapain dan risikonya sebelum dipasang.

ATURAN: jangan pernah jalankan perintah destruktif; jangan pernah minta
password-ku (login selalu lewat browser resmi); dan di akhir, kasih ringkasan
apa saja yang terpasang & tersambung + satu kalimat cara memakainya.
```

Notes for the OTIUM trainer:

- The `onboard` skill already runs the Git + Claude CLI checks (Step 0) — this prompt is the standalone version for machines that don't have the vault yet, or for fixing a single broken piece.
- Google Calendar connects through the claude.ai connector (browser OAuth) — same integration the `calendar` skill uses. No Google Cloud setup needed.
- `claude mcp add` is intentionally last and gated behind an explain-first rule: custom MCP servers run code on the user's machine.
