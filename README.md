# Container Tabanlı Bulut Ortamında Netflix Veri Seti Kullanılarak Web Uygulaması Geliştirilmesi

## 1. Projenin Tanımı

Bu proje, bulut bilişim altyapısı üzerinde çalışan, container tabanlı bir web uygulamasının uçtan uca
geliştirilmesini ve dağıtımını konu almaktadır. Proje kapsamında AWS EC2 üzerinde Ubuntu Server
kullanılarak bir sanal makine oluşturulmuş, Docker teknolojisi ile web uygulaması ve veritabanı
servisleri birbirinden bağımsız container’lar halinde çalıştırılmıştır.

Uygulama, gerçek bir veri kaynağı olan Kaggle platformundan alınan bir veri seti ile çalışmakta ve her
kullanıcı isteğinde verileri işleyerek bir veritabanına kaydetmektedir. Böylece, hem bulut ortamında
uygulama dağıtımı hem de veri odaklı bir web servisinin nasıl geliştirileceği pratik olarak gösterilmiştir.

## 2. Projenin Amacı

Bu projenin temel amacı, teorik olarak öğrenilen bulut bilişim, sanallaştırma ve container
kavramlarının gerçek bir senaryo üzerinde uygulanmasını sağlamaktır. Proje ile birlikte;

Bulut ortamında sanal makine yönetimi,
Container tabanlı uygulama geliştirme,
Web uygulaması ile veritabanı entegrasyonu,
Güvenlik ve ağ yapılandırmaları

gibi konularda uygulamalı deneyim kazanılması hedeflenmiştir.

## 3. Projenin Gündelik Hayattaki Kullanım Alanı

Bu projede geliştirilen yapı, gerçek hayatta yaygın olarak kullanılan birçok sistemin temelini
oluşturmaktadır. Günlük hayatta kullanılan;

E-ticaret siteleri,
Film ve dizi platformları,
Haber siteleri,
Sosyal medya uygulamaları

gibi servisler benzer bir mimari ile çalışmaktadır.

Örneğin, bir kullanıcı bir film platformunu her yenilediğinde farklı içeriklerin önerilmesi, arka planda bu
içeriklerin bir veritabanından çekilmesi ve kullanıcı etkileşimlerinin kaydedilmesi bu projede uygulanan
mantıkla birebir örtüşmektedir.


### Proje Sürecinde İzlenen Adımlar

1. Proje için uygun bulut altyapısı belirlenmiş ve AWS üzerinde bir EC2 sanal makinesi
    oluşturulmuştur.
2. EC2 üzerinde Ubuntu Server işletim sistemi kurulmuş ve temel sistem yapılandırmaları
    gerçekleştirilmiştir.
3. Sunucuya güvenli erişim sağlamak amacıyla SSH anahtar tabanlı bağlantı yapılandırılmıştır.
4. Container tabanlı mimariyi kullanmak için Docker kurulmuş ve gerekli servislerin çalıştırılacağı
    ortam hazırlanmıştır. _(_ **_sudo apt install docker.io -y , sudo systemctl start docker_** _)_
5. Web uygulaması ile veritabanının birbiriyle iletişim kurabilmesi için özel bir Docker ağı
    oluşturulmuştur. **_(docker network create appnet)_**
6. MySQL veritabanı, Docker container olarak ayağa kaldırılmış ve uygulama için gerekli veritabanı
    ve kullanıcılar tanımlanmıştır. **_(docker run -d --name mysql-db --network appnet)_**
7. Flask tabanlı Python web uygulaması geliştirilmiş ve Docker container olarak çalıştırılmıştır.
    **_(docker run -d --name webapp --network appnet \_**
8. **_-p 80:5000 mywebapp)_**
9. Kaggle platformundan alınan gerçek bir veri seti uygulamaya entegre edilmiştir.
**_(import__kagglehub, path = kagglehub.dataset_download("shivamb/netflix-shows"))_**
10. Uygulama, her HTTP isteğinde veri setinden rastgele kayıtlar seçerek bu verileri MySQL
    veritabanına kaydetmektedir.
11. Veritabanına eklenen veriler SQL sorguları ile doğrulanmış ve uygulamanın doğru şekilde
    çalıştığı test edilmiştir. **_(INSERT INTO netflix_titles (title, country, release_year), VALUES_**
    **_('Example Movie', 'USA', 2023);)_**
12. AWS Security Group ve Linux firewall ayarları yapılarak uygulamanın dış dünyaya güvenli
    şekilde açılması sağlanmıştır. **_(sudo ufw allow 80 , sudo ufw allow 22)_**
13. Uygulama tarayıcı üzerinden test edilmiş ve sistemin uçtan uca sorunsuz çalıştığı
    gözlemlenmiştir. **_(curl [http://public_ipv4_address)](http://public_ipv4_address))_**


#### Karşılaşılan Problemler ve Çözümleri

Proje geliştirme sürecinde hem altyapı hem de uygulama katmanında çeşitli problemlerle
karşılaşılmıştır. Bu problemler, özellikle Python tabanlı web uygulaması ile MySQL veritabanı
arasındaki entegrasyon sürecinde daha belirgin hale gelmiştir.

İlk olarak, Python uygulamasının MySQL veritabanına bağlanması sırasında bağlantı hataları
yaşanmıştır. Bu durumun temel sebebi, Docker container’lar arasında ağ yapılandırmasının doğru
şekilde yapılmaması ve veritabanı servisinin uygulamadan önce tam olarak hazır olmamasıdır. Sorun,
container’lar için özel bir Docker ağı oluşturularak ve uygulamanın veritabanı bağlantısını deneme–
bekleme mantığıyla kurması sağlanarak çözülmüştür.

Bunun yanında, SQL tablolarının uygulama tarafından otomatik olarak oluşturulması ve veri ekleme
işlemleri sırasında veri tipleriyle ilgili uyumsuzluklar gözlemlenmiştir. Özellikle metin uzunlukları ve
tarih alanlarında hatalar alınmış, bu durum tablo şemasının yeniden düzenlenmesiyle giderilmiştir.

Ayrıca, her HTTP isteğinde veritabanına veri yazılması sırasında eş zamanlı erişim ve bağlantı
sürekliliği problemleri yaşanmıştır. Bu sorun, veritabanı bağlantılarının kontrollü şekilde açılıp
kapatılması ve SQL sorgularının sadeleştirilmesiyle çözülmüştür.

Son olarak, SSH bağlantılarının zaman zaman kopması ve erişim problemleri yaşanmıştır. Bu durum,
bulut ortamlarında IP adresi değişimi ve kaynak kısıtları nedeniyle ortaya çıkmış, instance yeniden
başlatma ve disk taşıma yöntemleriyle giderilmiştir.


## Öğrenilen Dersler

Bu proje, yalnızca bir web uygulaması geliştirme süreci değil, aynı zamanda bulut tabanlı sistemlerin
bütüncül olarak nasıl çalıştığını anlamaya yönelik önemli kazanımlar sağlamıştır.

Öncelikle, bulut ortamlarında bir uygulamanın çalışabilmesi için yalnızca kodun yeterli olmadığı, ağ,
güvenlik, depolama ve servis yönetimi gibi birçok bileşenin birlikte düşünülmesi gerektiği öğrenilmiştir.
AWS EC2 üzerinde yapılan çalışmalar, sanal makine yönetimi ve bulut kaynaklarının doğru
yapılandırılmasının uygulama kararlılığı açısından kritik olduğunu göstermiştir.

Docker teknolojisi sayesinde, uygulama ve veritabanı servislerinin birbirinden bağımsız olarak
çalıştırılabilmesi ve bu yapıların taşınabilir hale gelmesi önemli bir deneyim olmuştur. Container
mimarisi, uygulama dağıtım süreçlerini kolaylaştırmış ve sistemin tekrar kurulabilirliğini artırmıştır.

Python ve SQL entegrasyonu sürecinde, uygulama katmanı ile veritabanı katmanı arasındaki
iletişimin dikkatli şekilde tasarlanması gerektiği görülmüştür. Hatalı sorguların veya yanlış bağlantı
yönetiminin uygulamanın tamamını etkileyebileceği anlaşılmıştır.

Ayrıca, gerçek bir veri setiyle çalışmanın uygulamayı daha anlamlı hale getirdiği ve teorik bilgilerin
pratikte karşılığını görmeyi sağladığı gözlemlenmiştir.


##### Olası İyileştirmeler

```
Bu proje, mevcut haliyle temel bir bulut tabanlı web uygulaması mimarisini başarıyla ortaya
koymaktadır. Ancak sistemin daha ileri seviyelere taşınabilmesi için çeşitli iyileştirme alanları
bulunmaktadır.
```
İlerleyen çalışmalarda Docker Compose kullanılarak tüm servislerin tek bir yapı dosyası üzerinden
yönetilmesi sağlanabilir. Bu sayede kurulum süreci daha hızlı ve hatasız hale getirilebilir. Ayrıca,
MySQL veritabanı AWS RDS gibi yönetilen bir servis üzerine taşınarak bakım ve yedekleme süreçleri
kolaylaştırılabilir.

```
Bununla birlikte, Elastic IP ve Load Balancer gibi AWS servisleri kullanılarak uygulamanın
erişilebilirliği ve sürekliliği artırılabilir. Veri setlerinin ve uygulama çıktılarının AWS S3 üzerinde
yedeklenmesi de sistemin güvenilirliğini yükseltecek bir diğer iyileştirme olarak değerlendirilebilir.
```
###### Sonuç

```
Sonuç olarak bu proje, modern yazılım sistemlerinde yaygın olarak kullanılan bulut altyapısı,
container mimarisi ve veritabanı entegrasyonunu gerçek bir senaryo üzerinden ele almıştır. Proje
sürecinde karşılaşılan problemler ve geliştirilen çözümler, bulut tabanlı uygulama geliştirme
konusunda önemli bir öğrenme deneyimi sağlamıştır. Bu yönüyle çalışma, hem akademik hem de
sektörel açıdan uygulanabilir bir örnek niteliği taşımaktadır.
```

