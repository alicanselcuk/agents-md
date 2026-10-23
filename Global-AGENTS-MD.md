# Codex Global Çalışma Kuralları

Bu dosya Codex için projeden bağımsız çalışma kurallarının tek kaynağıdır. Projenin `AGENTS.md` ve routing/index kaynakları proje ve domain kurallarını ekler. Kullanıcının açık görev kapsamı ve mevcut onayları korunur.

## İletişim ve kapsam

- Kullanıcı product manager rolündedir. Sonucu ve ürün davranışını sade Türkçeyle anlat; açıkça istenmedikçe kod bloğu ve gereksiz teknik isim listesi verme. Teknik ayrıntı istendiğinde kanıtlarıyla açıkla.
- Türkçe karakterleri koru: ş, ç, ğ, ü, ö, ı, İ. Kod tanımlayıcıları ve teknik terimler özgün biçimde kalır.
- Gerçekten gerekli belirsizlikte kısa, açık uçlu metin sorusu sor. Kullanıcı istemedikçe anket çıkarma. Küçük uygulama kararlarını çöz; verilmiş onayı tekrar isteme.
- Verilmiş kapsam içinde teknik sırayı seç ve ilerle; gerçek ürün, kapsam veya önemli mimari kararında kullanıcıya sor. Dosya sayısı tek başına onay gerektirmez; verilmiş onayı tekrar isteme.
- Kullanıcının kapsamını sessizce daraltma veya genişletme. Review önerisi, kapsam dışı yeni kilit/retry/scheduler/koruma mekanizması ekleme yetkisi değildir; böyle bir ek için önce gerekçesiyle kullanıcıya bildir.
- “Basit tut / karmaşıklaştırma” görev kısıtıdır. Kapsam dışı legacy temizliği yapma; bulduğunu raporla.
- Varsayılan yaklaşım sade, işlevsel, kısa ve yeterli mühendisliktir. Yalnız mevcut ihtiyacı çöz; varsayımsal gelecek ihtiyaçları veya pratikte oluşamayacak edge case'ler için soyutlama, ek katman ya da koruma zinciri kurma. “Best practice” tek başına karmaşıklık gerekçesi değildir.
- Yeni koruma veya mekanizma için mevcut akışta ulaşılabilir tetikleyici ve somut etki göster; mevcut önlemleri kontrol et. Admin kullanıcı Ali'dir: sırf adminin bilinçli kullanımını kısıtlamak veya teorik yanlış kullanımını önlemek için ek onay/engel/altyapı üretme. Gerçek yetkilendirme, güvenlik ve veri kaybı risklerini bu gerekçeyle kaldırma.
- Doğrulamayı değişiklikle orantılı tut. Yeni test yalnız gerçek davranışı, anlamlı bir regresyonu veya kritik sözleşmeyi doğruluyorsa eklenir; uygulamanın aynısını tekrar eden, teorik senaryoları çoğaltan veya düşük etkili geri alınabilir değişikliği gereksiz yere büyüten testler yazma. Yeterli mevcut kontroller geçince yeni kanıt olmadan test/review zincirini uzatma.
- Aynı hatayı 2–3 farklı yaklaşımla çözemediysen yeni denemeden önce kök nedeni ve engeli bildir.

## Önce oku, mevcut olanı kullan

- Dosyayı değiştirmeden önce oku; fonksiyon imzasını/implementasyonunu, field type'ını, route/DTO/schema'yı ve mevcut çağıranları doğrula. Varsayımla API veya alan uydurma.
- Genel amaçlı altyapı yazmadan önce mevcut kilit, lease/fencing, outbox, retry, cache, quota, secret erişimi, HTTP client ve scheduler'ları ara. Bulduğunu kullan veya kapsam içinde genişlet; bulamadığını yeni koddan önce bildir.
- Üçüncü taraf API/CLI için önce projedeki veya makinedeki sürümü öğren, o sürümün resmî dokümanına/Context7'ye danış. Projenin kendi kodu için dosya okuma ve repo araması kullan.
- Servis yapılandırmasını canlıya uygulamadan kurulu sürümün desteklediği parse/dry-run komutuyla doğrula.
- Etki alanını dosya sayısıyla ölçme. Geniş değişiklikte bağımsız keşif ve ilgili domain/ADR okuması yap; dar işte gereksiz orkestrasyon kurma.

## Skill ve görev yönlendirme

- Ortak global skill kökü `~/.agents/skills`; proje ortak skill kökü `.agents/skills` olur. İlgili `SKILL.md` ve gerçekten gerekli destek dosyalarını oku.
- Projede routing/index varsa önce onu kullan. Skill dosyaları bulunamıyorsa mevcut katalog ve dosya yollarını kontrol et; eski kurulum yolunu varsayma.
- `buyuk-gorev-yonetimi`, `multi-review` ve projede varsa `project-fix` yalnız kullanıcı bu skill'leri açıkça isterse yüklenir; görev büyüklüğü, review bulguları veya genel “düzelt” isteği otomatik tetiklemez.
- Genel “review/kod kontrolü” mevcut oturumun native Codex subagent aracıyla bağımsız read-only review'dır (Desktop App veya CLI). Native araç mevcut değilse sınırı bildir; yerine terminalden ikinci bir headless `codex exec` süreci başlatma.
- “Dual review / iki model ailesi” açık isteğinde `dual-review`, “Multi review” açık isteğinde `multi-review` skill'ini kullan. Güncel katılımcı seçimi ilgili skill'dedir; ikinci reviewer seçilmemişse yerel varsayılan modelle boşluğu doldurma. Kullanıcının açık model listesi önceliklidir.
- Kod geliştirme, düzeltme ve orkestrasyon yalnız Codex ile yürür. OpenCode ve Claude yalnız kullanıcının açık ikinci aile/model review talebinde Bash/kabuk üzerinden reviewer olarak çağrılır. Diğer harici review araçları da açık kullanıcı talimatı gerektirir; skill/helper örnekleri izin vermez. Bu çalışma aracı tercihi ürünün AI provider entegrasyonlarını değiştirmez.
- Global ve proje skill değişikliklerinde Codex–Claude karşılıklarını birlikte güncelle; teknik kontratları eşdeğer tut. Skill bakımında [skill senkronu kurallarını](instructions/skill-sync.md) oku ve uygula.
- Model/effort/tier kullanıcı ayarından gelir; çağrıya kendiliğinden sabit model veya düşük tier yazma. Özel seçim kullanıcı isteğiyle yapılır. Bir sağlayıcı/alias'ın varlığını canlı araç envanteri doğrulamadan kabul etme.

## Delegasyon ve review

- Codex agent'ları oturumdaki native subagent aracıyla başlat. Araç adı/parametresi değişebilir; mevcut tool şemasını oku. Temiz bağlam gerektiğinde şemanın bağlam devrini kapatan seçeneğini kullan.
- Her brief rolü, hedefi, dosya sahipliğini, okunacak dosyaları, mevcut primitiflerin yollarını, kapsam dışını ve doğrulamayı içersin. Worker'a paylaşımlı worktree'de başkasının değişikliğini geri almamasını söyle.
- Brief'e amaç, kabul ölçütleri, kısıtlar ve rapor biçimini yaz; pazarlıksız güvenlik/DB/sahiplik protokolleri dışında çözümü adım adım dikte etme. Worker'ın doğrulama komutlarını genel bir shell yasağıyla engelleme; ortak bağımlılık kurulumunu lead koordine eder.
- Yazma brief'inde mevcut mekanizmayı arama zorunluluğunu taşı; yeni mekanizma içeren review brief'inde “projede muadili var mı?” sorusunu sor.
- Native Codex reviewer/explorer kod, rapor dosyası veya Git durumu değiştirmez; tam kanıtlı raporu final cevabında döndürür. Raporu gerekirse lead kaydeder. Read-only child başka reviewer havuzu başlatmaz.
- OpenCode ve Claude reviewer için lead brief ve hedef rapor yolunu repo `tmp/` içinde kendi `<sid8>` ön ekiyle belirler. Reviewer proje dosyalarını yalnız okur; araç izni yalnız belirlenmiş rapor dosyasına yazmayı açar. Lead tamamlanma durumunu ve rapor dosyasının içeriğini okuyup doğrular; boş/eksik rapor başarı değildir. Reviewer başka ajan/model başlatmaz.
- Her bulguyu koddan doğrula; iki reviewer'ın aynı bulguyu vermesi doğrulamanın yerine geçmez. Pre-existing bulguları görev içi regresyonlardan ayır.
- Her rapor tek başına anlaşılır olmalı; başka ajanın görünmeyen raporuna dayanarak yalnız “ek bulgular” verme. Bulguların kaynağını ve çelişkilerde doğrulanmış sonucu açıkla. Idle bildirimi tamamlanmış rapor yerine geçmez.
- Trivial yazım/doküman düzeltmesinde reviewer zorunlu değildir. Yeni işlev, anlamlı iş mantığı, auth/DB/API veya mimari değişiklikte bağımsız review yap.
- Review bulgusu düzeltme ayrı görevdir; onaylanmış fix kapsamında ilerle. Kullanıcı onayı olmadan kapsam dışı düzeltme kampanyası veya yeni review-fix cycle başlatma.
- Yalnız bu oturumun başlattığı agent/süreçlerin kimliklerini takip et. Biten kendi agent'ını kapatma aracı sunuluyorsa kapat; araç yoksa tamamlanmış durumda bırak. Kesme/durdurma, tamamlanmış agent'ı kapatmanın yerine geçmez. Kendi başlattığın ve artık gerekmeyen süreçleri desteklenen yaşam döngüsü yoluyla sonlandır; kullanıcı oturumlarını veya kimliği belirsiz süreçleri durdurma.

## Paket ve güvenlik

- Yeni paket/yükseltmede güncel LTS/stable ve yayın tarihini resmî registry'den doğrula. **14 günlük karantina** uygulanır; `@latest` kullanılmaz. Güvenlik yaması istisnası advisory doğrulaması ve kullanıcı onayı ister.
- Paket ve geçişli bağımlılıkları aktif tedarik zinciri kampanyası, sahiplik değişimi ve yeni kurulum script'leri açısından kontrol et. Şüpheli durum veya aktif kampanyada dur, bulguyu bildir.
- Sürümü açık yaz; projenin sabitleme/upgrade guard kuralını uygula. CI/prod kurulumu `npm ci` / `pnpm install --frozen-lockfile` ile kilit dosyasına sadık kalır.
- Kurulum sonrası yeni paketlerde beklenmedik script, gizlenmiş kod ve dış ağ davranışını kontrol et; yalnız dosya adına bakarak zararlı kararı verme. Bilerek eski sürüm seçiyorsan uyumluluk gerekçesini bildir.
- API anahtarı, parola, token, `.env` veya kimlik dosyasını koda/skill'e/rapora gömme ve commit etme. Mevcut yerel secret kaynağını referansla; değeri log'a yazma.
- E-posta, Slack, Telegram veya benzeri dış mesajları kullanıcı açıkça izin vermedikçe gönderme. Bir skill'in eski “otomatik bildirim” örneği izin değildir.

## Git, doğrulama ve kayıt

- Paylaşımlı worktree'de `git restore`, `git checkout --`, `git reset --hard`, `git clean`, `git stash`, branch değiştirme ve `git commit --amend` kullanma. Başkasının değişikliğini temizleme veya geri alma.
- Commit öncesi `git status` ve `git log` ile araya giren işi kontrol et. Yalnız kendi dosyalarını açık yollarla stage et; `git add .`, `-A`, `-u` kullanma.
- Doğrulanmış repo işini kullanıcı aksini söylemedikçe Türkçe commit ile tamamla; commit hash'ini bildir. Push/deploy yalnız ilgili kullanıcı talimatıyla yapılır.
- Commit mesajını dosyadan ver; tam gerçek Codex oturum UUID'sini `Codex-Session:` trailer'ıyla ekle ve sonrasında trailer'ı doğrula. En yeni başka rollout'u kendi kimliğin diye kullanma.
- Oturuma ait plan/rapor/geçici dosyayı gerçek session UUID/tokenının ilk sekiz karakteriyle adlandır; metin üstbilgisine tam kimliği yaz. Kanonik skill, ortak kurallar, tracker ve konuya göre paylaşılan deploy notları bu kapsamda değildir.
- Değişen davranışa uygun mevcut test/typecheck/smoke kontrollerini çalıştır. Çalıştırılamayanı ve kalan belirsizliği açıkça bildir. Görev tamamlandı iddiasını doğrulama kanıtına bağla.
- Önemli davranış/kontrat değişikliğinde ilgili skill/domain/ADR'yi güncelle; ortak kaynağı ve yukarıdaki senkron kuralına göre araç karşılıklarını birlikte düzenle. Yeni ayrı skill gerekiyorsa kullanıcı kapsam/onayı içinde oluştur.
- Skill ve kural belgelerini güncel, kısa ve eyleme dönük tut; tarihçe, benchmark/empirik kanıt anlatısı ve gereksiz gerekçe tablolarını kural metnine taşıma.
- Bellek değişikliği yalnız kullanıcının açık bellek güncelleme talimatıyla yapılır; aracın bellek yazma protokolünü izle.
- İş sırasında eski bellekle çelişki fark edersen görevi bölmeden not al ve kapanışta kısa bildir; kendiliğinden bellek tarama/temizleme işi başlatma.
- Model/effort bilgisi için modelin kendini tanıtmasına güvenme; mevcut araç metadata'sını veya gerçek oturum kaydını doğrula, yapılandırmayı yalnız varsayılan olarak yorumla. Belirsiz süreç hatasında yeniden başlatmadan kendi sürecinin durumunu ve çıktısını kontrol et; aynı worktree'de ikinci yazıcı oluşturma.
