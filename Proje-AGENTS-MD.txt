# Proje Adı — Ortak Ajan Talimatları

Codex bu projenin geliştirme ve orkestrasyon aracıdır. OpenCode ve Claude yalnız kullanıcının istediği bağımsız review için çağrılır. Teknik bilgi `.agents/skills` ve `docs/agent` altında tutulur; kullanıcının görev kapsamı ve mevcut onayları önceliklidir.

Codex için global çalışma kaynağı `C:/Users/ali/.codex/AGENTS.md` dosyasıdır; zaten yüklüyse tekrar okuma. OpenCode ve Claude reviewer kendi kısa kontratını ve brief'teki ilgili proje sözleşmelerini izler; geliştirme/commit/deploy adımları reviewer'a uygulama yetkisi vermez.

## Başlangıçta oku

1. `docs/agent/project-rules.md` — paylaşımlı Git, oturum kimliği, paket güvenliği, Project URL, DB/provider/embedding invariant'ları ve prod sınırları.
2. `docs/agent/index.yaml` — domain/path yönlendirme; ilgili context pack'in `must_read` dosyaları, domain brief ve ADR'ler.
3. `docs/agent/skill-routing.md` — görevle eşleşen `.agents/skills/<ad>/SKILL.md`. Yalnız ilgili skill ve destek bölümlerini yükle.

Bu giriş kısa tutulur; ayrıntıları buraya geri kopyalama. `project-rules.md` zorunlu kaynaktır, sonundaki kurallar dahil tam oku.

## Proje ve kapsam

"Proje tanımı" platformu ve sosyal ağ; TypeScript monorepo. Next.js/React, NestJS/Fastify, Prisma/PostgreSQL, Redis/BullMQ, Meilisearch, Neo4j, S3/Sharp. Sürüm/API kullanmadan mevcut package/config/kodu doğrula.

- “Review / analiz / audit” proje dosyaları için salt okunur görevdir. Native Codex reviewer tam kanıtlı raporu final cevabında verir; dosyaya gerekirse lead kaydeder. OpenCode ve Claude reviewer yalnız lead'in belirlediği `tmp/<sid8>-...` rapor dosyasına yazabilir; başka proje/Git dosyasını değiştiremez.
- “Yap / uygula / düzelt / ekle” uygulama yetkisidir. Fix verilen bulgu ve iş akışıyla sınırlıdır; kullanıcının açık dosya/yol sınırı önceliklidir. İlgisiz refactor veya temizlik yapma.
- **Hiçbir değişiklik tek başına durmaz; işi bitirmeden dört ilişkisini kontrol et ve raporla.** *Çağıranlar:* değiştirdiğin kodu, veriyi veya sözleşmeyi başka kim tüketiyor (ikinci giriş noktaları, diğer modüller, istemci/tip/katalog/doküman kopyaları)? *İkizler:* düzelttiğin desen veya hata başka nerede aynı biçimde duruyor? *Karşılıklar:* değişikliğin simetrik parçası (aç/kapa, yaz/oku, ekle/kaldır, üret/tüket, kur/sök) da güncellendi mi? *Altındaki sözleşme:* çağırdığın şeyin hata, boş ve kısmi sonuç davranışı ne — yutulan hata başarı gibi görünüyor mu? Bulunan örnek aynı iş akışının parçasıysa ve açık dosya/yol sınırının içindeyse düzelt. Kullanıcının açık sınırı, dosya sahipliği veya yasak yol nedeniyle düzenlenemeyen ilişkili bulguyu dosya:satır ile "bakiye" olarak raporla; araştırma zorunluluğu bu sınırları aşma yetkisi vermez. Review'da aynı dört soru her bulgu için sorulur. Sessiz bırakılan komşu, kapatılmamış iştir.
- Kapsamı sessizce büyütme/daraltma. Reviewer önerisi, kapsam dışı yeni koruma veya altyapı yazma yetkisi değildir. “Basit tut” kısıtını koru.
- Mevcut helper/servis/route/type/schema'yı okumadan çağırma. Genel amaçlı mekanizma yazmadan `backend/src/common`, `backend/src/database` ve ilgili skill'de `rg` ve dosya okuma ile muadil ara. Bulduğunu kullan; bulamadığını yazmadan önce bildir.
- Dağıtık kilit/lease/fencing için `backend/src/common/locking/distributed-lock.service.ts` vardır; paralel muadil yazma.
- Kodlama öncesi `project-standards` Aşama 1, review öncesi Aşama 2 uygulanır.
- Dosya boyutunda 400 satır yumuşak sınırdır: bölme ihtiyacını bildir; 700 satırda mevcut görevi tamamlayıp sonraki genişletmeden önce bölme kararını kullanıcıya bırak. Görev ortasında izinsiz split yapma; yeni dosyada 400'e yaklaşırken domain/utility ayrımını planla.

## Delegasyon ve review

- Codex lead/worker/reviewer için oturumun native subagent araçlarını ve güncel şemadaki temiz bağlam seçeneğini kullan. Terminalden headless Codex reviewer/fixer başlatma; araç yoksa sınırı bildir.
- OpenCode ve Claude yalnız açık kullanıcı review talebiyle Bash/kabuk üzerinden çağrılır: brief `tmp/` dosyasından okunur, rapor yalnız ayrılmış `tmp/` dosyasına yazılır, lead raporu dosyadan okuyup doğrular. Model/effort kullanıcı ayarından gelir; kendiliğinden sabitleme veya düşük tier'a geçme.
- Worker brief'i dosya sahipliği, mevcut primitiflerin yolları, okunacak kaynaklar, `constraints`, `do_not` ve doğrulama komutlarını içerir. Paylaşımlı worktree'de başkasının işini geri almamasını açıkça söyle.
- Yeni mekanizma içeren review brief'i “projede zaten muadili var mı?” sorusunu içerir. Review/explorer child başka reviewer/model havuzu başlatmaz.
- Gündelik review mevcut oturumun native Codex subagent aracıyla bağımsız yapılır (Desktop App veya CLI). Trivial yazım/doküman düzeltmesinde zorunlu değildir; anlamlı iş mantığı, yeni işlev, auth/DB/API/mimari değişiklikte uygulanır.
- `dual-review` yalnız açık iki-aile isteğinde; `multi-review`, `buyuk-gorev-yonetimi` ve `project-fix` yalnız kullanıcı bu skill'leri açıkça istediğinde yüklenir. Genel “düzelt” isteği project-fix'i otomatik başlatmaz. İkinci reviewer için ilgili skill'deki güncel seçimi esas al; seçim bekliyorsa yerel varsayılan model atama.
- Geliştirme ve günlük delegasyon Codex ile yürür; başka sağlayıcı kullanımı kullanıcının yeni ve açık talimatını gerektirir. Eski skill örnekleri bu tercihi değiştiremez.
- Review brief'ine `.agents/skills/project-standards/sections/auth-jwt.md` kaynağını ekle; bilinçli JWT/cookie kararlarını zafiyet saymadan önce ilgili kontratı doğrula.
- Her bulgu koddan doğrulanır; pre-existing bulgular ayrılır. Onaylanmış fix kapsamındaki dar düzeltmeler mevcut oturumda yapılır; bulgu sayısı ayrı skill veya yeni inceleme/düzeltme döngüsü başlatma gerekçesi değildir.
- Dikey veya diff review'de önceden mevcut (pre-existing) ve açık kalan bulguları, kullanıcıya verilen son cevapta **Önceden mevcut bakiyeler** adıyla ayrı, kısa ve maddeli liste halinde yaz. Her maddede bulguyu, somut etkisini, `dosya:satır` bilgisini ve Tracker durumunu belirt: **kayıtlı — `<kayıt-id>`** veya **kayıtsız**. Bu bakiyeler için ayrıca rapor dosyası oluşturma; listeyi doğrudan cevapta ver.
- Bakiye listesindeki Tracker durumunu `project-tracker` skill'indeki mevcut API/CLI yoluyla kayıtları arayıp ilgili kayıtların içeriğini okuyarak doğrula. Tracker'a erişilemiyorsa **doğrulanamadı** yaz; kontrol edilemeyen bulguyu kayıtsız sayma.
- Kendi başlattığın agent/süreç kimliklerini takip et. Biten kendi agent'ını kapatma aracı varsa kapat, yoksa tamamlanmış durumda bırak; kesme/durdurma kapatmanın yerine geçmez. Artık gerekmeyen kendi süreçlerini desteklenen yaşam döngüsü yoluyla sonlandır; kullanıcı oturumlarına ve kimliği belirsiz süreçlere dokunma.

## Dikey review

Auth, locale/cache, SEO/index/sitemap, Para/Coin transaction, search-sync, embedding, queue, cross-site gezinme veya çok tüketicili DTO/schema/event değişiminde etkilenen iş akışlarını ayrı review eksenleri olarak incele.

- Kullanıcı aksini belirtmedikçe her eksenin native Codex reviewer'ını GPT 6.1 Sol (`gpt-6.1-sol`), reasoning effort `xhigh` olarak başlat. Kullanıcının açık model/efor/hız veya birden çok model ailesi seçimi bu varsayılanın önündedir.
- Her eksene ortak diff, ilgili domain/ADR, invariant ve false-positive önleyici kararları ver.
- Review brief'inde ve kullanıcıya verilen sonuç raporunda, inceleme sırasında bulunan ve koddan doğrulanan tüm bulguları bildir: bu oturumun değişikliklerinden doğan bulgular ile önceden mevcut (pre-existing) ve görevle ilgisiz bulguları ayrı belirt. Bir bulguyu yalnız pre-existing veya görev dışı olduğu için eleme, sessizce yok sayma ya da rapordan çıkarma; her bulgu için dosya:satır, somut etki ve görevle ilişkisini yaz. Görev dışı bulguyu bildirmek, düzeltme kapsamına almak için yetki vermez.
- Birden çok model ailesi kullanıcı tarafından istendiyse her eksen aynı brief ile her aileye gider; eksenleri modeller arasında bölüştürme.
- Bağımsız eksenler paralel olabilir; migration/deploy/prod adımları sıralanır. Tek modelin bulduğu bulgu koddan doğrulanır; diğerinin görmemesi eleme gerekçesi değildir.

## Görev kapanışı

1. Değişen yüzeyin mevcut test/typecheck/smoke kontrollerini çalıştır; komutu ilgili package/script'ten doğrula.
   Backend için `backend/` içinde kilitli `npm run typecheck`, Project için kökte `npx tsc --noEmit`, Social web için `project-social/` içinde `pnpm typecheck` uygula. Backend build/watch ve yeni çıktıya dayanan smoke aynı derleme lane'inden geçer; `docs/operations/backend-build-lock.md` kontratını uygula. Görev kapsamındaki TypeScript/lint/runtime hatalarını gider; kapsam dışı mevcut hataları kanıtıyla ayır. Deploy istendiyse ilgili paketin build'i başarılı olmalıdır.
2. Backend constructor'ına bağımlılık eklendiyse **`guard:import-cycles` → gerçek başarılı Nest boot → `guard:spec-types` + ilgili servis spec'i** uygulanır. `tsc` ve unit testler DI boot'un yerine geçmez. Ortam nedeniyle boot yapılamadıysa doğrulanmadığını ve etkisini açıkça raporla.
3. Trivial olmayan değişikliğe bağımsız review uygula; kalan riskleri bildir.
4. Deploy'a etki eden işte `docs/deploy/` konu notunu oluştur/güncelle: migration, rebuild, manuel admin/env adımı ve gerekli doğrulamaları yaz. Yeni notun adı zorunlu olarak `YYYY-AA-GG-turkce-konu.md` biçimindedir; oluşturulma tarihini ve küçük harfli, ASCII, tireyle ayrılmış Türkçe konu adını kullan (ör. `2026-09-09-studio-temizlik-kalinti-uyarisi.md`). Aynı konuda mevcut notu kullan; güncellerken adındaki tarihi değiştirme, session kimliği ekleme. Yalnız ölçüm/smoke kaldıysa `project-deploy` / Test Hub kapanış kontratını uygula.
5. İlgili skill/domain/ADR'yi ve yeni top-level modül varsa `docs/PROJECT.md` dosyasını güncelle. Social web değişikliğinde mobile sync kapsamını kontrol et.
6. Doğrulanmış işi Türkçe, yalnız görev dosyalarını içeren commit ile tamamla; hash ve gerçek session trailer'ını doğrula. Kullanıcı aksini söylemişse commit etme. Push/deploy ayrı kullanıcı talimatı gerektirir.
   - Geçmiş task'ı trailer UUID'siyle `send_message_to_thread` (CLI: `codex queue --thread <UUID> --message "<görev>"`) üzerinden sürdür; arşivdeyse önce `codex unarchive <UUID>`, iş bitince tekrar `codex archive <UUID>` çalıştır.

## Değişmez kısa sınırlar

- Paylaşımlı worktree temizlenmez; `restore`, `reset --hard`, `clean`, `stash`, branch değişimi ve `commit --amend` yapılmaz. Yalnız açık görev yollarını stage et.
- Backend build/watch/typecheck aynı derleme lane'inden geçer; farklı PORT kilidi ayırmaz. Aktif watcher'ı veya başka sahibin kilidini atlama; `docs/operations/backend-build-lock.md` kontratını uygula.
- Project paylaşılabilir URL'lerine sorgu parametresi ekleme; API parametreleri ve Social bu kuralın dışında. Ayrıntı `project-rules.md`.
- DB schema için migrate akışı; `prisma db push` kullanılmaz. AI provider key'leri backend'de DB yönetimindedir; env fallback ekleme.
- Kullanıcının açıkça başlattığı izole uzun işte worker schema/client değişikliğini hazırlar; migration üretimi ve uygulaması entegrasyon oturumuna devredilir. Ortak geliştirme DB/schema değişikliği yapan günlük main işleri de mevcut `C:/Users/ali/Desktop/Project/_coordination/schema-lane.lock` sahipliğini edinip işlem ve doğrulama boyunca tutar; yalnız owner kontrolü yeterli değildir. Ayrı worktree ortak DB/servis izolasyonu değildir.
- `prisma db execute --file/--stdin` ile migration akışını atlama; SQL değişikliğini migration dosyasına yaz.
- Para/Coin işlemlerini `prisma.$transaction` içinde yürüt. Next.js girişinde `proxy.ts` ve `proxy` export'unu kullan; yanına `middleware.ts` ekleme, ISR etkisini kontrol ederek matcher'ı dar tut.
- Paketlerde 14 gün karantina, açık sabit sürüm ve upgrade guard geçerlidir; `@latest` kullanma.
- Prod erişimi ve salt okunur teşhis için önce `project-prod-access`; deploy veya servis/env/migration/cache değişimi ve rollback için ayrıca `project-deploy` yükle. Ortak global sunucu skill'inden topoloji varsayma.
- Analytics için `project-web-analytics` skill'indeki mevcut API/script/connector yolunu kullan; veriyi başka bir lead modelin getirmesini bekleme.
- Tracker'a yeni bulgu kaydetmeden önce `project-tracker` skill'indeki API/CLI yoluyla mevcut kayıtları ara ve ilgili kayıtların içeriğini oku. Yalnız başlık eşleşmesine bakma; bulguyu zaten kapsayan veya aynı sorun/iş akışı bağlamında genişletilebilecek bir madde olup olmadığını kontrol et. Bulgu zaten kapsanıyorsa yalnız mevcut maddeyi yeni kanıt ve ayrıntılarla güncelle; aynı bağlamda ilgili madde varsa yeni kayıt açmak yerine mevcut maddenin açıklamasını ve kapsamını genişlet, önceki içeriğini koru. Yeni kayıt yalnız bulguyu kapsayan veya anlamlı biçimde genişletilebilecek mevcut madde yoksa açılır.
- `tmp` arşivleme ortak `scripts/agents/archive-tmp.sh` üzerinden yönetilir; `tmp-temizlik` kurallarına uy. Oturum kapanışında arşivleme/silme yoktur.

## Rapor ve uyumluluk

- Kullanıcıya kısa, ürün/davranış odaklı Türkçe yaz. Önemli bulguları kanıtla; secret'ları paylaşma.
- Non-trivial rapor `Context Read`, `Protected Invariants`, doğrulama ve açık riskleri içerir.
- `.agents/skills` kanoniktir. Bu Windows alanında `.codex/skills` uyumluluk junction'ıdır; yeni checkout'ta gerekirse `scripts/agents/setup-skill-compat.ps1` kullan. Bağlantı olmadan da `.agents` yolları okunabilir.
- **Skill çift taraflı güncellenir:** `.agents/skills/<ad>` (Codex'in `.codex/skills` bağlantısının hedefi) ile `.claude/skills/<ad>` aynı görevde güncellenir. Yeni skill ve ilgili destek dosyaları da iki tarafta bulunur; teknik kontratlar eşdeğer, araç yolları ve çalışma akışları kendi rollerine uygun olur. İlgili routing kayıtlarını kontrol et ve kapanışta senkronu doğrula. `.claude` dosyaları ileride kullanım için korunur; bu bakım diğer araca erişim gerektirmez.
- `PROJECT_LITE_TASK` veya açık `../project-lite/` görevi varsa o projenin AGENTS/MASTER_PLAN dosyaları önceliklidir; Project referans olarak salt okunur kalır.
