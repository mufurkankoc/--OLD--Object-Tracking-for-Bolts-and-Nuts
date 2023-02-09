# StromaVision
ÇALIŞMA PRENSİPLERİM

•	Tedbirli yaklaşım. En iyi çözüm, sorunları öncesinden görüp hazırlanmak, ve sorunlarla mümkün olduğunca hiç karşılaşmamaktır
•	Donanım, sistemler, kodlama gibi kullanılması gereken her objeyi beraberinde paralel çalışan bir obje daha bulundurmalı. Sorun karşısında alternatifsiz kalınmamalı
•	Üzerinde çalışılacak projenin benzerleri veya yapılmış versiyonları baştan sona incelenmeli. Bu bağlamda projelerin ortak ve ayrı yönleri saptanmalı. Hangi donanım üzerinde uyguluyor ? elimde mevcut mu, ulaşabilir miyim ? Framework’ler  üzerinde herhangi bir lisans sorunu var mı ? modüllerin datasheet’leri mevcut mu ?diğer geliştiriciler tarafından yaygın olarak kullanılıyor mu ?bilgilerine ulaşım kolay mı ? gibi sorular sorulup cevapları analiz edilmeli.
•	İsterler karşılandığı sürece daha kolay veya bildiğin yolu tercih et fakat 2. Bir yolu mutlaka bil. Zor olanı tercih ediyor olmak iyi bir mühendis olduğunu göstermez, fakat zor yolları da alternatif olarak bulundurmak ve onunla çözüm üretebilecek bir teknik altyapıya sahip olmak iyi bir mühendis reaksiyonudur.
•	Projenin zamansal planlamasını iyi yap. Çevresel faktörlerin zamanda sapma yapmaması için tedbirler al.
•	Planlamayı yaparken ilk başta sadece projenin isterlerini kusursuz karşılayacak şekilde yap. Vakit kalır ise, projeye yarar sağlayabileceğini düşündüğün fikirlerini ekleyebilirsin. Diğer durumda basit gibi düşünülen ekstralar, talihsiz sürprizler yaşatabilir ve planlanan sürede yetişmeyebilir.
•	Her aşamada en ufak uygulamayı bile test et, doğruluğunu teyit et ve sonraki adıma geç.

STRATEJİ
•	Projeyi açtım, dosyaları indirdim, elimde olanları ve istenilenleri anlayıp gerekenleri analiz ettim.
•	Tüm deep learning algoritmalarını ve kullandıkları annotations formatlarını inceledim.
•	Bunlar arasından üzerinde çalışmam gerektiğini düşündüğüm ve ona alternatif olması gerekn algoritmalara karar verdim. 1) Yolo V8  2)SSD MobileNet V2
•	1 adet Windows ve 1 adetr Linux işletim sistemine sahip bilgisayarı hazır hale getirdim.
•	Yapılmış Yolo V8 ve SSD Mobilenet V2 projelerini inceledim ve başlamak için hazır oldum.
•	Projeyi 5 ana aşamaya böldüm ve sırasıyla uyguladım.
o	Datasetleri ve annotation’ları hazırlama
o	Dataseti eğitme ve fine-tuning
o	Yazılım üzerinde görüntüleme ve gereken algoritmaları oluşturma
o	Donanım veya çalışma ortamlarını etkin hale getirme
o	Sonuca bağlama

DATASET HAZIRLAMA
•	Video üzerinden eğitim yapmak yerine imajlar üzerinde eğitim yapmamın daha sağlıklı olacağına karar verdim ve videoyu frame’lere ayırıp PC’ye dosya olarak kaydeden bir yazılım geliştirdim.
•	Annotation’ları incelediğimde imajların 1’den başlayacak ve .jpg uzantısıyla isimlendirilecek şekilde olduğunu gördüm. Ve her sayı 4 digit halinde kaydedilmesi gerekiyordu. (1.jpg F  0001.jpg T) Geliştirdiğim yazılıma gerekli eklentiyi yaptım
•	Çıktısını aldığım imajları ve isimlendirmeleri Annotations üzerinde karşılaştırıdım.  Annotations üzerindeki son imaj 1800. Jpg isimliydi fakat benim ürettiğim son imaj 1799 .jpg isimliydi. Dögüdeki İsim_Sayaci +=1kodu son görselde hata vermiş olduğunu farkettim. Fakat videonun son 2 frame’i aynıydı ve obje içermiyordu. O yüzden 1799. Görseli kopyala yapıştır yapıp 1800.jpg ismini verdim.
•	Annotation format değişikliği için roboflow.com sitesini tercih ettim. Yolo V8 için txt ve SSD MobileNet için CSV,tfrecord dosyaları export ettim.
•	Format değişikliği sonrası annotation üzerindeki adresleme ile imaj üzerinde görünen bbox’ları eşleme yapıp doğruladım. Herhangi bir kayma veya hata oluşmamış.

TRAINING
•	Öncelikle küçük ölçekli dataset ile çalışmaya ve proje tamamen sonuçlandıktan sonra büyük datayla eğitim yapıp içine gömmeye karar verdim. Böylece hataları analiz şansım ve zaman kazanma şansım olacaktı. Test dosyasındaki 1800 imaj ile standart Yolo V8 eğitimini gerçekleştirdim.
•	Projenin tüm isterlerini karşıladıktan ve sonuç aldıktan sonra Train datasetiyle tekrar eğitim yazılıma gömdüm.
•	Sadece test dosyasındaki verilerin bile çok iyi loss, mAP, precision, confidence sonuçları almamdan dolayı ve 2 bilgisayarında 12 saatlik Colab kullanım hakkını doldurmak istemememden dolayı epoch değerini 25 tutup 2,5 saatlik eğitim yaptım.
•	Bu eğitim ile değiştirmeyi uygun gördüğüm değerler;
o	%70 Train %20 Val %10 Test
o	Annotation olarak imajların ayna görüntüleri de eklendi.
o	Epoch=25 
o	İmgsz = 640

KODLAMA
•	5 madde üzerinden çalışma sağlandı.
o	Modeli okuma
o	Videoyu okuma ve frame’leri döngüye alma
o	Inference oluşturma
o	Frame’leri, listeleri ve değişkenleri format değişikliği yaparak inference kodları ile manipüle edilebilir forma kavuşturma.
o	Bbox çizme, sayma, takip etme gibi algoritmaları oluşturma
•	Yolo modeli okuması ve çalıştırabilmesi için “torch” ve “ultralytics” kütüphaneleri eklendi.
•	Inference işlemleri için Yolo V8 ile uyumlu çalışan “supervision” kütüphanesi tercih edildi.
•	Hızlı numerik işlemleri ve format değişikliği için “numpy” tercih edildi.
•	Video okuma, döngüler ve kalan görüntü işleme işlemleri için “cv2” kütüphanesi tercih edildi.

DONANIM VE ÇALIŞMA ORTAMI
•	1. Planda Linux işletim sistemi üzerinde OOP mimarisiyle yazılım hazırlandı.
•	Model okuma, video okuma ve döngüye alma, inference oluşturma ve manipüle etme, bbox çizme ve obje sayma başarıyla çalıştı.
•	Fakat takip algoritması sistemde başarılı şekilde çalışmadı. Bu işlem için ByteTracker ve DeepSort kütüphanelerini uygun buldum. Yükleme sırasında ByteTracker kütüphanesi hatalar getirdi. En son karşılaştığım hata ise requirement’lar içerisindeki Onnx kütüphaneleri oldu. Farklı versiyonlarını denedim, pip ve source ile indirmeyi denedim fakat sonuç alamadım. Pc üzerindeki Cmake’i güncellememle sorunun çözüleceğini fark ettim. Fakat bu işlem sürpriz sonuçlar getirebilir kaygısı ile çalışmaya Colab üzerinde devam etmeye karar verdim.
•	Colab son günlerde yoğun kullanıldığı için 12 saatlik kullanım süresi doldu. Diğer mail adresim ve Windows pc üzerinden devam ederek sonuca ulaştım.
•	ByteTracker üzerinden yaptığım değer değişimleri
o	track_thresh: float = 0.7


SONUÇ
•	Yaptığım çalışma teknik isterleri karşılamakta ve sonuç vermektedir.
•	StromaVision.ipynb dosyasını Colab üzerinde açmanız ve tümünü çalıştır butonunu seçmeniz yeterli olacaktır.
•	Stromavision.pt eğitim dosyasını ve test.mp4 dosyasını drive üzerinden otomatik çekmektedir ve output.mp4 isminde video çıktısı vermektedir.




