Arka uçta örnek yöntemler
==================================

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [N Emir](#n-emir)
- [Buradaki yönergeler için genel noktalar](#buradaki-y%C3%B6nergeler-i%C3%A7in-genel-noktalar)
- [Geliştirme ortamını README.md dosyasında belgeleyin](#geli%C5%9Ftirme-ortam%C4%B1n%C4%B1-readmemd-dosyas%C4%B1nda-belgeleyin)
- [Veriyi kalıcı olarak depolama](#veriyi-kal%C4%B1c%C4%B1-olarak-depolama)
  - [Genel değerlendirmeler](#genel-de%C4%9Ferlendirmeler)
  - [SaaS, bulutta sunucu ya da kendi sunucunuz?](#saas-bulutta-sunucu-ya-da-kendi-sunucunuz)
  - [Kalıcı deplolama çözümleri](#kal%C4%B1c%C4%B1-deplolama-%C3%A7%C3%B6z%C3%BCmleri)
    - [RDBMS](#rdbms)
    - [NoSQL](#nosql)
      - [Belge tabanlı depolama çözümleri](#belge-tabanl%C4%B1-depolama-%C3%A7%C3%B6z%C3%BCmleri)
      - [Anahtar-değer depolama çözümleri](#anahtar-de%C4%9Fer-depolama-%C3%A7%C3%B6z%C3%BCmleri)
      - [Grafik veritabanları](#grafik-veritabanlar%C4%B1)
- [Environments](#environments)
  - [Lokal geliştirme ortamı](#lokal-geli%C5%9Ftirme-ortam%C4%B1)
  - [CI ortamı](#ci-ortam%C4%B1)
  - [Test ortamı](#test-ortam%C4%B1)
  - [Canlı öncesi (Staging) ortamı](#canl%C4%B1-%C3%B6ncesi-staging-ortam%C4%B1)
  - [Canlı](#canl%C4%B1)
- [Malzeme Listesi](#malzeme-listesi)
- [Güvenlik](#g%C3%BCvenlik)
  - [Docker](#docker)
  - [Kimlik bilgileri](#kimlik-bilgileri)
  - [Gizli veriler](#gizli-veriler)
  - [Giriş Kısıtlama](#giri%C5%9F-k%C4%B1s%C4%B1tlama)
  - [Kullanıcı Şifreleri Depolanması](#kullan%C4%B1c%C4%B1-%C5%9Fifreleri-depolanmas%C4%B1)
  - [Denetim Logları](#denetim-loglar%C4%B1)
  - [Şüpheli Eylem Kısıtlama ve/veya engelleme](#%C5%9F%C3%BCpheli-eylem-k%C4%B1s%C4%B1tlama-veveya-engelleme)
  - [Anonim Veriler](#anonim-veriler)
  - [Geçici dosya depolama](#ge%C3%A7ici-dosya-depolama)
  - [Dedicated vs Shared server environment](#dedicated-vs-shared-server-environment)
- [Application monitoring](#application-monitoring)
  - [Status page](#status-page)
  - [Status page format](#status-page-format)
    - [Plain format](#plain-format)
    - [JSON format](#json-format)
  - [HTTP status codes](#http-status-codes)
  - [Load balancer health checks](#load-balancer-health-checks)
  - [Access control](#access-control)
- [Checklists](#checklists)
  - [Responsibility checklist](#responsibility-checklist)
  - [Release checklist](#release-checklist)
- [General questions to consider](#general-questions-to-consider)
- [Generally proven useful tools](#generally-proven-useful-tools)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# N Emir

1. Ana dizindeki README.md dosyası bu kütüphanenin ana belgesidir
2. Tek komutla çalıştır
3. Tek komutla inşa etmek
4. Tekrarlanabilir ve yeniden yaratılabilir yapılar inşa etmek
5. Yazılım inşaa sonuçları ["Bir Yığın Malzemeden"](#bill-of-materials) oluşur
6. Her zaman [UTC saat dilimi](http://yellerapp.com/posts/2015-01-12-the-worst-server-setup-you-can-make.html) kullan

# Buradaki yönergeler için genel noktalar

Kendimizi belirli teknoloji yığınları veya çerçevelerle sınırlamak istemiyoruz. Farklı problemler farklı çözümler gerektirir ve bu nedenle bu yönergeler her çeşit arka uç mimarisi için geçerlidir.

# Geliştirme ortamını README.md dosyasında belgeleyin

Geliştirme/sunucu ortamının tüm bölümlerini belgeleyin. Aynı kurulum ve sürümleri tüm ortamlarda, geliştirici dizüstü bilgisayarlarından başlayarak ve canlı ortamına kadar kullanmaya çalışın. Bu veritabanını, uygulama sunucusunu, proxy sunucusunu (nginx, Apache, ...), SDK sürümlerini, gemleri/kütüphaneleri/modülleri içerir.

Kurulum işlemini mümkün olduğunca otomatize etmeye çalışın. Örneğin, [Docker Compose](https://docs.docker.com/compose/), hem canlı hem de geliştirme ortamlarını eksiksiz bir şekilde oluşturmak için kullanılabilir ve [Dockerfile](https://docs.docker.com/articles/dockerfile_best-practices/) dosyaları, yazılımın tüm parçalarını derlemek ve ortamı tüm parçaları ile ayarlamak için gerekli komut listesini içerir. Daha sonra paketlerin ulaşılamaz hale gelmesi durumunda, yükleyicilerin arşivlenmiş kopyalarını kullanmayı düşünün. Asgari önlem, paketlerin SHA-1 sağlama toplamlarını tutmak ve paketler yüklendiğinde sağlama toplamının eşleştiğinden emin olmaktır.

Geliştirme ortamının ilgili tüm parçalarını ve bağımlılıklarını kalıcı olarak saklamayı düşünün. Eğer ortamlarınızı Docker ile inşaa edebiliyorsanız, bunu gerçekleştirmek için [docker export](http://docs.docker.com/reference/commandline/cli/#export) kullanabilirsiniz.

# Veriyi kalıcı olarak depolama
## Genel değerlendirmeler

Kullandığınız veri deplolama çözümünden bağımsız olarak aklınızda tutmanız gereken bazı değerlendirmeler:

* Çalıştığına emin olduğunız yedekler tutun
* Bir ortamdan diğerine veri kopyalabilmenizi sağlayan betikler ve araçlara sahip olun, örneğin oluşan bir hatayı tekrarlamak için canlıdan canlı öncesi ortamlara (stage vs.)
* Kalıcı deplolama sistemlerinin gerekli güncellemelerini yapmak için bir planınız olsun (veritabanı sunucusu güvenlik güncellemeleri vs.)
* Dikey ölçekleme yaparken planınız olsun
* Veri yapısı değişiklikleri için bir planınız olsun
* Kalıcı veri çözümünüzü sürekli izleyebildiğiniz çalışan bir gözleme yapınız olsun

## SaaS, bulutta sunucu ya da kendi sunucunuz?

Yaptığımız çözümler için bir önemli seçim de çözümün nerde çalışacağıdır.

* SaaS -- ilk kurulum hızlıca yapılabilir, kolayca dikey olarak ölçeklenebilir, her yerden erişim gibi istekler için az da olsa sistem işlemleri gerekebilir.
* Bulutta sunucu -- veritabanını SaaS'tan daha fazla ayarlamaya izin verir ve kendi sunucunuza sahip olma ile karşılaştırınca büyük olasılıkla daha ucuzdur, ama SaaS'a göre daha emek yoğun bir çözümdür.
* Self-hosted on own hardware -- her şeye ince çekilebilir ve fiziksel güvenliği yönetebilir, ama diğer iki çözüme göre daha pahalıdır ve daha çok emek gerektirir.

## Kalıcı deplolama çözümleri

Bu bölüm, kalıcı depolama çözümünün türünü seçmek için bazı kılavuzlar sağlamayı amaçlamaktadır. Seçim her zaman soruna göre uyarlanmalı ve bunların hiçbiri sihirli değnek değildir.

### RDBMS

Veri ve işlem bütünlüğü büyük bir gereksinim olduğunda veya çok sayıda veri analizi işlemi gerektiğinde PostgreSQL gibi ilişkisel bir veritabanı sistemi seçin. RDBMS seçeneklerinin [ACID uyumluluk](https://en.wikipedia.org/wiki/ACID), birleştirme ve dönüşüm fonksiyonları kararınızda yardımcı olacaktır.

### NoSQL

Yatay ölçeklemeyi beklediğinizde ve ACID kuralları gerekmediğinde bir NoSQL veritabanı seçin. Tabiki veri modelinize uygun bir sistem seçin.

#### Belge tabanlı depolama çözümleri

İçeriğe göre veya koleksiyona dahil edilerek kolayca adreslenebilen ve aranabilen belgeleri saklar. Bu veritabanı depolama formatını anladığı için mümkün olabiliyor. Sadece çok sayıda yapılandırılmış belgenin saklanması için kullanın. Dikkate değer örnekler:

* CouchDB
* ElasticSearch

> 9.4 sürümünden sonra PostgreSQL'in de JSON desteğine sahip olduğu için bu başlıkta değerlendirilebilir.

#### Anahtar-değer depolama çözümleri

Anahtarları ile erişilebilen değerleri veya bazen de anahtar/değer çiftlerinin gruplarını saklar. Değerleri basitçe blob olarak kabul eder, bu nedenle belge depolarının sorgulama yeteneklerini sağlamaz. Büyük boyutlara ölçeklenebilir. Dikkate değer örnekler:

* Cassandra
* Redis

#### Grafik veritabanları

Genel grafik veritabanları, bir grafiğin düğümlerini ve kenarlarını depolar ve herhangi bir düğümün komşularının indekssiz görünümlerini sağlar. En kısa yol veya çap gibi grafik benzeri sorguların çok önemli olduğu uygulamalar için kullanılabilir. Özelleşmiş grafik veritabanları veri kaydı için de kullanılabilir, örneğin. [RDF triples](https://en.wikipedia.org/wiki/Resource_Description_Framework).

# Environments

Bu bölüm en az kaç geliştirme ortamına sahip olmanız gerektiğini açıklamaya çalışıyor. Başlangıçta size çok gelebilir [ama hepsinin bir amacı var](http://futurice.com/blog/five-environments-you-cannot-develop-without).

- [Lokal geliştirme](#lokal-geliştirme-ortamı)
- [CI ortamı](#ci-ortamı)
- [Test ortamı](#test-ortamı)
- [Canlı öncesi (Staging) ortamı](#canlı-oncesi-staging-ortamı)
- [Canlı](#canlı)

## Lokal geliştirme ortamı

Bu sizin kendi geliştirme ortamınızdır. Muhtemelen paylaşılan bir dış geliştirme ortamına sahip olmamalısınız. Bunun yerine, tüm sistemin yerel olarak çalıştırılmasını mümkün kılmak için, gerekli üçüncü taraf araçların taklitlerini veya sahtelerini kullanarak çalışmanız gerekir.

## CI ortamı

CI (diğer ortamların dışında), yazılımınızın derlenmiş hallerinin çalıştığını ve otomatik testlerin her değişiklikten sonra başarı ile geçilmesini sağlamak için kurulan ortamdır.

## Test ortamı

Bu, kodun olabildiğince sık dağıtıldığı, tercihen her zaman kodun ana geliştirme dalına bağlı olduğu ortak bir ortamdır. Özellikle aktif gelişim aşamasında, zaman zaman bozulabilir. Bu önemli bir (canary environment )kanarya ortamıdır ve canlı ortama mümkün olduğu kadar benzemelidir. Kullanılan bir harici servislerin en az "staging" seviyesinde sürümlerinin kurulu olması gerekir.

## Canlı öncesi (Staging) ortamı

Canlı öncesi (prova, staging) tam olarak canlı gibi yapılandırılmalıdır. Hiç bir değişiklik .ilk önce bu ortamda prova edilmeden üretim ortamına gönderilemez. Gizemli (canlı ortamda ortaya çıkan) herhangi bir sorunun burada hata ayıklaması yapılabilir.

## Canlı

İşlemmiş büyük demir parçası. Uygun seviyede log yapılan, devamlı gözlemlenen, periyodik olarak temizlenen, her türlü sorundan uzaklaştırılmaya çalışılmış ve emniyete alınmaya çalışılmış bir ortam.

# Malzeme Listesi

Bu liste her derleme/build sonrası oluşan artifact içine dahil edilmeli ve aşağıdakileri içermeli:

1. Kritik araçların ya da SDK'nın hangi versiyonları  kullanıldı
1. Hangi bağımlılıklar dahil edilmiştir
1. Sonucun global olarak benzersiz bir sürüm numarası (örneğin git SHA-1)
1. Paketi oluştururken kullanılan ortam ve değişkenler
1. Fail eden test ve kontrollerin listesi


# Güvenlik

Olası güvenlik tehditleri ve sorunlarının farkında olun. En azından OWASP Top 10 güvenlik açıklarına aşina olmanız ve kullandığınız üçüncü taraf yazılımlardaki güvenlik açıklarını takip etmeniz gerekir.

Kabul edilebilir genel güvenlik kuralları şöyle olabilir:

## Docker

**Docker kullanmak uygulamanızı daha güvenli yapmayacaktır.** Docker kullanıyorsanız en azından aşağıdakileri yapmayı düşünmelisiniz:

- Güvenli kabul edilmeyen hiç birşey paketi Docker konteynırınızda çalıştırmayın
- Mümkün olduğunda konteynır içinde "root" dışında normal kullanıcılar yaratın ve onlar ile çalışın
- Düzenli aralıklarla konteynırlarınızı güncel kütüphane ve bağımlıklıkları kullanarak yeninden derleyin
- Düzenli aralıklarla Docker sunucunuzu gerekli güvenlik yamalarını yaparak güncelleyin
- Aynı ana bilgisayarda çalışan birden fazla konteynır, varsayılan olarak diğer konteynırlara ve ana bilgisayarın kendisine belli bir erişim düzeyine sahip olacaktır. Tüm ana bilgisayarları doğru şekilde emniyete alın ve minimum erişim grubuna sahip konteynırlar çalıştırın; örneğin, ihtiyaç duymadıklarında ağ erişimini engelleyin.

## Kimlik bilgileri

Asla kimlik bilgilerini hiçbir zaman genel ağ üzerinden şifrelenmemiş şekilde göndermeyin. Her zaman şifreleme kullanın (örneğin HTTPS, SSL vb.).

## Gizli veriler

Sürüm kontrol sistemlerinde gizli olması gereken bilgileri (şifreler, SSH anahtarları vb.) asla saklamayın! Orada olduklarını unutmak çok kolaydır ve proje kaynağı birçok yere (geliştirici makineler, geliştirme test sunucuları vb.) yüklenebilir; bu da tehlikeye atılma riskini gereksiz yere artırır. Ayrıca, sürüm kontrol sistemleri, dosya izinlerinin üzerine yazma konusunda çok kötü bir özelliğe sahiptir, bu nedenle, yapılandırma dosyası izinlerinizi güvence altına alsanız bile, bir sonraki kaynağı kontrol ettiğinizde izinler varsayılan olarak okunabilir ya da varsayılanın üzerine yazılabilir.

Muhtemelen gizli verileri yönetmenin en kolay yolu, onları ihtiyaç duyan sunuculardaki ayrı bir dosyaya koymak ve sürüm kontrolü tarafından yoksaymaktır. Örneğin sürüm kontrolünde bir “.sample” dosyası tutulabilir, o dosyaya gerçekte neyin olması gerektiğini göstermek için sahte değerler koyulabilir. Bazı durumlarda, ana konfigürasyondan ayrı bir konfigürasyon dosyası eklemek kolay değildir. Bu durumda, ortam değişkenlerini kullanmayı veya yapılandırma dosyasını sürüm kontrolündeki bir şablondan  oluşturmayı düşünün.

## Giriş Kısıtlama

Belli bir süre içinde kişi başına izin verilen giriş denemesi miktarına sınırlamalar koyun. Belirli sayıda başarısız denemeden sonra bir kullanıcı hesabını belirli bir süre için kilitleyin. (örneğin, 20 hatalı giriş denemesi sonrasında 5 dakika hesabı kitleyebilirsiniz).

Bu önlemlerin amacı, kullanıcı adlarına/şifrelerine karşı online kaba kuvvet saldırılarını olanaksız kılmaktır.

## Kullanıcı Şifreleri Depolanması

> Asla şifreleri düz metinler olarak tutmayın!

Eğer uygulama / sistem tarafından gerekli değilse, şifreleri asla geri çevirilebilir şifreleme yöntemleri ile kaydetmeyin. Konu ile ilgili güzel bir makale: https://crackstation.net/hashing-security.htm

Veritabanından düz metin parolaları elde etmeniz gerekiyorsa, takip etmeniz gereken bazı öneriler şöyledir.

Şifreler sık sık tekrar tekrar düz metne dönüştürülmezse (örneğin özel prosedür gerekiyorsa), şifre çözme anahtarlarını veritabanına düzenli olarak erişen uygulamadan uzak tutun.

Şifrelerin hala düzenli olarak şifrelerinin çözülmesi gerekiyorsa, şifre çözme işlevini ana uygulamadan mümkün olduğu kadar ayırın (örneğin, ayrı bir sunucu bir şifrenin şifresini çözme isteklerini kabul eder, azaltma, yetkilendirme, vb. gibi daha yüksek bir kontrol düzeyi uygular).

Mümkün olduğunda (vakaların büyük çoğunluğunda olması gerekir), şifreleri iyi bir rastgele değere sahip iyi bir tek yönlü şifreleme kullanarak depolayın. Ve kesinlikle diyebiliriz ki, SHA-1 bu bağlamda bir şifreleme işlevi için iyi bir seçim değildir. Parolalar göz önünde bulundurularak tasarlanan şifreleme işlevler kasıtlı olarak daha yavaştır, bu da çevrimdışı kaba kuvvet saldırılarını daha fazla zaman alan ve dolayısıyla daha az uygulanabilir kılar. Daha detaylı bilgi için: http://security.stackexchange.com/questions/211/how-to-securely-hash-passwords/31846#31846

## Denetim Logları

Hassas verileri işleyen uygulamalarda, özellikle belirli kullanıcıların nispeten geniş bir erişime veya denetime izin verildiği yerlerde, bir tür denetim logları tutmak iyi olur - sistemde gerçekleşen bir dizi eylem/olayı olay/kaynak başlatıcı ile birlikte depolamak iyidir. (kullanıcı, otomasyon işleri, vb). Örneğin:

    2012-09-13 03:00:05 Job "daily_job" performed action "delete old items".
    2012-09-13 12:47:23 User "admin_user" performed action "delete item 123".
    2012-09-13 12:48:12 User "admin_user" performed action "change password of user foobar".
    2012-09-13 13:02:11 User "sneaky_user" performed action "view confidential page 567".
    ...

Günlük, basit bir metin dosyası olabilir veya bir veritabanında saklanabilir. En azından bu üç öğeye sahip olmak iyidir: kesin bir zaman damgası, eylem/olay yaratıcısı (bunu yapan kişi) ve fiili eylem/olay (ne yapıldığı). Günlüğe kaydedilecek kesin işlemler elbette uygulamanın kendisi için neyin önemli olduğuna bağlıdır.

Denetim günlüğü normal uygulama günlüğünün bir parçası olabilir, ancak burada vurgulanan şey, yalnızca belirli bir eylemin gerçekleştirilip gerçekleştirilmediğini yapan ve kimin yaptığını günlüğe kaydetmektir. Mümkünse, denetim günlüğü kurcalamaya karşı korumalı, örn. yalnızca özel bir günlük kaydı işlemi veya kullanıcı tarafından erişilebilir ama doğrudan uygulama tarafından erişilebilir olmamalıdır.

## Şüpheli Eylem Kısıtlama ve/veya engelleme

Bu, Login Throttling'in genelleştirilmesi olarak görülebilir, bu böl
m uygulama bağlamında "şüpheli" kabul edilen keyfi eylemler için benzer mekanizmaları anlatıyor. Örneğin, normal kullanıcıların önemli miktarda bilgiye erişmesine izin veren, ancak kullanıcıların yalnızca bu bilgilerin küçük bir alt kümesiyle ilgilenmelerini bekleyen bir ERP sistemi, beklenen veri setlerinden çok daha hızlı veri girişimlerini sınırlayabilir. Örneğin, Kullanıcıların aynı anda bir veya iki müşteride çalışması gerekiyorsa, kullanıcıların tüm müşterilerin listesini indirmelerini engelleyin. Bunun, erişimi tamamen sınırlamaktan farklı olduğunu unutmayın; kullanıcıların herhangi bir müşteri hakkında bilgi almalarına izin verilir ama aynı anda bütün müşterilerin değil. Sisteme bağlı olarak, kısıtlama yeterli olmayabilir - öreğin. bir kişi tüm kaynaklar üzerinde tek bir istekle işlem başlattığında. O zaman tamamen kısıtlama gerekli olabilir. Tam müşteri bilgilerini almak için bir kerede bir müşteri olmak üzere 10 saniyede 1000 istek yapmak ile ve bu bilgileri bir kerede almak için tek bir istek yapmak arasında farkı not edin.

Örneğin, bir sistemde 10000 kayıt silmek tamamen meşru bir işlem olabilir, ancak diğerinde de olmayabilir.

## Anonim Veriler

Ne zaman büyük veri setleri üçüncü şahıslara ihraç edildiğinde ve verilerin kullanım amacı göz önüne alındığında veriler mümkün olduğunca anonimleştirilmelidir. Örneğin, bir üçüncü taraf hizmeti bir müşteri veritabanında genel istatistiksel analiz sağlayacaksa, muhtemelen bireysel müşterilerin isimlerini, adreslerini veya diğer kişisel bilgilerini bilmesi gerekmez. Genel müşteri kimlik numarası bile, veri kümesine bağlı olarak çok açık olabilir. Bu yazıya bir göz atabilirsiniz: http://arstechnica.com/tech-policy/2009/09/your-secrets-live-online-in-databases-of-ruin/.

Kullanıcı adı gibi kişisel olarak tanımlanabilecek bilgileri loglamaktan kaçının.

Loglarınız hassas bilgiler içeriyorsa, loglarınızın nasıl korunduğunu ve bulutta barındırılan log yönetim sistemleri durumunda da nerede olduklarını bildiğinizden emin olun.

Hassas bilgileri günlüğe kaydetmeniz gerekiyorsa, günlüğe kaydetmeden önce şifrelemeyi deneyin böylece işlemin farklı bölümleri arasında aynı varlığı tanımlayabilirsiniz.

## Geçici dosya depolama

Uygulamanızın geçici dosyaları nerede sakladığını bildiğinizden emin olun. Genel olarak erişilebilen dizinleri (muhtemelen varsayılanıdır) `/tmp` ve` /var/tmp` gibi kullanıyorsanız, dosyalarınızı mod 600 ile oluşturduğunuzdan emin olun, böylece sadece uygulamanızın çalıştığı sistem kullanıcısı tarafından okunabilir. Alternatif olarak, geçici dosyaları saklamak için korumalı bir dizine (sadece uygulama kullanıcısı tarafından erişilebilir dizin) sahip olabilirsiniz.

## Paylaşımsız vs Paylaşımlı sunucu ortamı

Güvenlik tehditleri uygulamanın paylaşımlı ya da paylaşımsız sunucular üzerinde çalışmasına bağlı olarak farklılıklar gösterir. Paylaşımlı demek sunucu üzerinde başka uygulamalar da (üçüncü şahıslara ait olmasına gerek yok) çalışıyor demektir. Paylaşımlı sunucularda doğru dosya izinlerinin olması kritiktir, çünkü bir hata olması durumunda uygulama kaynak kodları, veri dosyaları, geçici dosyalar, loglar gibi bilgiler görmemesi gereken kişilere görünür olabilir. Ayrıca diğer uygulamalardaki bir güvenlik açığı sizin uygulamanızızn açığa çıkmasına da sebebp olabilir.

Başlangıçta paylaşımsız bir sunucuda çalışmaya başlasanız bile uygulamanızın ileride nasıl bir ortamda ya da sunucuda çalışacağına hiçbir zaman emin olamazsınız. Örneğin, sunucuya ileride başka uygulamalar da yüklenebilir. Bunun içindir ki, en iyi plan uygulamanızın her zaman paylaşımlı bir sunucuda çalışacağını düşünmek ve önlemleri ona göre almaktır. İşte düşünmeniz gereken dosyaların/dizinlerin ayrıntılı bir listesi:

* uygulamanızın kaynak kodu
* veri dizinleri
* geçici dosya depolama dizinleri (genellikle ön tanımlı sistem dizinleri (/tmp vs) kullanılır - bir önceki bölüme bakabilirsiniz)
* ayar dosyaları
* sürüm kontrol sistemlerinin ayar dizinleri - .git, .hg, .svn, vs.
* başlangıç betikleri (başlangıç değişkenleri ve gizli bilgilerini içerebilir vs)
* log dosyaları
* çöküş raporları
* özel anahtar dosyalar (SSL, SSH, vs)
* vs.

Bazen bazı dosyalar başka sistem kullanıcıları tarafından da okunması gerekir (örneğin, apache tarafından yayınlanan dosyalar). Bu durumda, sadece gerekli olan kullanıcıların erişmesine izin verin.

UNIX/Linux dosya sistemlerinde yazma izninin çok güçlü bir izin olduğunu unutmayın, çünkü bu izin silme yetkisini de kapsıyor ve bir dosyanın silinip yeniden oluşturularak tamamen değiştirilmesine sebep olabilir. /tmp ve /var/tmp dizinleri üzerinde ayarlanması gereken yapışkan bit (sticky bit) nedeniyle bu etkiden ön tanımlı olarak güvenlidir.

Bunlara ek olarak, gizli veriler bölümünde anlatıldığı gibi dosya izinleri sürüm kontrol sistemlerinde korunmayabilir. Başlangıçta siz düzgün bir şekilde ayarlamış olsanız bile checkout/update gibi komutlar dosya izinlerini değiştirebilirler. Bunun için en güzel çözümlerden biri de bir Makefile, bir betik, sürüm kontrol sisteminde bir kanca işlevi ya da bunlara benzer bir yapı ile her zaman dosya izinlerini güncellemektir.

# Uygulama takibi

Uygulamanızın tam anlamıyla durumunu takip edebilmek için hem işletim sistemi seviyesinde hem de uygulamanıza özel kontroller yapmalısınız. İşletim sistemi seviyesinde yapılacak kontroller CPU, depolama, hafıza kullanımı, çalışan uygulamalar, açık portlar vs gibi kontrolleri içerir. Ancak, uygulamaya özel kontroller hizmet çalışması açısından daha önemlidir. Bu kontroller "bu URL cevap veriyor mu ve HTTP 200 dönüyor mu" gibi kontrollerden başlayıp veritabanı bağlantısından veri tutarlılığına kadar genişleyebilir.

Bu bölüm, uygulamaya özel kontrollerin uygulanmasının, genel uygulama sağlığının izlenmesini kolaylaştıracak ve somut uygulama bağlamında hangi kontrollerin anlamlı olduğunu belirlemek için uygulama geliştiricilerine tam kontrol sağlayan yöntemleri açıklamaktadır.

İşin özünde, en güzel yol uygulamanın durumu hakkında toplu bir bilgi veren tek bir erişim noktası (uyuglamada çalışan bir URL) olmasıdır.  Bunun uygulama içinde yapılması gerekir ve proje ekibinin dahil olması gereklidir, çünkü proje ekibi uygulamanın OK durumunun ne olduğunu ve ERROR durumunun ne olduğunu gerçekten tanımlayabilen ekiptir.

Uygulama, herhangi bir sayıda "alt sistem" kontrolü de uygulayabilir. Örneğin,

* veritabanı bağlantısı sağlıklı mı
* veri bütünlüğü korunuyor mı (Örneğin, bir tablodaki kayıtlar doğru mu)
* uygulamanın iletişimde olduğu harici servisler erişilebilir mi
* ElasticSearch indeksleri tutarlı mı
* uygulama için anlamlı olan herşey

Alt sistem kontrollerinden gelen bilgilerle birleştirilerek, uygulama tarafından toplu bir genel durum gösterimi sağlanmalıdır. Buradaki fikir, bir harici izleme sisteminin yalnızca bu birleştirilmiş genel bakışı izlemesidir, böylece yeni bir uygulama kontrolü eklendiğinde veya değiştirildiğinde, harici izlemenin yeniden yapılandırılması gerekmez. Dahası, geliştiriciler genel durumun alt sistem kontrollerine neyin dayandığına karar verebilecek olan kişilerdir. (örneğin, hangisi önemli hangisi değil vs).

## Durum sayfası

Bütün durum kontrol sonuçları `/status` sayfasında aşağıdakiler gibi erişilebilir olmalı:

* `/status` - toplam durum sayfası (zorunlu)
* `/status/subsystem1` - belli bir alt sisteme ait durum sayfası (zorunlu değil)
* ...

Ana `/status` sayfası bir sonraki bölümde açıklandığı gibi en azından sistemin genel durumunu vermelidir. This means that the main `/status` page should execute ALL subsystem checks and report the aggregated overall system status. It is up to the developers to decide how the overall system status is determined based on the subsystems. For example an `ERROR` state of some non-critical subsystem may only generate an overall `WARNING` status.

For performance reasons, some subsystem checks may be excluded from this overall `/status` page - for example, when the check causes higher resource usage, takes longer time to complete, etc. Overall, the main status page should be light enough so that it can be polled relatively often (every 1-3 minutes) and not cause too much load on the system. Subsystem checks that are excluded from the overall status check should have their own URLs, as shown above. Naturally, monitoring those would require modifications in the monitoring system configuration. To overcome this, a different approach can be taken: the application could perform the heavy subsystem checks in a background process at a rate that is acceptable and store the status internally. This would allow the main status page to reflect also these heavy checks (e.g. it would retrieve the last performed check status). This approach should be used, unless its implementation is too difficult.

## Status page format

We propose two alternative formats for the status pages - `plain` and `JSON`.

### Plain format

The plain format has one status per line in the form `key: value`. The key is a subsystem/check name and the value is the status value. The status value can be one of:

* `OK`
* `WARN Message`
* `ERROR Message`

where `Message` can be some meaningful text, that can help quickly identify the problem. The message is single line, without specified length restriction, but use common sense - e.g. probably should not be longer than 200 characters.

The main status page MUST have a key called `status` that shows the overall aggregated application status. The individual subsystem check status lines are optional. Subsystem status keys should have a suffix `_status`. Here are some examples:

When everything is ok:

```
status: OK
database_status: OK
elastic_search_status: OK
```

When some check is failing:

```
status: ERROR Database is not accessible
database_status: ERROR Connection failed
elastic_search_status: OK
```

Multiple failures at the same time:

```
status: ERROR failed subsystems: database, elasticsearch. For details see https://myapp.example.com/status
database_status: ERROR Connection failed
elastic_search_status: WARN Too few entries in index A.
```

In addition to the status lines, a status page can have non-status keys. For example, those can be showing some metrics (that may or may not be monitored). The additional keys must be prefixed with the subsystem name.

```
status: OK
database_status: OK
database_customers: 378
database_items: 8934748
elastic_search_status: OK
elastic_search_shards: 20
```

The overall status may naturally be based on some of the metrics:

```
status: WARN Too few items in database
database_status: WARN Too few customers in database
database_customers: 378
database_items: 1
elastic_search_status: OK
elastic_search_shards: 20
```

Subsystem checks that have their own URL (`/status/subsystemX`) should follow a similar format, having a mandatory key `status` and a number of optional additional keys. Example for e.g. `/status/database`:

```
status: OK
connection_pool: 30
latency: 2
```

### JSON format

The JSON format of the status pages can be often preferable, for example when the tooling or integration to other systems is easier to achieve via a common data format.

The status values follow the same format as described above - `OK`, `WARN Message` and `ERROR Message`.

The equivalent to the status key form the plain format is a `status` key in the root JSON object. Subsystems should use nested objects also having a mandatory `status` key. Here are some examples:

All is fine:

```json
{
    "status": "OK",
    "database": {
        "status": "OK"
    },
    "elastic_search": {
        "status": "OK"
    }
}
```

Status page with additional metrics:

```json
{
    "status": "OK",
    "uptime": 18234,
    "database": {
        "status": "OK",
        "connection_pool": 30
    },
    "elastic_search": {
        "status": "OK",
        "multinode": false
    }
}
```

Something failing:

```json
{
    "status": "ERROR Database is not accessible. See https://myapp.example.com/status for details.",
    "database": {
        "status": "ERROR Connection failed",
        "connection_timeout": 30
    },
    "elastic_search": {
        "status": "OK"
    }
}
```

## HTTP status codes

Whenever the overall application status is OK, the HTTP status code in the status page response MUST be set to 200 (OK). Otherwise a 5XX error code SHOULD be set. For example, code 500 (Internal Server Error) could be used. Optionally, non-critical WARN status may still respond with 200.

## Load balancer health checks

Often the application is running behind a load balaner. Load balancers typically can monitor application servers by polling a given URL. The health check is used so that the load balancer can stop routing traffic to the failing application servers.

The overall `/status` page is a good candidate for the load balancer health check URL. However, a separate dedicated status page for a load balancer health check provides an important benefit. Such a page can be fine-tuned for when the application is considered to be healthy from the load balancer's perspective. For example, an error in a subsystem may still be considered a critical error for the overall application status, but does not necessarily need to cause the application server to be removed from the load balancer pool. A good example is a 3rd party integration status check. The load balancer health check page should only return non-200 status code when the application instance must be considered non-operational.

The load balancer health check page should be placed at a `/status/health` URL. Depending on your load balancer, the format of that page may deviate from the overall status format described here. Some load balancers may even observe only the returned HTTP status code.

## Access control

The status pages may need proper authorization in place, especially in case they expose debugging information in status messages or application metrics. HTTP basic authentication or IP-based restrictions are usually good enough candidates to consider.

# Checklists

To avoid forgetting the most important things, here are some handy checklists for your current or upcoming projects.

## Responsibility checklist

In bigger projects, especially when multiple parties are involved, it is crucial to keep track of all different aspects and its responsibilities. The following table illustrates how a go-live checklist for releasing a website could look like:

| Aspect    | Task                              | Responsible person / party | Deadline     | Status            |
|---        |---                                |---                         |---           |---                |
| Frontend  | Website wireframes                | e.g. Company B / Person X  | e.g. 17.6.   |  e.g. in progress |
| Frontend  | Website design                    | e.g. Company A / Person Z  | e.g. 23.7.   |  e.g. waiting     |
| Frontend  | Website templates                 |   |   |   |
| Frontend  | Content creation and population   |   |   |   |
| Backend   | Setup CMS                         |   |   |   |
| Backend   | Setup staging environment         |   |   |   |
| Backend   | Setup production environment      |   |   |   |
| Backend   | Migrate hosting services to client accounts |   |   |   |
| Backend   | DNS configuration                 |   |   |   |
| Backend   | Setup website analytics           |   |   |   |
| Backend   | Integrate marketing automation    |   |   |   |
| Backend   | Web font license                  |   |   |   |
| Dates     | Website/Product go-live time      |   |   |   |
| Dates     | Publish the website               |   |   |   |

## Release checklist

When you are ready to release, remember to check off everything on your release checklist! The resulting peace of mind, repeatability and dependability is a great boon.

You *do* have one, right? If you don't, here is a good generic starting point for you:

* [ ] Deploying works the same no matter which environment you are deploying to
* [ ] All environments have well defined names, and they are referred to using those names
* [ ] All environments have the same underlying software stack
* [ ] All environment configuration is version controlled (web server config, CI build scripts etc.)
* [ ] The product has been tested from the networks from where it will be used (e.g. public Internet, customer LAN)
* [ ] The product has been tested with all of the targeted devices
* [ ] There is a simple way to find out what code is running in any given environment
* [ ] A versioning scheme has been defined
* [ ] Any version of the product should be easily mappable to a state of the code base
* [ ] Rolling back a deployment is possible
* [ ] Backups are running
* [ ] Restoring from a backup has been tested
* [ ] No secrets are stored in version control
* [ ] Logging is turned on
* [ ] There is a well defined process for accessing and searching through logs
* [ ] Logging includes exceptions and stack traces where appropriate
* [ ] Errors can be mapped to stack traces
* [ ] Release notes have been written
* [ ] Server environments are up-to-date
* [ ] A plan for updating the server environments exists
* [ ] The product has been load tested
* [ ] A method exists for replicating the state of one environment in another (e.g. copy prod to QA to reproduce an error)
* [ ] All repeating release processes have been automated

# General questions to consider

* What is the expected/required life-span of the project?
* Is the project one-off, or will there be continuous development?
* What is the release cycle for a version of the service?
* What environments (dev, test, staging, prod, ...) are going to be set up?
* How will downtime of the production service impact the value of the service?
* How mature is the technology? Is major changes that break backward compatibility to be expected?

# Generally proven useful tools

* [HTTPie](https://github.com/jakubroztocil/httpie) is a great tool for testing APIs on the command line. It's simple to pass in custom headers and cookies, and it even has session support.
* [jq](http://stedolan.github.io/jq/) is a CLI JSON processor. Massage JSON data coming in from cURL (or of course HTTPie!) at will. Another great tool for API testing or exploration.

# License

[Futurice Oy](http://www.futurice.com)
Creative Commons Attribution 4.0 International (CC BY 4.0)
