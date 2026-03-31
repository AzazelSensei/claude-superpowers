---
name: skillify
description: Mevcut session'daki tekrarlanabilir süreci analiz edip yeniden kullanılabilir bir skill dosyasına dönüştür
userInvocable: true
argumentHint: "[sürecin kısa açıklaması]"
---

# Skillify

Mevcut session'da yapılan tekrarlanabilir süreci analiz edip yeniden kullanılabilir bir skill olarak kaydet.

## Görev

### Adım 1: Session'ı Analiz Et

Soru sormadan önce session'ı analiz et:
- Hangi tekrarlanabilir süreç uygulandı
- Girdi/parametreler nelerdi
- Adımlar (sırasıyla)
- Her adımın başarı kriterleri
- Kullanıcının seni düzelttiği yerler
- Hangi tool'lar ve izinler gerekti
- Hedefler ve başarı çıktıları

### Adım 2: Kullanıcıyla Görüş

AskUserQuestion tool'unu kullanarak kullanıcının ne otomatize etmek istediğini anla.

**Tur 1: Üst düzey onay**
- Analizine dayanarak skill için bir isim ve açıklama öner
- Üst düzey hedef(ler) ve başarı kriterleri öner

**Tur 2: Detaylar**
- Tespit ettiğin adımları numaralı liste olarak sun
- Skill argüman alacaksa, gözlemlerine dayanarak argüman öner
- Skill inline mi (mevcut konuşmada) yoksa fork mu (ayrı context'te) çalışmalı sor
- Skill nereye kaydedilsin sor:
  - **Bu repo** (`.claude/skills/<isim>/SKILL.md`) — projeye özel workflow'lar
  - **Kişisel** (`~/.claude/skills/<isim>/SKILL.md`) — tüm repo'larda geçerli

**Tur 3: Her adımı detaylandır**
Her ana adım için:
- Bu adım sonraki adımların ihtiyaç duyduğu ne üretiyor?
- Bu adımın başarılı olduğunu ne kanıtlıyor?
- Devam etmeden önce kullanıcıya sorulmalı mı? (geri dönüşü olmayan aksiyonlar için)
- Bağımsız adımlar paralel çalışabilir mi?

**Tur 4: Son sorular**
- Bu skill ne zaman çağrılmalı, tetikleyici ifadeler neler
- Dikkat edilecek tuzaklar var mı

Basit süreçler için fazla soru sorma!

### Adım 3: SKILL.md Yaz

Şu formatı kullan:

```markdown
---
name: {{skill-ismi}}
description: {{tek satırlık açıklama}}
userInvocable: true
argumentHint: "{{argüman ipucu}}"
---

# {{Skill Başlığı}}
Skill açıklaması

## Girdiler
- `$arg_ismi`: Bu girdinin açıklaması

## Hedef
Workflow'un net hedefi ve tamamlanma kriterleri.

## Adımlar

### 1. Adım İsmi
Bu adımda ne yapılacağı. Spesifik ve uygulanabilir olsun.

**Başarı kriteri**: Her adımda ZORUNLU.

...
```

**Adım yapısı kuralları:**
- Paralel çalışabilecek adımlar alt numara kullanır: 3a, 3b
- Kullanıcı aksiyonu gereken adımlar `[kullanıcı]` etiketi alır
- Basit skill'leri basit tut

### Adım 4: Onayla ve Kaydet

Dosyayı yazmadan önce tam SKILL.md içeriğini code block olarak göster.
AskUserQuestion ile onay al.
Yazdıktan sonra kullanıcıya bildir:
- Skill nereye kaydedildi
- Nasıl çağrılacağı: `/{{skill-ismi}} [argümanlar]`
- SKILL.md'yi direkt düzenleyerek ince ayar yapabileceği
