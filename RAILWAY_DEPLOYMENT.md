# 🚂 Panduan Deploy ke Railway

## Masalah yang Diperbaiki

Error "TERJADI KESALAHAN KONEKSI KE SERVER" terjadi karena:

1. ❌ **Port hardcoded** - Railway menggunakan environment variable `PORT`
2. ❌ **Database SQLite tidak persisten** - File database hilang setiap restart
3. ❌ **Script start tidak ada** - Railway butuh command untuk production

## ✅ Solusi yang Diterapkan

### 1. Port Dinamis
Server sekarang menggunakan `process.env.PORT` yang disediakan Railway:
```typescript
const PORT = process.env.PORT || 3000;
app.listen(PORT, '0.0.0.0', () => {
  console.log(`Server running on port ${PORT}`);
});
```

### 2. Database Path Konfigurasi
Database path bisa dikonfigurasi via environment variable:
```typescript
const dbPath = process.env.DATABASE_PATH || 'showroom.db';
const db = new Database(dbPath);
```

### 3. Build & Start Scripts
Package.json sudah diupdate dengan script production menggunakan `tsx`:
```json
{
  "build": "vite build",
  "start": "NODE_ENV=production tsx server.ts"
}
```

**Catatan:** Menggunakan `tsx` untuk menjalankan TypeScript langsung di production, menghindari konflik ES Module vs CommonJS.

## 📋 Langkah Deploy ke Railway

### Step 1: Push ke GitHub
```bash
git add .
git commit -m "Fix Railway deployment configuration"
git push origin main
```

### Step 2: Setup di Railway Dashboard

1. **Login ke Railway** → https://railway.app
2. **New Project** → Deploy from GitHub repo
3. **Pilih repository** → GudangMokas

### Step 3: Konfigurasi Environment Variables

Di Railway Dashboard, tambahkan environment variables berikut:

```
NODE_ENV=production
PORT=3000
DATABASE_PATH=/app/data/showroom.db
```

**PENTING:** Untuk database persisten, tambahkan Railway Volume:
- Pergi ke **Settings** → **Volumes**
- Klik **Add Volume**
- Mount Path: `/app/data`
- Ini akan membuat database persisten dan tidak hilang saat restart

### Step 4: Deploy Settings

Railway akan otomatis detect konfigurasi dari:
- ✅ `railway.json` - Railway configuration
- ✅ `nixpacks.toml` - Build configuration
- ✅ `package.json` - Build & start scripts

### Step 5: Verifikasi Deployment

1. Tunggu build selesai (biasanya 2-5 menit)
2. Railway akan memberikan URL deployment (contoh: `https://your-app.up.railway.app`)
3. Buka URL tersebut
4. Test login admin dengan:
   - Username: `admin`
   - Password: `admin123`

## 🔧 Troubleshooting

### Jika masih error "Connection Failed":

1. **Check Logs di Railway:**
   - Buka project → Deployments → View Logs
   - Cari error message

2. **Verify Environment Variables:**
   - Pastikan `PORT` tidak di-set manual (Railway auto-inject)
   - Pastikan `NODE_ENV=production`

3. **Check Build Logs:**
   - Pastikan `npm run build` berhasil
   - Pastikan tidak ada TypeScript errors

4. **Database Volume:**
   - Pastikan Volume sudah di-mount ke `/app/data`
   - Restart deployment setelah add volume

### Jika database kosong setelah restart:

Ini berarti Volume belum di-setup. Ikuti langkah:
1. Railway Dashboard → Settings → Volumes
2. Add Volume dengan mount path `/app/data`
3. Set environment variable: `DATABASE_PATH=/app/data/showroom.db`
4. Redeploy

## 📱 Testing Login Admin

Setelah deploy berhasil:

1. Buka URL Railway app
2. Scroll ke bawah, klik tombol admin/settings
3. Login dengan:
   - **Username:** `admin`
   - **Password:** `admin123`
4. Jika berhasil, Anda akan masuk ke Admin Dashboard

## 🔐 Keamanan

**PENTING:** Setelah deploy, segera ganti password admin:
1. Login ke Admin Dashboard
2. Pergi ke tab **Settings**
3. Update username dan password
4. Save changes

## 📊 Monitoring

Railway menyediakan:
- **Metrics** - CPU, Memory, Network usage
- **Logs** - Real-time application logs
- **Deployments** - History dan rollback

## 💡 Tips

1. **Custom Domain:** Bisa add custom domain di Settings → Domains
2. **Auto Deploy:** Railway auto-deploy setiap push ke GitHub
3. **Environment per Branch:** Bisa setup staging/production environments
4. **Database Backup:** Export database secara berkala via admin panel

## 🆘 Butuh Bantuan?

Jika masih ada masalah:
1. Check Railway logs untuk error details
2. Verify semua environment variables sudah benar
3. Pastikan Volume sudah di-mount untuk database persistence
4. Test locally dengan `npm run build && npm start` untuk verify build works

---

**Status:** ✅ Ready for deployment
**Last Updated:** 2026-03-17
