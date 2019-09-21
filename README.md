Arka uçta örnek yöntemler
==================================

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Belgenin çevirileri](#belgenin-%C3%A7evirileri)
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
- [Geliştirme Ortamları](#geli%C5%9Ftirme-ortamlar%C4%B1)
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
  - [Paylaşımsız vs Paylaşımlı sunucu ortamı](#payla%C5%9F%C4%B1ms%C4%B1z-vs-payla%C5%9F%C4%B1ml%C4%B1-sunucu-ortam%C4%B1)
- [Uygulama takibi](#uygulama-takibi)
  - [Durum sayfası](#durum-sayfas%C4%B1)
  - [Status sayfası formatı](#status-sayfas%C4%B1-format%C4%B1)
    - [Düz format](#d%C3%BCz-format)
    - [JSON format](#json-format)
  - [HTTP durum kodları](#http-durum-kodlar%C4%B1)
  - [Yük dengeleyici kontrolleri](#y%C3%BCk-dengeleyici-kontrolleri)
  - [Erişim kısıtlaması](#eri%C5%9Fim-k%C4%B1s%C4%B1tlamas%C4%B1)
- [Kontrol listesi](#kontrol-listesi)
  - [Sorumluluk kontrol listesi](#sorumluluk-kontrol-listesi)
  - [Yayın kontrol listesi](#yay%C4%B1n-kontrol-listesi)
- [Dikkat edilmesi gereken sorular](#dikkat-edilmesi-gereken-sorular)
- [Faydalı olduğu kabul edilebilir araçlar](#faydal%C4%B1-oldu%C4%9Fu-kabul-edilebilir-ara%C3%A7lar)
- [Lisans](#lisans)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Belgenin çevirileri

Bunlar, bu belgenin gönüllüler tarafından sağlanan çevirileridir. Herhangi bir çeviriyle ilgili yorumlarınız varsa, lütfen çevirinin sağlayıcısına başvurun.

- [Türkçe](https://github.com/umutphp/backend-best-practices) çeviri [umutphp](https://github.com/umutphp) tarafından yapıldı.

# N Emir

1. Ana dizindeki README.md dosyası bu kütüphanenin ana belgesidir
2. Tek komutla çalıştır
3. Tek komutla paketi çık
4. Tekrarlanabilir ve yeniden yaratılabilir yapılar inşa et
5. Paketlerle beraber bir ["Bir Yığın Malzemeden"](#malzeme-listesi) oluştur
6. Her zaman [UTC saat dilimi](http://yellerapp.com/posts/2015-01-12-the-worst-server-setup-you-can-make.html) kullan

# Buradaki yönergeler için genel noktalar

Bu belgede kendimizi belirli teknoloji yığınları veya çerçevelerle sınırlamak istemiyoruz. Farklı problemler farklı çözümler gerektirir ve bu nedenle bu yönergeler her çeşit arka uç mimarisi için geçerlidir.

# Geliştirme ortamını README.md dosyasında belgeleyin

Geliştirme/sunucu ortamının tüm bölümlerini belgeleyin. Aynı kurulum ve sürümleri tüm ortamlarda, geliştirici dizüstü bilgisayarlarından başlayarak ve canlı ortamına kadar kullanmaya çalışın. Bu tavsiye veritabanını, uygulama sunucusunu, proxy sunucusunu (nginx, Apache, ...), SDK sürümlerini, gemleri/kütüphaneleri/modülleri kapsar.

Kurulum işlemini mümkün olduğunca otomatize etmeye çalışın. Örneğin, [Docker Compose](https://docs.docker.com/compose/), hem canlı hem de geliştirme ortamlarını eksiksiz bir şekilde oluşturmak için kullanılabilir ve [Dockerfile](https://docs.docker.com/articles/dockerfile_best-practices/) dosyaları, yazılımın tüm parçalarını derlemek ve ortamı tüm parçaları ile ayarlamak için gerekli komut listesini içerebilir. Daha sonra paketlerin ulaşılamaz hale gelmesi durumunda, yükleyicilerin arşivlenmiş kopyalarını kullanmayı düşünün. Asgari önlem, paketlerin SHA-1 sağlama toplamlarını tutmak ve paketler yüklendiğinde sağlama toplamının eşleştiğinden emin olmaktır.

Geliştirme ortamının ilgili tüm parçalarını ve bağımlılıklarını kalıcı olarak saklamayı düşünün. Eğer ortamlarınızı Docker ile inşaa edebiliyorsanız, bunu gerçekleştirmek için [docker export](http://docs.docker.com/reference/commandline/cli/#export) kullanabilirsiniz.

# Veriyi kalıcı olarak depolama
## Genel değerlendirmeler

Kullandığınız veri deplolama çözümünden bağımsız olarak aklınızda tutmanız gereken bazı noktalar:

* Çalıştığına emin olduğunız yedekler tutun
* Bir ortamdan diğerine veri kopyalabilmenizi sağlayan betikler ve araçlara sahip olun, örneğin oluşan bir hatayı tekrarlamak için canlıdan canlı öncesi ortamlara (stage vs.)
* Kalıcı deplolama sistemlerinin gerekli güncellemelerini yapmak için bir planınız olsun (veritabanı sunucusu güvenlik güncellemeleri vs.)
* Dikey ölçekleme yaparken planınız olsun
* Veri yapısı değişiklikleri için bir planınız olsun
* Depolama sistemini sürekli izleyebildiğiniz çalışan bir gözleme yapınız olsun

## SaaS, bulutta sunucu ya da kendi sunucunuz?

Yaptığımız çözümler için bir önemli seçim de çözümün nerde çalışacağıdır.

* SaaS -- ilk kurulum hızlıca yapılabilir, kolayca dikey olarak ölçeklenebilir, her yerden erişim gibi istekler için az da olsa sistem işlemleri gerekebilir.
* Bulutta sunucu -- veritabanını SaaS'tan daha fazla ayarlamaya izin verir ve kendi sunucunuza sahip olma ile karşılaştırınca büyük olasılıkla daha ucuzdur, ama SaaS'a göre daha emek yoğun bir çözümdür.
* Self-hosted on own hardware -- her şeye ince ayar çekilebilir ve fiziksel güvenliği yönetilebilir, ama diğer iki çözüme göre daha pahalıdır ve daha çok emek gerektirir.

## Kalıcı deplolama çözümleri

Bu bölüm, kalıcı depolama çözümünün türünü seçmek için bazı kılavuzlar sağlamayı amaçlamaktadır. Seçim her zaman çözülmeye çalışılan soruna göre uyarlanmalı ve bunların hiçbirinin sihirli bir değnek olmadığı unutulmamalıdır.

### RDBMS

Veri ve işlem bütünlüğü büyük bir gereksinim olduğunda veya çok sayıda veri analizi işlemi gerektiğinde PostgreSQL gibi ilişkisel bir veritabanı sistemi seçin. RDBMS seçeneklerinin [ACID uyumlu olması](https://en.wikipedia.org/wiki/ACID), birleştirme ve dönüşüm fonksiyonları kararınızda yardımcı olacaktır.

### NoSQL

Yatay ölçeklemeyi beklediğinizde ve ACID kuralları gerekmediğinde bir NoSQL veritabanı seçin. Tabiki veri modelinize uygun bir sistem seçin.

#### Belge tabanlı depolama çözümleri

İçeriğe göre veya her hangi bir koleksiyona dahil edilerek kolayca adreslenebilen ve aranabilen belgeleri saklar. Bu işlev veritabanı, depolama formatını anladığı için mümkün olabiliyor. Sadece ve sadece çok sayıda yapılandırılmış belgenin saklanması için kullanın. Dikkate değer örnekler:

* CouchDB
* ElasticSearch

> 9.4 sürümünden sonra PostgreSQL'in de JSON desteğine sahip olduğu için bu başlıkta değerlendirilebilir.

#### Anahtar-değer depolama çözümleri

Anahtarları ile erişilebilen değerleri veya bazen de anahtar/değer çiftlerinin gruplarını saklar. Değerleri basitçe blob olarak kabul eder, bu nedenle belge depolarının sorgulama yeteneklerini sağlamaz. Büyük boyutlara ölçeklenebilir. Dikkate değer örnekler:

* Cassandra
* Redis

#### Grafik veritabanları

Genel grafik veritabanları, bir grafiğin düğümlerini ve kenarlarını depolayıp herhangi bir düğümün komşularının indekssiz ulaşılabilmesini sağlar. En kısa yol veya çap gibi grafik benzeri sorguların çok önemli olduğu uygulamalar için kullanılabilir. Özelleşmiş grafik veritabanları veri kaydı için de kullanılabilir. Örneğin, [RDF triples](https://en.wikipedia.org/wiki/Resource_Description_Framework).

# Geliştirme Ortamları

Bu bölüm en az kaç geliştirme ortamına sahip olmanız gerektiğini açıklamaya çalışıyor. Başlangıçta size çok gelebilir [ama hepsinin bir amacı var](http://futurice.com/blog/five-environments-you-cannot-develop-without).

- [Lokal geliştirme](#lokal-geliştirme-ortamı)
- [CI ortamı](#ci-ortamı)
- [Test ortamı](#test-ortamı)
- [Canlı öncesi (Staging) ortamı](#canlı-oncesi-staging-ortamı)
- [Canlı](#canlı)

## Lokal geliştirme ortamı

Bu sizin kendi geliştirme ortamınızdır. Muhtemelen paylaşılan bir dış geliştirme ortamına sahip olmamalısınız. Bunun yerine, tüm sistemin yerel olarak çalıştırılmasını mümkün kılmak için, gerekli üçüncü taraf araçların taklitlerini veya sahtelerini kullanarak çalışmanız gerekir.

## CI ortamı

CI (diğer ortamların dışında), yazılımınızın derlenmiş hallerinin çalıştığı ve otomatik testlerin her değişiklikten sonra başarı ile geçirilmesini sağlamak için kurulan ortamdır.

## Test ortamı

Bu ortam kodun olabildiğince sık dağıtıldığı ve tercihen her zaman kodun ana geliştirme dalına bağlı olduğu ortak bir ortamdır. Özellikle aktif gelişim aşamasında bu ortam zaman zaman bozulabilir. Bu önemli bir (canary environment) kanarya ortamıdır ve canlı ortama mümkün olduğu kadar benzemelidir. Kullanılan harici servislerin en az "staging" seviyesinde sürümlerinin kurulu olması gerekir.

## Canlı öncesi (Staging) ortamı

Canlı öncesi (prova da denilebilir) ortam tam olarak canlı gibi yapılandırılmalıdır. Hiç bir değişiklik ilk önce bu ortamda prova edilmeden üretim ortamına gönderilemez. Gizemli (canlı ortamda ortaya çıkan) herhangi bir sorunun burada hata ayıklaması yapılabilir.

## Canlı

İşlemmiş büyük demir parçası. Uygun seviyede log yapılan, devamlı gözlemlenen, periyodik olarak temizlenen, her türlü sorundan uzaklaştırılmaya çalışılmış ve emniyete alınmaya çalışılmış bir ortam.

# Malzeme Listesi

Bu liste her derleme/build sonrası oluşan sonuç paketi içine dahil edilmeli ve aşağıdakileri içermeli:

1. Kritik araçların ya da SDK'nın hangi versiyonları kullanıldığı
1. Hangi bağımlılıkları dahil edildiği
1. Sonuç pakitin global olarak benzersiz bir sürüm numarası (örneğin git commit SHA-1 numarası)
1. Paketi oluştururken kullanılan ortam ve değişkenler
1. Hata oluşturan eden test ve kontrollerin listesi


# Güvenlik

Olası güvenlik tehditleri ve sorunlarının farkında olmaya çalışın. En azından "OWASP Top 10" listesindeki güvenlik açıklarına aşina olmanız ve kullandığınız üçüncü taraf yazılımlardaki güvenlik açıklarını takip etmeniz gerekir.

Kabul edilebilir genel güvenlik kuralları şöyle olabilir:

## Docker

**Docker kullanmak uygulamanızı daha güvenli yapmayacaktır.** Docker kullanıyorsanız en azından aşağıdakileri yapmayı düşünmelisiniz:

- Güvenli kabul edilmeyen hiç birşey paketi Docker konteynırınızda çalıştırmayın
- Mümkün olduğunda konteynır içinde "root" dışında normal kullanıcılar yaratın ve onlar ile çalışın
- Düzenli aralıklarla konteynırlarınızı güncel kütüphane ve bağımlıklıkları kullanarak yeninden derleyin
- Düzenli aralıklarla Docker sunucunuzu gerekli güvenlik yamalarını yaparak güncelleyin
- Aynı ana bilgisayarda çalışan birden fazla konteynır, varsayılan olarak diğer konteynırlara ve ana bilgisayarın kendisine belli bir erişim düzeyine sahip olacaktır. Tüm ana bilgisayarları doğru şekilde emniyete alın ve minimum erişim grubuna sahip konteynırlar çalıştırın; örneğin, ihtiyaç duymadıklarında ağ erişimini engelleyin.

## Kimlik bilgileri

Kimlik bilgilerini asla genel ağ üzerinden şifrelenmemiş şekilde göndermeyin. Her zaman şifreleme kullanın (örneğin HTTPS, SSL vb.).

## Gizli veriler

Sürüm kontrol sistemlerinde gizli olması gereken bilgileri (şifreler, SSH anahtarları vb.) asla saklamayın! Orada olduklarını unutmak çok kolaydır ve proje kaynağı birçok yere (geliştirici makineler, geliştirme test sunucuları vb.) yüklenebilir; bu da tehlikeye maruz kalma riskini gereksiz yere artırır. Ayrıca, sürüm kontrol sistemleri, dosya izinlerinin üzerine yazma konusunda çok kötü bir özelliğe sahiptir, bu nedenle, yapılandırma dosyası izinlerinizi güvence altına alsanız bile, kodunuz sürüm kontrol sisteminden alındığında izinler varsayılan olarak okunabilir ya da varsayılanın üzerine yazılabilir.

Muhtemelen gizli verileri yönetmenin en kolay yolu, onları ihtiyaç duyan sunuculardaki ayrı bir dosyaya koymak ve sürüm kontrolü tarafından yoksaymaktır. Örneğin sürüm kontrolünde “.sample” uzantılı bir dosya tutulabilir, o dosyaya gerçekte neyin olması gerektiğini göstermek için sahte değerler koyulabilir. Bazı durumlarda, ana konfigürasyondan ayrı bir konfigürasyon dosyası eklemek kolay değildir. Bu durumda, ortam değişkenlerini kullanmayı veya yapılandırma dosyasını sürüm kontrolündeki bir şablondan  oluşturmayı düşünün.

## Giriş Kısıtlama

Belli bir süre içinde kişi başına izin verilen giriş denemesi sayısına sınır koyun. Belirli sayıda başarısız denemeden sonra bir kullanıcı hesabını bir süre için kilitleyin. (Örneğin, 20 hatalı giriş denemesi sonrasında 5 dakika hesabı kitleyebilirsiniz).

Bu önlemlerin amacı, kullanıcı adlarına/şifrelerine karşı kaba kuvvet saldırılarını olanaksız kılmaktır.

## Kullanıcı Şifreleri Depolanması

> Asla şifreleri düz metin olarak tutmayın!

Eğer uygulama/sistem için gerekli değilse, şifreleri asla geri çevirilebilir şifreleme yöntemleri ile saklamayın. Konu ile ilgili güzel bir makale: https://crackstation.net/hashing-security.htm

Veritabanından parolaları düz metin olarak çekmeniz gerekiyorsa, takip etmeniz gereken bazı öneriler şöyledir.

Şifreler sık sık, tekrar tekrar düz metne dönüştürülmesi gerekmiyorsa (örneğin özel prosedür gerekiyorsa), şifre çözme anahtarlarını veritabanına düzenli olarak erişen uygulamadan uzak tutun.

Eğer şifrelerin düzenli olarak düz metne dönüştürülmesi gerekiyorsa, şifre çözme işlevini ana uygulamadan mümkün olduğu kadar ayırın (örneğin, ayrı bir sunucu bir şifrenin şifresini çözme isteklerini kabul eder, azaltma, yetkilendirme, vb. gibi daha yüksek bir kontrol düzeyi uygular).

Mümkün olduğunda (vakaların büyük çoğunluğunda olması gerekir), şifreleri güçlü bir rastgele değere sahip tek yönlü bir şifreleme yöntemi kullanarak depolayın. Ve kesinlikle diyebiliriz ki, SHA-1 bu bağlamda bir şifreleme işlemi için iyi bir seçim değildir. Parolalar göz önünde bulundurularak tasarlanan şifreleme yöntemleri kasıtlı olarak daha yavaştır, bu da çevrimdışı kaba kuvvet saldırılarını daha fazla zaman alan ve dolayısıyla daha az uygulanabilir kılar. Daha detaylı bilgi için: http://security.stackexchange.com/questions/211/how-to-securely-hash-passwords/31846#31846

## Denetim Logları

Hassas verileri işleyen uygulamalarda, özellikle de belirli kullanıcıların nispeten geniş bir erişime veya denetime izin verildiği yazılımlarda, bir tür denetim logları tutmak iyi olur - sistemde gerçekleşen eylem veya olayı olay ve yapan bilgisi ile birlikte depolamak iyidir (kullanıcı, otomasyon işleri, vb). Örneğin:

    2012-09-13 03:00:05 Job "daily_job" performed action "delete old items".
    2012-09-13 12:47:23 User "admin_user" performed action "delete item 123".
    2012-09-13 12:48:12 User "admin_user" performed action "change password of user foobar".
    2012-09-13 13:02:11 User "sneaky_user" performed action "view confidential page 567".
    ...

Bu log basit bir metin dosyası olabilir veya veritabanında saklanabilir. En azından bu üç öğeye sahip olması iyidir: kesin bir zaman damgası, eylem/olay yaratıcısı (bunu yapan kişi) ve eylem/olay (ne yapıldığı). Loga kaydedilecek işlemler elbette uygulamanın kendisi için neyin önemli olduğuna bağlıdır.

Denetim logu normal uygulama logunun bir parçası olabilir, ancak burada vurgulanan şey yalnızca belirli bir eylemin gerçekleştirilip gerçekleştirilmediğini ve kimin yaptığını günlüğe kaydetmektir. Mümkünse denetim logu kurcalamaya karşı korumalı ve doğrudan uygulama tarafından erişilebilir olmamalıdır (Örneğin, yalnızca özel bir log işlemi veya kullanıcı tarafından erişilebilir olmalıdır).

## Şüpheli Eylem Kısıtlama ve/veya engelleme

Bu bölüm giriş kısıtlamasının genelleştirilmesi olarak görülebilir ve uygulama bağlamında "şüpheli" kabul edilen keyfi eylemler için benzer mekanizmaları anlatıyor. Örneğin, normal kullanıcıların önemli miktarda bilgiye erişmesine izin veren, ancak kullanıcıların yalnızca bu bilgilerin küçük bir alt kümesiyle ilgilenmelerini bekleyen bir ERP sistemi, beklenen veri setlerinden çok daha hızlı veri girişimlerini sınırlayabilir. Örneğin, kullanıcıların aynı anda bir veya iki müşteride çalışması gerekiyorsa, kullanıcıların tüm müşterilerin listesini indirmelerini engelleyin. Bunun, erişimi tamamen sınırlamaktan farklı olduğunu unutmayın; kullanıcıların herhangi bir müşteri hakkında bilgi almalarına izin verilir ama aynı anda bütün müşterilerin değil. Sisteme bağlı olarak, kısıtlama yeterli olmayabilir. Örneğin, bir kişi tüm kaynaklar üzerinde tek bir istekle işlem başlattığında. O zaman tamamen kısıtlama gerekli olabilir. Tüm müşteri bilgilerini almak için bir kerede bir müşteri olmak üzere 10 saniyede 1000 istek yapmak ile ve bu bilgileri bir kerede almak için tek bir istek yapmak arasında farka dikkat edin.

Örneğin, bir sistemde 10000 kayıt silmek tamamen meşru bir işlem olabilir, ancak diğerinde de olmayabilir.

## Anonim Veriler

Ne zaman veri setleri üçüncü şahıslara ihraç edildiğinde, kullanım amacı göz önüne alındığınarak veriler mümkün olduğunca anonimleştirilmelidir. Örneğin, bir üçüncü taraf hizmeti bir müşteri veritabanında genel istatistiksel analiz sağlayacaksa, muhtemelen bireysel müşterilerin isimlerini, adreslerini veya diğer kişisel bilgilerini bilmesi gerekmez. Genel müşteri kimlik numarası bile, veri kümesine bağlı olarak çok açık olabilir. Bu yazıya bir göz atabilirsiniz: http://arstechnica.com/tech-policy/2009/09/your-secrets-live-online-in-databases-of-ruin/.

Kullanıcı adı gibi kişisel olarak tanımlanabilecek bilgileri loglamaktan kaçının.

Loglarınız hassas bilgiler içeriyorsa, loglarınızın nasıl korunduğunu ve bulutta barındırılan log yönetim sistemleri kullanıdığınız durumda da logların nerede tutulduklarını bildiğinizden emin olun.

Hassas bilgileri günlüğe kaydetmeniz gerekiyorsa, günlüğe kaydetmeden önce şifrelemeyi deneyin böylece işlemin farklı bölümleri arasında aynı değerleri tanımlayabilirsiniz.

## Geçici dosya depolama

Uygulamanızın geçici dosyaları nerede sakladığını bildiğinizden emin olun. Genel olarak erişilebilen dizinleri (muhtemelen varsayılanıdır) `/tmp` ve` /var/tmp` gibi kullanıyorsanız, dosyalarınızı `mod 600` ile oluşturduğunuzdan emin olun, böylece sadece uygulamanızın çalıştığı sistem kullanıcısı tarafından okunabilir. Alternatif olarak, geçici dosyaları saklamak için korumalı bir dizine (sadece uygulama kullanıcısı tarafından erişilebilir dizin) sahip olabilirsiniz.

## Paylaşımsız vs Paylaşımlı sunucu ortamı

Güvenlik tehditleri uygulamanın paylaşımlı ya da paylaşımsız sunucular üzerinde çalışmasına bağlı olarak farklılıklar gösterir. Paylaşımlı demek sunucu üzerinde başka uygulamalar da (üçüncü şahıslara ait olmasına gerek yok) çalışıyor demektir. Paylaşımlı sunucularda doğru dosya izinlerinin olması kritiktir, çünkü bir hata olması durumunda uygulama kaynak kodları, veri dosyaları, geçici dosyalar, loglar gibi bilgiler görmemesi gereken kişilere görünür olabilir. Ayrıca diğer uygulamalardaki bir güvenlik açığı sizin uygulamanızın açığa çıkmasına da sebep olabilir.

Başlangıçta paylaşımsız bir sunucuda çalışmaya başlasanız bile uygulamanızın ileride nasıl bir ortamda ya da sunucuda çalışacağına hiçbir zaman emin olamazsınız. Örneğin, sunucuya ileride başka uygulamalar da yüklenebilir. Bunun içindir ki, en iyi plan uygulamanızın her zaman paylaşımlı bir sunucuda çalışacağını düşünmek ve önlemleri ona göre almaktır. İşte düşünmeniz gereken dosyaların/dizinlerin ayrıntılı bir listesi:

* uygulamanızın kaynak kodu
* veri dizinleri
* geçici dosya depolama dizinleri (genellikle ön tanımlı sistem dizinleri (/tmp vs) kullanılır - bir önceki bölüme bakabilirsiniz)
* ayar dosyaları
* sürüm kontrol sistemlerinin ayar dizinleri - .git, .hg, .svn, vs.
* başlangıç betikleri (başlangıç değişkenleri ve gizli bilgilerini içerebilir vs)
* log dosyaları
* hata raporları
* özel anahtar dosyalar (SSL, SSH, vs)
* vs.

Bazen bazı dosyalar başka sistem kullanıcıları tarafından da okunması gerekir (örneğin, apache tarafından yayınlanan dosyalar). Bu durumda, sadece gerekli olan kullanıcıların erişmesine izin verin.

UNIX/Linux dosya sistemlerinde yazma izninin çok güçlü bir izin olduğunu unutmayın, çünkü bu izin silme yetkisini de kapsıyor ve bir dosyanın silinip yeniden oluşturularak tamamen değiştirilmesine sebep olabilir. /tmp ve /var/tmp dizinleri üzerinde ayarlanması gereken yapışkan bit (sticky bit) nedeniyle bu etkiden ön tanımlı olarak güvenlidir.

Bunlara ek olarak, gizli veriler bölümünde anlatıldığı gibi dosya izinleri sürüm kontrol sistemlerinde korunmayabilir. Başlangıçta siz düzgün bir şekilde ayarlamış olsanız bile checkout/update gibi komutlar dosya izinlerini değiştirebilirler. Bunun için en güzel çözümlerden biri de bir Makefile, bir betik, sürüm kontrol sisteminde bir kanca işlevi ya da bunlara benzer bir yapı ile her zaman dosya izinlerini olması gereken değerlere güncellemektir.

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

Ana `/status` sayfası bir sonraki bölümde açıklandığı gibi en azından sistemin genel durumunu vermelidir. Bu, ana `/status` sayfasının TÜM alt sistem kontrollerini yürütmesi ve bütün sistem durumunu bildirmesi gerektiği anlamına gelir. Genel sistem durumunun alt sistemlere göre nasıl belirlendiğine karar vermek geliştiricilere kalmıştır. Örneğin, bazı kritik olmayan alt sistemlerin `HATA` durumu genel sistem için yalnızca bir `UYARI` durumu oluşturabilir.

Performans endişelerinden dolayı, bazı alt sistem kontrolelri genel `/status` sayfasında gözardı edilebilir, örneğin kontrol sırasında sistem kaynakları çok fazla kullanılıyorsa ya da kontrolün yapılması uzun zaman alıyorsa. Bir bütün olarak düşündüğümüzde, ana `/status` sayfası çok hızlı ve sade olmalı ki çok sık çağrılması durumda (1-3 dakikada bir) sistemde her hangi bir yüke sebep olmamalı. Gözardı edilen alt sistem kontrollerinin mutlaka yukarıda bahsedildiği gibi kendine özel sayfaları olmalı. Doğal olarak bu sayfaların takip edilmesi için harici takip sistemleri gerektiği gibi ayarlanmalı. Bu durumu başka bir şekilde çözmek için, şöyle bir yaklaşım ile çözüm uygulanabilir: arkaplanda çalışan bir işlem belli bir aralıkla bu yorucu alt sistem kontrolünü yapar ve sonu kaydeder. Bu ana durum sayfanızda bu tür yüklü kontrollerin sonucunu hızlıca göstermenizi sağlar (örneğin, yapılan son kontrolün sonucunu). Bu yaklaşım eğer uygulanması çok zor değilse mutlaka uygulanmalıdır.

## Status sayfası formatı

İki çeşit format öneriyoruz - `düz` and `JSON`.

### Düz format

Düz formatta her satır `anahtar: değer` şeklinde bir durumu  belirtir. Anahtar alt sistem ya da kontrol ismi ve değer de durumu temsil eder. Durum değerleri şunlardan biri olabilir:

* `OK`
* `WARN Mesaj`
* `ERROR Mesaj`

Burada ki `Mesaj` metinleri durumun ne olduğunu anlatabilecek anlamlı kısa metinler olmalı. Mesaj metinleri tek satır olmalı, herhangi bir uzunluk kısıtlamasına tabi olmamalı ama mantıklı bir uzunluk olması tercih edilmelidir. Örneğin, 200 karakterden uzun olmamalı denebilir.

Ana durum sayfası `status` adlı değerinde bütün sistemin durumunu belirten bir anahtar bulundurmalı. Bunun dışında alt sistemler için olan satırlar zorunlu değildir. Eğer kullanılacaksa bu satırların anahtarında `_status` ekleri olmalı:

Herşey düzgün çalışıyorsa:

```
status: OK
database_status: OK
elastic_search_status: OK
```

Sorunlu bir kontrol varsa:

```
status: ERROR Database is not accessible
database_status: ERROR Connection failed
elastic_search_status: OK
```

Birden fazla sorunlu kontrol varsa:

```
status: ERROR failed subsystems: database, elasticsearch. For details see https://myapp.example.com/status
database_status: ERROR Connection failed
elastic_search_status: WARN Too few entries in index A.
```

Durum satırlarının yanısıra, durum belirtmeyen satırlar da olabilir. Örneğin bazı ölçümlerin sonuçları olabilir (takip edilsin ya da  edilmesin). Bu ek değerlerin anahları da alt sistemin adını ön ek olarak almalıdır.

```
status: OK
database_status: OK
database_customers: 378
database_items: 8934748
elastic_search_status: OK
elastic_search_shards: 20
```

Genel sistem durumu da bazen bu ölçüm değerlerine bağlı olabilir:

```
status: WARN Too few items in database
database_status: WARN Too few customers in database
database_customers: 378
database_items: 1
elastic_search_status: OK
elastic_search_shards: 20
```

Alt sistem durum sayfaları da kendi adreslerine (`/status/subsystemX`) sahip olmalılar ve aynı formatı uygulamalılar; zorunlu `status` anahtarını bulundurmalılar ve gerekiyorda başka anahları da bulundurabilirler. Örneğin `/status/database`:

```
status: OK
connection_pool: 30
latency: 2
```

### JSON format

Durum sayfalarında bazen JSON formatı da tercih edilebilir, örneğin kullanılan harici sistemlerin bu format üzerinde çalışması daha kolay ise.

Durum değerleri aynı yukarıdaki gibi olmalıdır - `OK`, `WARN Mesaj` ve `ERROR Mesaj`.

Düz formatta olduğu gibi `status` anahtarı JSON nesnesinin ana elemanının hemen altında olmalı. Alt sistemler ise zorunlu `status` anahtarına sahip iç nesleler olarak ana nesnenin içine gömülmelidir. Aşağıdaki örnekleri inceleyebilirsiniz:

Herşey sorunsuz ise ise:

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

Bazı ek ölçülerle:

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

Sorunlu bir kontrol var:

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

## HTTP durum kodları

Eğer uygulama sağlıklı ve ayakta ise, durum sayfasının cevabının HTTP kodu 200 (OK) OLMALIDIR. Bunun dışındaki durumlarda 5XX HTTP kodları DÖNÜLMELİDİR. Örneğin, 500 (Internal Server Error - Sunucu hatası) kodu kullanılabilir. İsteğe bağlı olarak, kritik olmayan WARN durumlarında HTTP 200 kodu kullanılabilir.

## Yük dengeleyici kontrolleri

Bzen uygulamalar yük dengeleyicelerin arkasında çalışır. Yük dengeleyiciler arkalarındaki uygulamaları basitce bir URL çağırarak kontrol ederler. Bu kotroller dengeleyicilerin bir uygulama sunucusunda sorun olması durumunda o sunucuya trafik göndermemelerini sağlar.

Uygulamamızın genel `/status` sayfası dengeleyicilerin kontrol URL'si olarak kullanması için oldukça uygundur. Bunun yanında sadece dengeleyicilere özel bir durum sayfası yapmak da fayda sağlayabilir. Bu sayfa denegeleyicinin bakışına göre durumu ifadelendirerek şekillendirilebilir. Örneğin, genel durum için oldukça kötü olarak değerlendirilen bir hata, denegeleyici için aynı mantıkla değerlendirilemeyebilir ve sunucunun dengeleyici havuzundan çıkmaması sağlanabilir bu sayede. Buna güzel bir örnek de harici servislerin hata vermesi durumudur ve bu durumda sunucu kötü durumda olarak değerlendirilip havuzdan çıkarılamaz.

Denegeleyici için hazırladığınız sayfanın URL'si `/status/health` olabilir. Kullandığınız dengeleyici çözümüne göre sayfanın formatı burada bahsettiğimiz formatlardan farklı olabilir. Örneğin bazı yük dengeleyiciler sadece HTTP kodlarına bakarlar.

## Erişim kısıtlaması

Durum sayfaları eğer hassas hata ayıklama ya da uygulama bilgileri veriyorlarsa uygun seviyede erişim yetkilendirilmesi ile korunmalıdırlar. "HTTP basic authentication" ya da IP tabanlı kısıtlama bunun için uygundur.

# Kontrol listesi

Önemli işleri unutmamak için kullanabileceğiniz bazı kontrol listelerini sizin için derledik.

## Sorumluluk kontrol listesi

Özellikle birden fazla ekibin yer aldığı büyük projelerde her grubun ve sorumluğun takibini yapmak önemlidir. Aşağıdaki tablo bir web sayfasının canlıya çıkarılması sırasında kullanıbilecek bir kontrol listesini gösteriyor:

| Grup      | Görev                             | Sorumlu kişi / takım       | Bitiş tarihi | Durum             |
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

## Yayın kontrol listesi

Eğer kodunuzun yeni bir sürümünü yayınlamaya hazırsanız, yayın kontrol listenizdeki herşeyi tamamlamayı unutmayın! Ortaya çıkan huzur, tekrarlanabilirlik ve güvenilirlik büyük bir nimettir.

Sizin *zaten* bir listeniz vardır, değil mi? Eğer yoksa, aşağıdaki sizin için güzel bir başlangıç olabilir:

* [ ] Sürüm yükleme işlemi ortam ne olursa olsun çalışıyor
* [ ] Bütün ortamların iyi tanımlanmış isimleri var ve yaptıkları işlerle alakalı
* [ ] Bütün ortamlar aynı teknoloji yığınına sahip
* [ ] Bütün ortam ayarları sürüm kontrol sistemlerinde olmalı (web sunucu ayarları, CI betikleri vs.)
* [ ] Ürün, kullanılacağı yerdeki ağlardan test edilmiştir (örneğin genel İnternet, müşteri LAN).
* [ ] Ürün, hedeflenen tüm cihazlarla test edildi
* [ ] Hangi ortamda hangi sürüm olduğunu anlamanın kolay bir yolu olmalı
* [ ] Bir sürüm şeması tanımlandı
* [ ] Ürünün herhangi bir sürümü, kod tabanının sabit bir durumuna göre kolayca eşleştirilebilir
* [ ] Herhangi bir önceki sürüme dönüş kolaydır
* [ ] Yedekleme sistemleri çalışıyor
* [ ] Herhangi bir yedekten ayağı kaldırma işlemi test edildi
* [ ] Hiçbir gizli bilgi sürüm kontrol sisteminde saklanmıyor
* [ ] Loglama aktif
* [ ] Loglara erişim ve araştırma yapmak için tanımlanmış bir süreç olmalı
* [ ] Loglar uygun olduğunda "exception" ve hata izlerini içermeli
* [ ] Hatalar logdaki hata izlerine eşlenebilir
* [ ] Sürüm notları (Release Note) yazıldı
* [ ] Sunucu ortamları güncel
* [ ] Sunucu ortamları güncelleme planı var
* [ ] Ürüne yük testi uygulandı
* [ ] Herhangi bir ortamı diğer bir ortama kopyalama işlemi var (Örneğin, canlı ortamı QA ortamına kopyalayıp canlıda olan hatayı oluşturma)
* [ ] Tekrar eden bütün sürüm işlemleri otomatize edilmiş

# Dikkat edilmesi gereken sorular

* Projenin beklenen/gerekli ömrü nedir?
* Proje tek seferlik mi yoksa sürekli gelişme olacak mı?
* Hizmet için sürüm döngüsü nedir?
* Hangi ortamlar (dev, test, staging, prod, ...) kurulacak?
* Canlı ortamın aksama süresi servisin değerini nasıl etkileyecek?
* Kullandığımız teknolojiler ne kadar olgun? Geriye dönük uyumlulukları bozan önemli değişiklikler beklenebilir mi?

# Faydalı olduğu kabul edilebilir araçlar

* [HTTPie](https://github.com/jakubroztocil/httpie) API'leri komut satırında test etmek için harika bir araçtır. Özel başlıklara ve çerezlere geçiş yapmak kolaydır ve hatta oturum desteğine sahiptir.
* [jq](http://stedolan.github.io/jq/) bir CLI JSON işlemcisidir. İsteğe bağlı olarak cURL'den (veya elbette HTTPie!) gelen JSON verilerine işlem yapın. API testi veya araştırması için bir başka harika araç.

# Lisans

[Futurice Oy](http://www.futurice.com)
Creative Commons Attribution 4.0 International (CC BY 4.0)
