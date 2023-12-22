# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]


##### Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 


MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
	#ilk 3 soruyu join kullanmadan yazın.
	1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	select o.ograd,o.ogrsoyad,i.atarih from ogrenci as o, islem as i
    where o.ogrno = i.ogrno
    order by o.ograd,o.ogrsoyad
	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	select k.kitapadi,t.turadi from kitap as k ,tur as t
    where k.turno = t.turno
    and t.turadi IN ("Fıkra","Hikaye")
    order by t.turadi.k.kitapadi;
	
	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
	SELECT o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi,o.sinif
    FROM ogrenci as o,kitap as k,islem as i
    WHERE o.ogrno= i.ogrno
    AND i.kitapno = k.kitapno
    AND o.sinif IN ("10B","10C")
    ORDER BY o.ograd;
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	SELECT o.ogrno,o.ograd,o.ogrsoyad,i.atarih
    FROM ogrenci as o
    INNER JOIN islem as i
    ON o.ogrno = i.ogrno
    ORDER BY o.ograd;
	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	SELECT k.kitapadi,t.turadi 
    FROM kitap as k ,tur as t
    WHERE k.turno = t.turno
    AND t.turadi IN ("Fıkra","Hikaye")
    ORDER BY t.turadi.k.kitapadi;
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	SELECT o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi,o.sinif
    FROM ogrenci as o,kitap as k,islem as i
    WHERE o.ogrno= i.ogrno
    AND i.kitapno = k.kitapno
    AND o.sinif IN ("10B","10C")
    ORDER BY o.ograd;
	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
	select o.ograd, o.ogrsoyad,i.atarih
    from ogrenci as o
    LEFT JOIN islem as i
    On o.ogrno = i.ogrno
    order by i.atarih = asc;
	
	8) Kitap almayan öğrencileri listeleyin.
    select o.ograd,o.ogrsoyad,i.atarih
    from ogrenci as o
    LEFT JOIN islem as i
    On o.ogrno = i.ogrno
    where i.ogrno is null;
    order by i.atarih asc;
	
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
	select i.kitapno,k.kitapadi,count(i.kitapno) as TOTAL
    from islem as i
    Inner JOIN islem as k
    On i.kitapno = k.kitapno
    group by i.kitapno,k.kitapno;
    order by k.kitapno asc;
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.
    select i.kitapno,k.kitapadi,count(i.kitapno) as TOTAL
    from kitap as k
    left join islem as i
    On i.kitapno = k.kitapno
    group by i.kitapno,k.kitapadi
    order by total asc;

	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	SELECT o.ograd,o.ogrsoyad,k.kitapadi
    FROM ogrenci as o
    INNER JOIN islem as i
    ON o.ogrno = i.ogrno
    INNER JOIN kitap k
    ON k.kitapno = i.kitapno
    ORDER BY o.ograd
  
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
	select o.ograd,o.ogrsoyad,k.kitapadi,t.turadi,y.yazarad,i.atarih
    FROM ogrenci as o
    LEFT JOIN islem as i
    ON o.ogrno = i.ogrno
    LEFT JOIN kitap k
    ON i.kitapno = k.kitapno
    LEFT JOIN  yazar y 
    ON y.yazarno = k.yazarno
    LEFT JOIN tur t
    ON t.turno = k.turno
    ORDER BY k.kitapadi desc;
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
	SELECT o.ograd,o.ogrsoyad,o.sinif,count(i.ogrno) as TOTAL_READ
    FROM ogrenci o
    INNER JOIN islem as i
    ON o.ogrno = i.ogrno
    WHERE o.sinif in ("10A","10B")
    GROUP BY i.ogrno
    ORDER BY TOTAL_READ desc;
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG
    SELECT AVG(k.sayfasayisi)
    FROM kitap as k;
	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
	SELECT k.kitapadi,k.sayfasayisi
    FROM kitap as k
    WHERE k.sayfasayisi > (SELECT AVG(k.sayfasayisi)
    FROM kitap as k)
    ORDER BY k.sayfasayisi desc;
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin
	SELECT count(ogrno) FROM ogrenci;
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
    SELECT count(ogrno) as toplam FROM ogrenci;

	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	SELECT count(distinct ograd) as toplam FROM ogrenci;
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	SELECT * FROM kitap 
    ORDER BY sayfasayisi desc;
    LIMIT 1
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	SELECT k.kitapadi,MAX(k.sayfasayisi) 
    FROM kitap k 
    GROUP BY k.kitapadi;
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	SELECT MIN(k.sayfasayisi) from kitap k;
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	SELECT k.kitapadi,MIN(k.sayfasayisi) FROM kitap k group by k.kitapadi;
		
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
	SELECT MAX(k.sayfasayisi) 
    FROM kitap k
    INNER JOIN tur t
    ON t.turno = k.turno
    WHERE t.turadi = "Dram";
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
	SELECT o.ograd, k.kitapadi 
    FROM ogrenci o
    INNER JOIN islem i
    ON o.ogrno = i.ogrno
    INNER JOIN kitap k
    ON i.kitapno = k.kitapno
    WHERE o.ogrno = 15;
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )
	SELECT o.ograd,count(o.ogrno) 
    FROM ogrenci o
    GROUP BY o.ograd;
   
	26) Her sınıftaki öğrenci sayısını bulunuz.
    SELECT o.sinif,count(o.ogrno) 
    FROM ogrenci o
    GROUP BY o.sinif;
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	SELECT o.cinsiyet,count(o.ogrno)
    FROM ogrenci o
    GROUP BY o.cinsiyet;
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	SELECT o.ograd,o.ogrsoyad,
    SUM(k.sayfasayisi) as okudugusayfasayisi
    FROM ogrenci o
    INNER JOIN islem i
    ON o.ogrno = i.ogrno
    INNER JOIN kitap k
    ON k.kitapno = i.kitapno
    GROUP BY o.ograd,o.ogrsoyad
    ORDER BY okudugusayfasayisi desc;
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.
	SELECT o.ograd,o.ogrsoyad,count(i.islemno) as okudugukitapsayisi
    FROM ogrenci o
    INNER JOIN islem i
    ON o.ogrno = i.ogrno
    GROUP BY o.ograd,o.ogrsoyad;
    