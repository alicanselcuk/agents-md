# AGENTS.md çalışma kuralları

Bu repo, Codex ile yürütülen yazılım çalışmalarında kullanılabilecek Türkçe global ve proje düzeyindeki ajan talimatlarını içerir. Amaç; kullanıcının belirlediği kapsamı korumak, mevcut yapıyı anlayarak çalışmak ve sonucu doğrulanabilir biçimde teslim etmektir.

## Dosyalar

| Dosya | Kapsam |
| --- | --- |
| [Global-AGENTS-MD.txt](Global-AGENTS-MD.txt) | Projelerden bağımsız iletişim, kapsam, delegasyon, review, paket güvenliği, Git ve doğrulama kuralları. |
| [Proje-AGENTS-MD.txt](Proje-AGENTS-MD.txt) | Projeye özel başlangıç kaynakları, domain ve skill yönlendirmesi, mimari sınırlar, review ve görev kapanışı için uyarlanabilir şablon. |

## Temel yaklaşım

- **Sade iletişim ve açık kapsam:** Sonucu Türkçe ve ürün davranışı üzerinden anlat. Verilen yetki içinde ilerle; önemli ürün, kapsam ve mimari kararlarını kullanıcıya bırak.
- **Önce oku, mevcut olanı kullan:** Değişiklikten önce ilgili kodu ve sözleşmeleri doğrula. Mevcut mekanizmaları araştır; varsayımla API veya alan üretme.
- **Yeterli mühendislik:** Mevcut ihtiyacı çöz. Gereksiz soyutlama, kapsam dışı temizlik ve varsayımsal senaryolar için ek altyapı kurma.
- **Sınırları belirli delegasyon ve review:** Geliştirme Codex ile yürür. Bağımsız inceleme ile düzeltme yetkisi ayrıdır; diğer model aileleri açık kullanıcı talebiyle review yapar.
- **Güvenlik ve kontrollü bağımlılıklar:** Secret değerlerini kod ve raporlara yazma. Paketlerde sabit sürüm, resmî kaynak kontrolü ve 14 günlük karantina uygula.
- **Paylaşımlı çalışma alanına saygı:** Başkasının değişikliklerini geri alma. Yalnız görev dosyalarını commit et; push ve deploy için ilgili kullanıcı talimatını esas al.
- **Kanıta dayalı kapanış:** Değişiklikle orantılı kontrolleri çalıştır; doğrulanamayan noktaları ve kalan bulguları açıkça bildir.

## Projeye uyarlama

Global dosya ortak çalışma ilkelerini, proje dosyası ise bunlara eklenen proje sözleşmelerini tanımlar. Kullanıcının açık görev kapsamı ve mevcut onayları korunur.

Proje şablonundaki proje adı, teknoloji yığını, yerel yollar, skill adları, test komutları ve operasyon sınırları kendi projenize göre düzenlenmelidir. Belgelerde adı geçen destek dosyaları ve skill'ler bu repoya dahil değildir; ilgili projede ayrıca bulunmalıdır.
