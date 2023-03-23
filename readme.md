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

    insert into yazar(yazarad,yazarsoyad)

values ("Kemal","Uymaz");

    2) Biyografi türünü tür tablosuna ekleyiniz.


    insert into tur(turadi)

values ("Biyografi");

3. 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin.

   INSERT INTO ogrenci (sinif, ograd, ogrsoyad,cinsiyet)
   VALUES
   ('10A', 'ÇAĞLAR','ÜZÜMCÜ','E'),
   ('9B', 'LEYLA', 'ALAGÖZ', 'K'),
   ('11C', 'AYŞE', 'BEKTAŞ', 'K');

4. Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.

   insert into yazar(yazarad,yazarsoyad) select ograd,ogrsoyad from ogrenci

order by rand()
limit 1

5. Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.

insert into yazar(yazarad,yazarsoyad)
SELECT ograd,ogrsoyad from ogrenci
WHERE ogrno BETWEEN 10 AND 30;

6. Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
   (Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)

7. 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.

update ogrenci
set sinif="10C"
WHERE ogrno=3;

8. 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın

update ogrenci
set sinif="10A"
WHERE sinif="9A" and ogrno!=0;

9. Tüm öğrencilerin puanını 5 puan arttırın.

   update ogrenci

set puan=puan+5
where ogrno !=0 //safe mode

10. 25 numaralı yazarı silin.

    delete from yazar where yazarno="25"

    11. Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)

select \* from ogrenci
where dtarih IS Null

    12) Doğum tarihi null olan öğrencileri silin.


    13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.

     update kitap

set puan=puan+2
where kitapno !=0 and kitapadi="a%"
//safe mode

    14) Kişisel Gelişim isimli bir tür oluşturun.


    15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.


    16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.

delimiter //
create procedure ogrencilistesi(in ogrno int)
begin
select \* from ogrenci;
end//
delimiter ;

    17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.

    delimiter //

create procedure ekle(in ograd varchar(20),in ogrsoyad varchar(20))
begin
insert into ogrenci(ograd,ogrsoyad)values (o_ograd,o_ogrsoyad);
end//
delimiter ;

    18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.

    select * from  ogrenci;

delimiter //
create procedure sil (in ono int )
begin
delete from ogrenci
where ogrno=ono;
end //
delimiter ;

call sil (1) 19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.

CREATE DEFINER=`root`@`localhost` PROCEDURE `sinifdegistir`(in o_no int,o_sinif varchar(25))
begin
update ogrenci set sinif=o_sinif where ogrno=o_no;
END

    20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.


    CREATE DEFINER=`root`@`localhost` PROCEDURE `searchad`(in search varchar(25))

BEGIN
select \* from ogrenci
where concat(ograd,ogrsoyad) like concat("%",search,"%");
END

// %,search,% içinde o kelime geçenleri listeliyor

21. Daha önceden oluşturduğunu tüm prosedürleri silin.
    #Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)

22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.

23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.

24) Kitap okumayan öğrencileri listeleyiniz.

25) Okunmayan kitapları listeleyiniz

26) Mayıs ayında okunmayan kitapları listeleyiniz.

start transaction;
update kitap set sayfasayisi=sayfasayisi+100
where kitapno=1;
