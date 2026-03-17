# 🚀 Panduan Deploy Website Gudang Mokas

## Opsi 1: Vercel (PALING MUDAH - Recommended untuk Demo)

### ⚠️ Catatan Penting:
Vercel adalah platform serverless, jadi ada limitasi:
- ❌ SQLite database tidak bisa dipakai (serverless = no persistent storage)
- ❌ File uploads ke folder `/public/uploads` tidak persisten
- ✅ Website frontend bisa jalan sempurna
- ✅ Gratis dan super cepat

### Solusi untuk Vercel:
1. **Ganti SQLite dengan Database Cloud** (pilih salah satu):
   - **Turso** (SQLite di cloud) - GRATIS, paling mirip dengan setup sekarang
   - **Supabase** (PostgreSQL) - GRATIS, powerful
   - **PlanetScale** (MySQL) - GRATIS tier tersedia

2. **Ganti File Upload dengan Cloud Storage**:
   - **Cloudinary** - GRATIS, khusus untuk gambar
   - **Uploadcare** - GRATIS tier
   - **Supabase Storage** - GRATIS

### Langkah Deploy ke Vercel (5 Menit):

```bash
# 1. Install Vercel CLI
npm install -g vercel

# 2. Login ke Vercel
vercel login

# 3. Deploy (dari folder project)
vercel

# 4. Ikuti prompt:
# - Set up and deploy? Yes
# - Which scope? (pilih account kamu)
# - Link to existing project? No
# - Project name? gudang-mokas
# - Directory? ./
# - Override settings? No

# 5. Deploy production
vercel --prod
```

**Selesai!** Website langsung online di: `https://gudang-mokas.vercel.app`

---

## Opsi 2: Railway (RECOMMENDED - Support Database & File Upload)

### ✅ Keuntungan Railway:
- ✅ Support SQLite database
- ✅ Support file uploads (persistent storage)
- ✅ Gratis $5/bulan credit
- ✅ Setup mudah, tidak perlu ubah code banyak

### Langkah Deploy ke Railway (10 Menit):

```bash
# 1. Install Railway CLI
npm install -g @railway/cli

# 2. Login
railway login

# 3. Initialize project
railway init

# 4. Link project
railway link

# 5. Deploy
railway up

# 6. Add domain (optional)
railway domain
```

**Atau pakai Web UI (Lebih Mudah):**
1. Buka https://railway.app
2. Sign up dengan GitHub
3. Klik "New Project" → "Deploy from GitHub repo"
4. Connect repository kamu
5. Railway auto-detect dan deploy!

---

## Opsi 3: Render (Support Full-Stack)

### Langkah Deploy ke Render:

1. Buka https://render.com
2. Sign up gratis
3. Klik "New +" → "Web Service"
4. Connect GitHub repo
5. Settings:
   - **Build Command**: `npm install && npm run build`
   - **Start Command**: `npm start`
6. Klik "Create Web Service"

**Selesai!** Website online dalam 5-10 menit.

---

## Opsi 4: Cloudflare Pages (GRATIS & CEPAT)

### ⚠️ Catatan:
Cloudflare Pages untuk static sites, tapi bisa pakai **Cloudflare Workers** untuk backend.

### Langkah Deploy:

```bash
# 1. Install Wrangler CLI
npm install -g wrangler

# 2. Login
wrangler login

# 3. Deploy
wrangler pages deploy dist
```

**Atau pakai Web UI:**
1. Buka https://dash.cloudflare.com
2. Pilih "Pages"
3. Connect GitHub repo
4. Auto deploy!

---

## 🎯 Rekomendasi Saya:

### Untuk Demo Cepat (1-2 hari):
**Railway** - Karena:
- ✅ Tidak perlu ubah code sama sekali
- ✅ Support SQLite & file uploads
- ✅ Setup 10 menit
- ✅ Gratis $5/bulan (cukup untuk traffic kecil-menengah)

### Untuk Production Jangka Panjang:
**Vercel + Supabase** - Karena:
- ✅ Vercel: Hosting frontend super cepat & gratis
- ✅ Supabase: Database + Storage gratis
- ✅ Scalable untuk traffic besar
- ⚠️ Perlu setup database cloud (1-2 jam)

---

## 📝 Checklist Sebelum Deploy:

- [ ] Update `.env` dengan production values
- [ ] Test semua fitur di local
- [ ] Backup database (`showroom.db`)
- [ ] Setup domain custom (optional)
- [ ] Setup SSL certificate (auto di semua platform)
- [ ] Test admin login di production
- [ ] Test upload gambar
- [ ] Test form jual mobil & trade-in

---

## 🔧 File yang Perlu Diubah untuk Production:

### 1. Update `package.json` - Tambah start script:
```json
"scripts": {
  "dev": "tsx server.ts",
  "build": "vite build",
  "start": "NODE_ENV=production tsx server.ts",
  "preview": "vite preview"
}
```

### 2. Update `server.ts` - Production mode:
```typescript
const PORT = process.env.PORT || 3000;
```

### 3. Buat `vercel.json` (jika pakai Vercel):
```json
{
  "version": 2,
  "builds": [
    {
      "src": "server.ts",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "server.ts"
    }
  ]
}
```

---

## 💡 Tips:

1. **Railway** = Paling mudah, langsung jalan tanpa ubah code
2. **Vercel** = Paling cepat, tapi perlu setup database cloud
3. **Render** = Balance antara mudah & fitur lengkap
4. **Cloudflare** = Gratis unlimited, tapi perlu setup Workers

Mau saya buatkan setup untuk Railway atau Vercel?
