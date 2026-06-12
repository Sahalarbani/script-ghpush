# ghpush

`ghpush` adalah CLI helper berbasis Node.js untuk workflow Git + GitHub dari terminal. Script ini dibuat untuk dipakai sebagai command langsung di Termux, jadi namanya sengaja `ghpush`, bukan `ghpush.sh`.

```bash
ghpush
```

## Fitur

- Pilih project dari folder `~/Projects`
- Auto-init Git repository jika belum ada `.git`
- Cek dan buat remote GitHub memakai GitHub CLI
- Stage semua file atau pilih file manual
- Buat commit manual atau generate pesan commit via Gemini AI
- AI commit memakai skill `atomic-commits` agar pesan commit tetap fokus, kecil, dan reviewable
- Push branch aktif ke GitHub
- Branch manager untuk switch, create, dan delete branch lokal
- TUI modern dengan dashboard status repository dan ASCII wordmark

## Requirement

Pastikan command ini tersedia:

```bash
node --version
npm --version
git --version
gh --version
```

Di Termux:

```bash
pkg update
pkg install nodejs git gh
```

Login GitHub CLI:

```bash
gh auth login
```

## Install Dependency

Script memakai package Node berikut:

```bash
npm install @clack/prompts chalk boxen cli-table3
```

Untuk setup single-file command di Termux, jalankan dari home directory supaya Node bisa menemukan dependency dari `~/node_modules`:

```bash
cd ~
npm install @clack/prompts chalk boxen cli-table3
```

## Install Command

Copy file `ghpush` ke `~/.local/bin/ghpush`:

```bash
mkdir -p ~/.local/bin
cp ghpush ~/.local/bin/ghpush
chmod +x ~/.local/bin/ghpush
```

Pastikan `~/.local/bin` masuk ke `$PATH`.

Untuk Bash:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Untuk Zsh:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

Cek command:

```bash
which ghpush
ghpush
```

## Struktur Project

Secara default, `ghpush` membaca daftar project dari:

```bash
~/Projects
```

Contoh:

```bash
~/Projects/my-app
~/Projects/my-script
~/Projects/my-website
```

Saat dijalankan, pilih project dari daftar tersebut, lalu `ghpush` akan masuk ke folder project dan menjalankan workflow Git.

## Gemini AI Commit

Fitur AI commit memakai Gemini API key. Saat pertama kali memilih mode AI, script akan meminta API key dan menyimpannya ke:

```bash
~/.ghpush.env
```

Format file:

```env
GEMINI_API_KEY=your_api_key
AI_MODEL=gemini-1.5-flash-latest
```

File ini dibuat dengan permission `600`.

AI commit sekarang membawa skill bawaan `atomic-commits`. Skill ini mengarahkan model untuk:

- Membuat satu logical change per commit
- Menghindari pesan WIP atau terlalu umum
- Menjaga commit tetap reviewable
- Memilih scope paling dominan saat diff terlalu campur aduk

## Usage

Jalankan:

```bash
ghpush
```

Alur utama:

1. Pilih workspace project
2. Cek atau buat remote GitHub
3. Pilih action dari dashboard workspace
4. Stage file
5. Pilih metode commit
6. Push ke branch aktif

## Catatan Nama File

File ini tidak memakai ekstensi `.sh` karena bukan shell script. Script memakai shebang:

```js
#!/usr/bin/env node
```

Selama file executable dan berada di folder yang masuk `$PATH`, command bisa dipanggil langsung:

```bash
ghpush
```

## Development Check

Cek syntax:

```bash
node --check ghpush
```
