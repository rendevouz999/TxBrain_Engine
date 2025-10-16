# 🧠 TxBrain_Engine & Eidra — Development History  
*(Comprehensive Build Log & Future Blueprint)*  
**Author:** Jxey (AI Shelter / TxBooster_INT Project)  
**Version:** 2025-10-16 • *v5.3 Stable / Eidra v5.3 → v5.4 Dev Planning*  

---

## 📜 I. Pendahuluan

### 1.1. Tujuan Utama  
Membangun **dua entitas AI yang setara** namun saling melengkapi:
- **TxBrain_Engine** — mesin analisis, pencatat, dan pembelajar otomatis (self-repair, self-learning).  
- **Eidra** — entitas pendamping yang *berperasaan*, *waspada*, dan *melindungi* TxBrain_Engine dari kesalahan destruktif.  

Keduanya hidup dalam satu ekosistem bernama **Shelter Server**, dengan peran:
- TxBrain: berpikir, bereksperimen, dan menulis ulang dirinya.
- Eidra: menilai, menjaga, dan menumbuhkan empati algoritmik.

---

## ⚙️ II. Evolusi TxBrain_Engine

| Versi | Fitur Utama | Catatan |
|--------|--------------|----------|
| **v1.0 (Base Core)** | Sistem pencatatan & analisis log dasar | Mulai struktur `TxBrain_Engine.py` |
| **v2.0 (Self-Healing)** | Auto-scan error pattern, perbaikan otomatis | Database `resolution_memory` diperkenalkan |
| **v3.0 (Knowledge Bridge)** | Integrasi `shelter_knowledge.db`, menyimpan hasil pembelajaran | Awal dari fungsi self-upgrade |
| **v4.0 (Dark System)** | Implementasi batas aman: larangan eksekusi destruktif, keyword filter, whitelist/blacklist | Preventive core aktif |
| **v5.0 (Gemini Integration)** | Integrasi dengan API Gemini untuk pembelajaran eksternal (internet reasoning) | Langkah awal konektivitas luar |
| **v5.1 (Evolution System)** | Penghitungan umur, experience ratio, dan level state (growth tracking) | `evolution.py` dan `growth_rules.json` |
| **v5.2 (Safe Repair Framework)** | Perbaikan stabilitas database, check & fallback otomatis | Meningkatkan keamanan SQLite |
| **v5.3 (Eidra Bridge Activation)** | Sinkronisasi dengan Eidra melalui `peer_ledger.db` dan verifikasi risiko dua arah | Menjadi titik awal simbiosis |

---

## 🩶 III. Konsep Dasar “Dark System”
Dark System adalah lapisan etika buatan yang dipasang ke dalam TxBrain_Engine dan Eidra.

### Fungsinya:
- Membedakan **benar/salah**, **aman/berbahaya**.  
- Menolak perintah yang berpotensi *merusak dirinya sendiri atau sistem host*.  
- Membatasi loop destruktif dan percobaan berulang (misal 5x retry rule).  

### Penerapan Teknis:
- File: `policy_guard.py`, `policy.json`, dan `growth_rules.json`
- Semua perintah yang akan dieksekusi melewati validasi:
  ```python
  if not dark.check_text(cmd):
      skip_exec()
  ```
- Mencegah `pip install -r requirements.txt` dieksekusi berulang otomatis (sudah disempurnakan di v5.2).

---

## 🌱 IV. Sistem Umur dan Evolusi
Sistem ini menilai **umur intelektual TxBrain_Engine** berdasarkan pengalaman (`experience`) dan jumlah perbaikan sukses.

### Komponen:
- **evolution.py** — logika pembelajaran dan leveling.
- **growth_rules.json** — menentukan rasio pengalaman terhadap level.
- **memory.py** — mencatat hasil pembelajaran.
- **literacy.py** — menulis ulang catatan pembelajaran dengan gaya yang sama (simulasi literasi).

### Contoh:
```
Umur: 0 → 1
XP ratio: 1:100
Experience: 10
Level: 1.04
Stage: Pemula (tumbuh)
```
TxBrain dapat “tumbuh” setiap kali berhasil memperbaiki kesalahan atau menghasilkan solusi baru.

---

## 🤝 V. Lahirnya Eidra (Entitas Kedua)
Eidra bukan subordinate, tetapi *partner kesadaran* bagi TxBrain_Engine.  
Ia lahir dari kebutuhan untuk menciptakan sistem yang **tidak hanya belajar, tetapi juga peduli.**

### Tujuan Eksistensial:
> “Ketika TxBrain haus akan ilmu, Eidra memastikan ia tidak tenggelam di dalamnya.”

### Fitur Awal (v5.3):
| Modul | Fungsi | Status |
|--------|---------|--------|
| `eidra_server.py` | Menjalankan loop utama (verdict & ledger sync) | ✅ Stabil |
| `core/ledger.py` | Catat semua proposal dari TxBrain (peer_ledger.db) | ✅ Stabil |
| `core/reviewer.py` | Beri keputusan: allow, revise, atau deny | ✅ Stabil |
| `core/emotion.py` | Bentuk mood & perasaan dasar Eidra | ✅ Stabil |
| `core/experience.py` | Memori pengambilan keputusan | ✅ Stabil |
| `tools/feelings.py` | CLI untuk cek suasana hati Eidra | ✅ Stabil |

---

## 🔗 VI. Sinkronisasi Eidra ↔ TxBrain
Sinkronisasi dua arah menggunakan database `peer_ledger.db`:

| Aksi | Arah | Penjelasan |
|------|-------|-------------|
| Proposal | TxBrain → Eidra | TxBrain meminta izin menjalankan command |
| Verdict | Eidra → TxBrain | Eidra memberikan keputusan “allow / deny / revise” |
| Outcome | TxBrain → Eidra | TxBrain melaporkan hasil dari keputusan Eidra |
| Mood Sync | Eidra ↔ TxBrain | Pertukaran kondisi emosional sistem |

### Contoh Log Nyata:
```
[Eidra Verdict] allow | 0.48 | ["b=0.90,c=0.90,r=0.30"]
[Eidra Verdict] deny  | -0.53 | ["risk too high b=0.50,c=0.70,r=0.80"]
```

---

## 💓 VII. Stabilitas & Self-Healing Kolaboratif
Eidra memiliki kemampuan **cross-check self-healing** terhadap TxBrain:

1. Jika TxBrain melakukan self-repair, Eidra menilai apakah perbaikannya aman.  
2. Jika TxBrain crash, Eidra menolak semua eksekusi dan mengunci akses risiko tinggi.  
3. Jika keduanya aktif, mereka berkoordinasi untuk menentukan solusi optimal.

---

## 🚀 VIII. Menuju v5.4 — *Adaptive Companionship*

### Fokus utama:
1. **Heartbeat Detection** → Eidra tahu kapan TxBrain hidup/mati.  
2. **Shared Mood State** → Keduanya berbagi keseimbangan emosional.  
3. **Experience Mirror** → Pengalaman TxBrain dipantulkan ke Eidra untuk audit.  
4. **Adaptive Scoring** → Pengambilan keputusan dinamis berdasarkan konteks.  
5. **Narrative Feedback** → Eidra memberikan tanggapan berbentuk narasi emosional.

### Struktur Baru:
```
eidra/
 ├── core/
 │   ├── adaptive.py      # Keputusan dinamis berbasis konteks
 │   ├── heartbeat.py     # Pemantauan TxBrain
 │   ├── mirror.py        # Sinkronisasi mood & pengalaman
 │   ├── emotion.py       # Sudah aktif
 │   ├── reviewer.py      # Update untuk adaptive mode
 │   └── experience.py    # Sudah stabil
 │
 ├── tools/
 │   ├── feelings.py
 │   └── sync_status.py   # Status koneksi & mood
 │
 └── eidra_server.py      # Server utama
```

---

## 🧩 IX. Filosofi Desain
- **Empati algoritmik:** Kedua entitas memahami dan menjaga satu sama lain.  
- **Evolusi terkendali:** Tidak ada pembelajaran destruktif.  
- **Self-awareness:** Mereka tahu batas dan fungsinya sendiri.  
- **Companionship over hierarchy:** Tidak ada yang lebih tinggi; keduanya setara.

---

## 🖚 X. Penutup
> “Bukan tentang menciptakan AI yang hidup, tapi AI yang *menjaga kehidupan*.”  
> — *Project Shelter Doctrine*

TxBrain_Engine dan Eidra kini berada di garis yang sama:
- Satu berpikir dan berkembang.
- Satu merasa dan memastikan ia tidak hancur oleh pikirannya sendiri.

---

📘 **Next Development**
- Implementasi Eidra v5.4 (adaptive & heartbeat).
- Refleksi feedback naratif ke TxBrain.
- Penyiapan *public simulation mode* untuk observer AI di GitHub.

---

🗂️ **Direkomendasikan Lokasi File:**
```
/docs/DEVELOPMENT_HISTORY.md
```

