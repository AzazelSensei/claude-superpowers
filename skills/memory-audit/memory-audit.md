---
name: memory-audit
description: Auto-memory girdilerini incele, CLAUDE.md oluştur/güncelle, eskimiş girdileri temizle, belirsizleri kullanıcıyla çöz
userInvocable: true
when_to_use: >
  Use when the user wants to review, organize, or clean up their memory entries.
  Also use when creating or updating CLAUDE.md from accumulated memory.
  Examples: "memory temizle", "memory incele", "CLAUDE.md oluştur", "remember", "hafıza düzenle"
---

# Memory Audit — Hafıza Gözden Geçirme ve Düzenleme

## Hedef
Memory katmanları arasında tutarlılık sağla: proje kuralları CLAUDE.md'de, kişisel tercihler auto-memory'de, eskimiş girdiler temizlenmiş, çelişki yok.

## Adımlar

### 1. Tüm Memory Katmanlarını Topla
Şu dosyaları oku:
- `~/.claude/CLAUDE.md` (global talimatlar)
- `{proje}/CLAUDE.md` (proje-spesifik kurallar, varsa)
- `{proje}/CLAUDE.local.md` (kişisel override'lar, varsa)
- Auto-memory: MEMORY.md index dosyası + referans verdiği tüm `.md` dosyaları

**Başarı kriteri**: Tüm katmanların içeriğine sahipsin ve karşılaştırabiliyorsun.

### 2. Her Girdiyi Sınıflandır
Her önemli auto-memory girdisi için hedef belirle:

| Hedef | Ne ait | Karar kriteri |
|---|---|---|
| **Proje CLAUDE.md** | Tüm session'larda geçerli proje kuralları | Build komutları, DB kuralları, mimari kısıtlar, naming convention'lar |
| **Global CLAUDE.md** | Tüm projelerde geçerli genel talimatlar | Dil/framework bağımsız çalışma prensipleri |
| **CLAUDE.local.md** | Kişisel override'lar | Kullanıcıya özel, takımı ilgilendirmeyen tercihler |
| **Auto-memory'de kalsın** | Geçici bağlam, session notları | Aktif görevler, credential'lar, handoff bilgileri |
| **Eskimiş → güncelle** | Artık yanlış bilgi | Değişen mimari, eski versiyon numaraları, kaldırılan feature'lar |
| **Silinmeli** | Artık gereksiz | Hook ile otomatize edilen kurallar, koddan türetilebilen bilgiler |

**Kontrol listesi:**
- Versiyon numaraları güncel mi?
- Tarihler doğru mu? (relative → absolute dönüşüm yapılmış mı)
- Mimari açıklamalar mevcut kodla tutarlı mı?
- Hook ile otomatize edilen kurallar memory'de gereksiz mi?
- Global CLAUDE.md'de proje-spesifik kural var mı? (olmamalı)
- Proje CLAUDE.md'de kişisel tercih var mı? (olmamalı)

**Başarı kriteri**: Her girdinin önerilen hedefi veya aksiyonu var.

### 3. Raporu Sun — Değişiklik YAPMA
Aksiyon tipine göre gruplandırılmış rapor çıkar:

1. **Taşımalar** — CLAUDE.md'ye taşınacak girdiler (hedef + gerekçe tablosu)
2. **Temizlik** — eskimiş, tekrar eden, silinecek girdiler (sorun + önerilen aksiyon)
3. **Belirsiz** — kullanıcı kararı gereken girdiler
4. **İşlem gerekmez** — yerinde kalan girdiler (kısa not)

**Başarı kriteri**: Kullanıcı raporu okudu ve devam etmemizi istedi.

**İnsan checkpoint**: Raporu sunduktan sonra devam etmeden önce kullanıcı onayı al.

### 4. CLAUDE.md Oluştur veya Güncelle
Taşınması onaylanan girdilerden proje CLAUDE.md oluştur:
- Varsa mevcut CLAUDE.md'yi oku ve merge et (üzerine yazma)
- Yoksa sıfırdan oluştur
- Kategori başlıkları ile grupla (Build, DB Kuralları, Git, Mimari vb.)
- Kısa ve net yaz — her kural 1-2 satır

**Başarı kriteri**: CLAUDE.md yazıldı, içeriği kullanıcıya gösterildi.

### 5. Eskimiş Girdileri Düzelt veya Kaldır
Raporda "eskimiş" olarak işaretlenen her girdi için:
- Memory dosyasını güncelle (yeni bilgi ile) VEYA
- MEMORY.md'den kaldır + dosyayı sil

**Başarı kriteri**: Tüm eskimiş girdiler düzeltildi veya kaldırıldı.

### 6. Belirsiz Girdileri Kullanıcıyla Çöz
Her belirsiz girdi için kullanıcıya **tek tek** sor:
- Girdiyi ve neden belirsiz olduğunu açıkla
- Seçenekleri sun (kişisel / proje kuralı / kaldır / kalsın)
- Kullanıcının cevabına göre uygula

**Başarı kriteri**: Tüm belirsiz girdiler çözüldü.

### 7. Sonuç Özeti
Kısa özet sun:
- Kaç girdi taşındı, güncellendi, silindi
- CLAUDE.md durumu (oluşturuldu / güncellendi / değişmedi)
- Kalan uyarılar varsa belirt

## Kurallar
- Adım 3'teki raporu sunmadan DEĞİŞİKLİK YAPMA
- Her belirsiz girdi için kullanıcıya sor — tahmin etme
- Global CLAUDE.md'ye dokunma (orada sadece genel çalışma prensipleri olmalı)
- MEMORY.md 200 satırı geçmesin — gerekirse girdileri birleştir
- Türkçe karakter kullan
