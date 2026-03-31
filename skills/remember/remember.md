---
name: remember
description: Auto-memory girdilerini incele, CLAUDE.md'ye taşınması gerekenleri öner, eski/çakışan/tekrar eden girdileri temizle
userInvocable: true
---

# Memory Gözden Geçirme

## Amaç
Kullanıcının memory katmanlarını incele ve önerilen değişiklikleri aksiyon tipine göre gruplandırılmış bir rapor olarak sun. Değişiklikleri UYGULAMA — önce kullanıcı onayı al.

## Adımlar

### 1. Tüm memory katmanlarını topla
Proje kökündeki CLAUDE.md ve CLAUDE.local.md dosyalarını oku (varsa). Auto-memory içeriğin zaten system prompt'unda — oradan incele. Ayrıca `.claude/projects/` altındaki memory dosyalarını da tara.

**Başarı kriteri**: Tüm memory katmanlarının içeriğine sahipsin ve karşılaştırabiliyorsun.

### 2. Her auto-memory girdisini sınıflandır
Her önemli girdi için en uygun hedefi belirle:

| Hedef | Ne ait | Örnekler |
|---|---|---|
| **CLAUDE.md** | Tüm katkıda bulunanların uyması gereken proje kuralları ve Claude talimatları | "mvn clean compile kullan", "API route'ları kebab-case", "Türkçe karakter zorunlu" |
| **CLAUDE.local.md** | Bu kullanıcıya özel, başkalarını ilgilendirmeyen kişisel talimatlar | "kısa cevaplar tercih ederim", "commit öncesi test çalıştır", "amend yapma" |
| **Auto-memory'de kalsın** | Çalışma notları, geçici bağlam, veya net bir yere sığmayan girdiler | Session'a özel gözlemler, belirsiz pattern'ler |
| **Silinmeli** | Artık geçerli olmayan, eskimiş veya yanlış bilgi içeren girdiler | Eski versiyon numaraları, değişen mimari kararlar |

**Önemli ayrımlar:**
- CLAUDE.md Claude'a talimat içerir, kullanıcı tercihleri (editör teması vb.) buraya ait değil
- Workflow pratikleri (PR convention, merge stratejisi) belirsiz — kullanıcıya sor
- Emin değilsen tahmin etme, sor

**Başarı kriteri**: Her girdinin önerilen bir hedefi var veya belirsiz olarak işaretli.

### 3. Temizlik fırsatlarını tespit et
Tüm katmanları tara:
- **Tekrar**: Auto-memory'de olup CLAUDE.md'de zaten olan → auto-memory'den silmeyi öner
- **Eskimiş**: CLAUDE.md'deki girdi daha yeni auto-memory ile çelişiyor → eski katmanı güncellemeyi öner
- **Çakışma**: İki katman arasında çelişki → hangisinin daha güncel olduğunu belirterek çözüm öner
- **Stale versiyon/tarih**: Sabit tarih veya versiyon numarası içeren girdiler → güncel mi kontrol et

**Başarı kriteri**: Tüm katmanlar-arası sorunlar tespit edildi.

### 4. Raporu sun
Aksiyon tipine göre gruplandırılmış yapılandırılmış rapor çıkar:

1. **Taşımalar** — taşınacak girdiler, hedef ve gerekçe ile
2. **Temizlik** — tekrarlar, eskimiş girdiler, çözülecek çakışmalar
3. **Belirsiz** — kullanıcının hedef belirlemesi gereken girdiler
4. **İşlem gerekmez** — yerinde kalması gereken girdiler hakkında kısa not

Auto-memory boşsa bunu belirt ve CLAUDE.md temizliği teklif et.

**Başarı kriteri**: Kullanıcı her öneriyi tek tek onaylayabilir/reddedebilir.

## Kurallar
- Değişiklik yapmadan ÖNCE tüm önerileri sun
- Kullanıcı onayı OLMADAN dosya değiştirme
- Hedef dosya yoksa yeni dosya oluşturma (kullanıcıya sor)
- Belirsiz girdiler hakkında tahmin etme — sor
- Türkçe karakter kullan (ş, ç, ğ, ü, ö, ı)
