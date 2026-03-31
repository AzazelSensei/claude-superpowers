---
name: verify
description: Kod değişikliğinin gerçekten çalıştığını doğrula — uygulamayı çalıştır, test et, kanıtla
userInvocable: true
argumentHint: "[doğrulanacak değişikliğin açıklaması]"
---

# Verify — Değişiklik Doğrulama

Yapılan kod değişikliğinin gerçekten çalıştığını doğrula. "Çalışıyor" demek yetmez — kanıt göster.

## Felsefe

- **İddia etme, kanıtla.** "Düzelttim" demek yetmez, çalıştığını göster.
- **Kullanıcı gibi test et.** Compile geçmesi yetmez, uygulama gerçekten çalışmalı.
- **Yan etkileri kontrol et.** Bir şeyi düzeltirken başka bir şeyi bozma.

## Adımlar

### 1. Değişikliği Anla
- Hangi dosyalar değişti? (`git diff --stat`)
- Değişikliğin amacı ne?
- Hangi bileşenler etkileniyor?

**Başarı kriteri**: Değişikliğin kapsamını net olarak ifade edebiliyorsun.

### 2. Build/Compile Kontrolü
- Projenin build sistemini çalıştır
- Tüm hatalar temizlenmeli

**Başarı kriteri**: Clean build, sıfır hata.

### 3. Doğrudan Test
Değişikliğin türüne göre uygun testi seç:

**Backend değişikliği:**
- Uygulamayı başlat (veya başlatılmışsa)
- İlgili endpoint'e curl/HTTP isteği at
- Yanıtı doğrula

**Frontend değişikliği:**
- Dev server'ı başlat
- İlgili sayfayı aç veya screenshot al
- UI'ın beklendiği gibi render edildiğini doğrula

**Veritabanı/Migration:**
- Migration'ı çalıştır
- Tablo/kolon varlığını doğrula
- Rollback test et

**Başarı kriteri**: Değişiklik beklenen davranışı üretiyor — çıktı veya yanıtla kanıtla.

### 4. Regresyon Kontrolü
- Değişiklikten etkilenebilecek mevcut fonksiyonları test et
- Mevcut testler varsa çalıştır
- İlişkili endpoint'leri/sayfaları kontrol et

**Başarı kriteri**: Mevcut fonksiyonalite bozulmamış.

### 5. Rapor
Doğrulama sonucunu kısa ve net raporla:

```
## Doğrulama Raporu
- Değişiklik: [ne değişti]
- Build: ✓/✗
- Doğrudan test: ✓/✗ [kanıt]
- Regresyon: ✓/✗ [kontrol edilen alanlar]
- Sonuç: GEÇTI / KALDI
```

## Kurallar
- Doğrulama yapmadan "tamamlandı" deme
- Her adımın çıktısını göster — sessiz başarı kabul edilmez
- Hata bulursan düzeltmeye çalışmadan ÖNCE raporla
- Kullanıcının argümanı varsa o değişikliğe odaklan
