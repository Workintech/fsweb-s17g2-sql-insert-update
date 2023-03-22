# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

# Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

    1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.

    	CEVAP:	insert into yazar (yazarad,yazarsoyad)
    			values ("Kemal","Uyumaz");

    2) Biyografi türünü tür tablosuna ekleyiniz.

    	CEVAP:  insert into tur (turadi)
    			values ("Biyografi");

    3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin.

    	CEVAP:  insert into ogrenci (sinif,ograd,ogrsoyad,cinsiyet)
    			values ("10A","Çağlar","Üzümcü","E"),
    			("9B","Leyla","Alagöz","K"),
    			("11C","Ayşe","Bektaş","K");

    4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.

      	CEVAP:  insert into yazar (yazarad,yazarsoyad)
                (select ograd,ogrsoyad from ogrenci order by rand() limit 1);

    5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.

    	CEVAP:  insert into yazar (yazarad,yazarsoyad)
                (select ograd,ogrsoyad from ogrenci
                where ogrno between 10 and 30);

    6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
    (Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)

    	CEVAP:  insert into yazar (yazarad,yazarsoyad) values ("Nurettin","Belek");
    			select @@IDENTITY

    7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.

    	CEVAP:  update ogrenci set sinif="10C" where ogrno=3;

    8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın

    	CEVAP:  SET SQL_SAFE_UPDATES = 0;
    			update ogrenci set sinif= "10A" where sinif= "9A" ;
    			SET SQL_SAFE_UPDATES = 1;

    9) Tüm öğrencilerin puanını 5 puan arttırın.

    	CEVAP:  update ogrenci set puan=puan + 5 where ogrno!="cvb" ;

    10) 25 numaralı yazarı silin.

    	CEVAP:  delete from yazar where yazarno=25 ;

    11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)

    	CEVAP:  select * from ogrenci where dtarih is null;

    12) Doğum tarihi null olan öğrencileri silin.

    	CEVAP:  SET SQL_SAFE_UPDATES = 0;
    			delete from ogrenci where dtarih is null ;
    			SET SQL_SAFE_UPDATES = 1;

    13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.

    	CEVAP:  SET SQL_SAFE_UPDATES = 0;
    			update kitap set puan = puan + 2 where kitapadi like "a%";
    			SET SQL_SAFE_UPDATES = 1;

    14) Kişisel Gelişim isimli bir tür oluşturun.

    	CEVAP:  insert into tur (turadi) values ("Kişisel Gelişim");

    15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.

    	CEVAP: 	SET SQL_SAFE_UPDATES = 0;
    			update kitap set turno=(select turno from tur where turadi="Kişisel Gelişim")
    			where kitapno=45;
    			SET SQL_SAFE_UPDATES = 1;

    16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.

    	CEVAP:  CREATE DEFINER=`root`@`localhost` PROCEDURE `ogrencilistesi`()
    			BEGIN
    			select * from ogrenci;
    			END

    17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.

    	CEVAP:  CREATE DEFINER=`root`@`localhost` PROCEDURE `ekle`(in
    			ograd varchar(50),
    			ogrsoyad varchar(50),
    			sinif varchar(20),
    			dtarih date,
    			cinsiyet varchar(10),
    			puan varchar(10))
    			BEGIN
    			insert into ogrenci (ograd,ogrsoyad,sinif,dtarih,cinsiyet,puan)
    			values (ograd,ogrsoyad,sinif,dtarih,cinsiyet,puan);
    			END

    18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.

    	CEVAP:  CREATE DEFINER=`root`@`localhost` PROCEDURE `sil`(in ono int)
    			BEGIN
    			delete from ogrenci where ogrno=ono;
    			END

    19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.

    	CEVAP:  CREATE DEFINER=`root`@`localhost` PROCEDURE `sınıfdegistir`(in o_no int, o_sinif varchar(25))
    			BEGIN
    			update ogrenci set sinif=o_sinif where ogrno=o_no;
    			END

    20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.

    	CEVAP:  CREATE DEFINER=`root`@`localhost` PROCEDURE `searchad`(in search varchar(25))
    			BEGIN
    			select * from ogrenci
    			where concat(ograd," ",ogrsoyad) like concat("%",search,"%") ;
    			END

    21) Daha önceden oluşturduğunu tüm prosedürleri silin.


    #Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
    22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.

    	CEVAP:  select tur.turadi,kitap.kitapadi from kitap,tur
    			where kitap.turno=tur.turno
    			and tur.turadi=(select tur.turadi from tur where tur.turadi like "%Dram%")

    23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.

    	CEVAP:  select yazar.*,kitapadi from kitap,yazar
    			where  yazar.yazarno in (select yazarno from yazar where yazarad like "e%")

    24) Kitap okumayan öğrencileri listeleyiniz.



    25) Okunmayan kitapları listeleyiniz



    26) Mayıs ayında okunmayan kitapları listeleyiniz.
