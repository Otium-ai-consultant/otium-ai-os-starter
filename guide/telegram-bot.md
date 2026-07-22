---
title: Bot Telegram untuk AI OS-mu
type: guide
tags: [telegram, bot, power-up]
---

# 🤖 Bot Telegram untuk AI OS-mu — jalan di komputermu sendiri

Chat dengan AI OS-mu **dari HP, lewat Telegram** — dari mana saja. Pesanmu masuk ke
Claude Code yang berjalan di folder AI OS di komputermu, dan balasannya dikirim kembali
ke Telegram. Tanpa VPS, tanpa sewa server: komputermu sendiri yang jadi "server"-nya.

> **Jujur di awal — kompromi jalan lokal:** bot ini hidup **hanya saat komputermu menyala
> dan tidak tidur**. Laptop ditutup = bot offline. Ini normal dan bisa diakali (lihat
> Langkah 7). Kalau kamu butuh bot yang hidup 24/7, itulah gunanya VPS — tapi untuk
> pemakaian pribadi, komputer yang nyala di jam kerja biasanya sudah cukup.

**Prasyarat:** Claude Code ter-install dan sudah login (lihat `SETUP.md`), Node.js 18+
(unduh dari nodejs.org), dan folder AI OS-mu sudah jadi (sudah lewat `onboard`).

---

## Langkah 1 — Buat bot di Telegram (2 menit)

1. Di Telegram, cari **@BotFather** (yang centang biru) → kirim `/newbot`.
2. Kasih nama (mis. "Asisten AI-ku") dan username (harus diakhiri `bot`, mis. `asistenku_bot`).
3. BotFather membalas dengan **token** — deretan seperti `123456789:AAF...xyz`.
   **Salin dan rahasiakan** — siapa pun yang pegang token ini bisa mengendalikan botmu.

## Langkah 2 — Ambil chat ID-mu (1 menit)

Bot ini harus **hanya melayani kamu** — ia punya akses ke Claude di komputermu.

1. Di Telegram, cari **@userinfobot** → kirim `/start`.
2. Ia membalas dengan **Id** milikmu (angka, mis. `123456789`). Salin.

## Langkah 3 — Siapkan folder bot (3 menit)

Buka terminal (Mac: Terminal, Windows: PowerShell), lalu:

```bash
mkdir ai-os-telegram-bot
cd ai-os-telegram-bot
npm init -y
npm install node-telegram-bot-api dotenv
```

## Langkah 4 — File konfigurasi `.env`

Buat file bernama `.env` di folder itu, isi (ganti dengan nilaimu sendiri):

```bash
TELEGRAM_BOT_TOKEN=123456789:AAF...xyz
ALLOWED_CHAT_ID=123456789
# Path lengkap ke folder AI OS-mu:
# Mac:     /Users/namamu/Documents/ai-os
# Windows: C:\Users\namamu\Documents\ai-os
VAULT_PATH=/Users/namamu/Documents/ai-os
```

> `.env` berisi rahasia — jangan pernah di-commit ke git. Kalau foldernya jadi repo git,
> tambahkan `.env` ke `.gitignore`.

## Langkah 5 — Script bot: `bot.js`

Buat file `bot.js`, isi:

```javascript
require("dotenv").config();
const TelegramBot = require("node-telegram-bot-api");
const { execFile } = require("child_process");

const bot = new TelegramBot(process.env.TELEGRAM_BOT_TOKEN, { polling: true });
const ALLOWED = process.env.ALLOWED_CHAT_ID;
const VAULT = process.env.VAULT_PATH;

// Windows butuh shell untuk menemukan `claude`; Mac/Linux tidak.
const IS_WIN = process.platform === "win32";

// Tools yang boleh dipakai tanpa konfirmasi. Read-only = aman.
// Mau bot bisa MENULIS catatan juga? Tambahkan "Write,Edit" — pahami risikonya dulu.
const ALLOWED_TOOLS = "Read,Grep,Glob";

let busy = false;

bot.on("message", (msg) => {
  const chatId = String(msg.chat.id);
  if (chatId !== ALLOWED) return; // abaikan semua orang selain kamu
  const text = msg.text;
  if (!text) return;

  if (busy) {
    bot.sendMessage(chatId, "⏳ Masih mengerjakan pesan sebelumnya — tunggu sebentar.");
    return;
  }
  busy = true;
  bot.sendChatAction(chatId, "typing");

  // "/new" memulai percakapan baru; selain itu lanjutkan percakapan terakhir.
  const args = ["-p", text, "--output-format", "text", "--allowedTools", ALLOWED_TOOLS];
  if (text !== "/new" && text !== "/start") args.push("--continue");

  execFile(
    "claude",
    args,
    { cwd: VAULT, shell: IS_WIN, timeout: 300000, maxBuffer: 10 * 1024 * 1024 },
    (err, stdout, stderr) => {
      busy = false;
      if (err) {
        bot.sendMessage(chatId, "⚠️ Error: " + (stderr || err.message).slice(0, 500));
        return;
      }
      const reply = stdout.trim() || "(jawaban kosong)";
      // Telegram membatasi 4096 karakter per pesan — pecah kalau panjang.
      for (let i = 0; i < reply.length; i += 4000) {
        bot.sendMessage(chatId, reply.slice(i, i + 4000));
      }
    }
  );
});

console.log("🤖 Bot hidup. Kirim pesan ke bot-mu di Telegram.");
```

## Langkah 6 — Jalankan dan tes

```bash
node bot.js
```

Buka botmu di Telegram, kirim: *"apa isi tasks/todo.md?"* — kalau ia menjawab isi
todo-mu, semuanya jalan. Kirim `/new` kapan pun kamu mau mulai topik baru.

## Langkah 7 — Biarkan hidup terus (auto-start + anti-tidur)

**Supaya bot bangkit sendiri** setelah crash atau restart, pakai **pm2**:

```bash
npm install -g pm2
pm2 start bot.js --name ai-os-bot
pm2 save
pm2 startup   # ikuti instruksi yang muncul (Mac/Linux; di Windows: npm i -g pm2-windows-startup)
```

Perintah berguna: `pm2 logs ai-os-bot` (lihat log) · `pm2 restart ai-os-bot` · `pm2 stop ai-os-bot`.

**Supaya komputer tidak tidur** saat kamu ingin bot tetap hidup:
- **Mac:** System Settings → Battery/Energy → aktifkan *"Prevent automatic sleeping when
  the display is off"* (saat tersambung listrik), atau jalankan `caffeinate -s` di terminal.
- **Windows:** Settings → System → Power → *Screen and sleep* → set **Sleep: Never**
  (minimal saat *plugged in*).

---

## Keamanan — baca ini

- **Allowlist itu wajib.** Baris `if (chatId !== ALLOWED) return;` adalah pagarmu — tanpa
  itu, siapa pun yang menemukan botmu bisa memakai Claude di komputermu.
- **Mulai read-only.** `ALLOWED_TOOLS` default hanya bisa membaca. Menambahkan
  `Write,Edit` berarti pesan Telegram bisa mengubah file-mu; `Bash` berarti bisa
  menjalankan perintah di komputermu. Longgarkan hanya kalau kamu paham konsekuensinya.
- **Token = kunci.** Bocor? Kembali ke @BotFather → `/revoke` → dapat token baru.

## Troubleshooting

| Masalah | Solusi |
|---|---|
| `claude: command not found` / bot diam saja | Pastikan Claude Code ter-install via CLI dan bisa dipanggil dari terminal (`claude --version`). Di Windows, `shell: IS_WIN` di script sudah membantu; kalau masih gagal, isi path lengkap ke `claude`. |
| Balasan lambat | Normal — pertanyaan besar butuh waktu. Timeout script 5 menit; naikkan `timeout` kalau perlu. |
| `409 Conflict` di log | Ada dua instance bot jalan bersamaan (mis. `node bot.js` + pm2). Matikan salah satu. |
| Bot menjawab tapi tidak tahu konteks OS-mu | Cek `VAULT_PATH` di `.env` menunjuk ke folder AI OS yang benar (yang berisi `CLAUDE.md`). |
| Bot mati saat laptop ditutup | Memang begitu — lihat Langkah 7, atau pertimbangkan VPS kalau butuh 24/7. |

## Connected
- [[README]]
- [[SETUP]]
