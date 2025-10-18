
# 🧭 Git Advanced Commands — Summary Notes

## 1️⃣ git reset
**Amaç:** Çalışma alanını veya commit geçmişini geri almak.  
**Kullanım:**
```bash
git reset --soft HEAD~1   # Sadece commit geri al (değişiklikler korunur)
git reset --mixed HEAD~1  # Commit + stage geri al (değişiklikler kalır)
git reset --hard HEAD~1   # Tamamen geri al (değişiklikler silinir)
```
🧩 *Yanlış commit attım ama kodlar kalsın* dersen `--soft`,  
*Tamamen sıfırlamak istiyorum* dersen `--hard` kullanılır.

---

## 2️⃣ git revert
**Amaç:** Eski bir commit’i geri almak ama yeni bir commit oluşturarak.  
**Kullanım:**
```bash
git revert <commit_hash>
```
💡 `git reset` geçmişi değiştirirken, `git revert` geçmişi korur.  
Ekip çalışmasında daha güvenlidir.

---

## 3️⃣ git rebase
**Amaç:** Commit geçmişini yeniden düzenlemek veya başka bir branch’in sonuna taşımak.  
**Kullanım:**
```bash
git rebase main
```
🧱 `feature` branch’ini `main`'in sonuna taşır — sanki en güncel halden yapılmış gibi olur.

🔹 **Interactive rebase** (düzenleme modu):
```bash
git rebase -i HEAD~3
```
→ Son 3 commit’i düzenleyebilirsin (`pick`, `reword`, `squash`, `drop` vs.).

---

## 4️⃣ git cherry-pick
**Amaç:** Başka bir branch’teki tek bir commit’i almak.  
**Kullanım:**
```bash
git cherry-pick <commit_hash>
```
🎯 Tüm branch’i merge etmeden sadece bir düzeltmeyi taşıyabilirsin.

---

## 5️⃣ git reword
**Amaç:** Commit mesajını değiştirmek.  
**Kullanım:**
```bash
git rebase -i HEAD~3
```
Sonra ilgili commit’in başındaki `pick` kelimesini `reword` olarak değiştir.  
Git mesaj düzenleme ekranı açar.  

📝 Örnek:
```bash
pick 12ab34 fix login bug
reword 56cd78 update button color
```

---

## 6️⃣ git squash
**Amaç:** Birden fazla commit’i tek commit’te birleştirmek.  
**Kullanım:**
```bash
git rebase -i HEAD~3
```
`pick` → `squash` olarak değiştir:
```bash
pick 12ab34 add login ui
squash 56cd78 fix typo
squash 78ef90 update color
```
🔹 Hepsi tek commit olarak kalır (tarih temizlenir, geçmiş sadeleşir).

---

## 💡 Özet Tablo

| Komut | İşlev | Geçmişi Değiştirir mi? | Ortak Kullanım |
|-------|--------|-------------------------|----------------|
| `git reset` | Commit veya değişiklikleri geri alır | ✅ Evet | Yanlış commit’leri düzeltmek |
| `git revert` | Commit’i geri alır (yeni commit ile) | ❌ Hayır | Ortak repoda hatayı geri almak |
| `git rebase` | Commit geçmişini düzenler | ✅ Evet | Temiz geçmiş oluşturmak |
| `git cherry-pick` | Tek commit’i başka branch’e taşır | ❌ Hayır | Belirli bir düzeltmeyi almak |
| `git reword` | Commit mesajını değiştirir | ✅ Evet | Mesaj hatalarını düzeltmek |
| `git squash` | Commit’leri birleştirir | ✅ Evet | Gereksiz commitleri toplamak |

---

📘 **Öneri:**  
Bu notları `advanced-git-notes.md` dosyası olarak GitHub deposuna ekleyebilirsin.
