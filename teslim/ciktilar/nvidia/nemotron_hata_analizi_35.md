# Nemotron: 35 Metinlik Hata Analizi

## Değerlendirme yöntemi ve sonuç

Kaydedilmiş Nemotron tahminleri, güncel `outputs/altin_set_35.json` etiketleriyle yeniden karşılaştırıldı. Aynı metin 200 örneklemde birden çok kez varsa ilk geçerli tahmin puanlamaya alındı; tekrar tahminleri ayrıca incelendi. Bu işlem yeni model/API çağrısı yapmadı.

| Ölçüm | Sonuç |
|---|---:|
| Duygu doğruluğu | 31/35 (**88.6%**) |
| Duygu macro F1 | **0.896** |
| Konu kümesinin tam eşleşmesi | 14/35 (**40.0%**) |
| Konu mikro kesinliği | **64.6%** |
| Konu mikro duyarlılığı | **63.3%** |
| Konu mikro F1 | **63.9%** |
| Duygu ve konuların birlikte tam eşleşmesi | 13/35 (**37.1%**) |

Altın etiketler tek değerlendirici tarafından hazırlandı; bu ölçüm değerlendiriciler arası uyumu göstermez. Altın setteki iki `iş kalitesi` yazımı, tanımlı taksonomiye uygun `iş_kalitesi` biçimine kanonikleştirildi.

## Hata özeti

- Altın etikete göre 22 kayıtta duygu veya konu kümesi en az bir noktada eşleşmedi.
- En sık fazla atanan konular: `fiyat` (9), `iş_kalitesi` (3), `yedek_parça` (2), `randevu` (1), `süre` (1), `temizlik` (1).
- En sık atlanan konular: `diğer` (10), `iş_kalitesi` (3), `süre` (2), `personel` (2), `randevu` (1).
- Tekrar tahminleri 7 altın metinde tutarsızdı.

## Eşleşmeyen kayıtlar

| # | Geri bildirim | Altın etiket | Nemotron tahmini | Fark |
|---:|---|---|---|---|
| 1 | Fiyat çok yüksek geldi, aynı işi özel serviste yarı fiyata yaptırabilirdim. | Olumsuz · fiyat, iş_kalitesi | Olumsuz · fiyat | Eksik konu: iş_kalitesi. |
| 3 | Servis temizdi ve ekip güler yüzlüydü, yalnızca yedek parça beklemesi can sıkıcıydı. | Karışık · personel, süre, temizlik | Karışık · personel, temizlik, yedek_parça | Eksik konu: süre. Fazla konu: yedek_parça. |
| 5 | İkinci kez geliyorum, yine sorunsuz. Ekibe teşekkürler. | Olumlu · iş_kalitesi, personel | Olumlu · personel | Eksik konu: iş_kalitesi. |
| 7 | Randevu saatinde gittim ama iki saat bekledim, kimse bilgi vermedi. | Olumsuz · personel, randevu, süre | Olumsuz · personel, süre | Eksik konu: randevu. |
| 8 | Yedek araç verilmesi büyük kolaylık oldu. | Olumlu · diğer | Olumlu · yedek_parça | Eksik konu: diğer. Fazla konu: yedek_parça. |
| 10 | Harika bir deneyimdi, sadece üç hafta bekledim. | Olumlu · süre | Olumlu · randevu | Eksik konu: süre. Fazla konu: randevu. |
| 12 | Kampanya mesajı geldi ama şubede geçerli değilmiş. | Olumsuz · diğer | Olumsuz · fiyat | Eksik konu: diğer. Fazla konu: fiyat. |
| 14 | Ödeme sırasında POS çalışmadı, nakit aramak zorunda kaldım. | Olumsuz · diğer | Olumsuz · fiyat, iş_kalitesi | Eksik konu: diğer. Fazla konu: fiyat, iş_kalitesi. |
| 16 | Aracımla ilgili değil ama web sitenizden randevu alınmıyor, sürekli hata veriyor. | Olumsuz · diğer, randevu | Olumsuz · randevu | Eksik konu: diğer. |
| 17 | Arıza giderilmemiş, aynı ses devam ediyor. Tekrar gitmek zorunda kaldım. | Olumsuz · iş_kalitesi | Olumsuz · iş_kalitesi, süre | Fazla konu: süre. |
| 19 | Aracımın içinde yağ lekesi bıraktılar, koltuk kirlenmişti. | Olumsuz · iş_kalitesi, temizlik | Olumsuz · temizlik | Eksik konu: iş_kalitesi. |
| 20 | Servis iyi de otopark yok, aracı sokağa bırakmak zorunda kaldım. | Karışık · diğer | Olumsuz · fiyat, temizlik | Duygu: Karışık yerine Olumsuz. Eksik konu: diğer. Fazla konu: fiyat, temizlik. |
| 21 | Hiç de memnun kalmadım denemez, işlerini biliyorlar. | Olumlu · iş_kalitesi | Olumsuz · iş_kalitesi | Duygu: Olumlu yerine Olumsuz. |
| 22 | Danışman bey her adımı tek tek anlattı, fiyat konusunda da şeffaftı. | Olumlu · personel | Olumlu · fiyat, personel | Fazla konu: fiyat. |
| 24 | Mükemmel değildi ama beklentimin altında da kalmadı. | Karışık · diğer | Karışık · fiyat | Eksik konu: diğer. Fazla konu: fiyat. |
| 26 | Kötü diyemem ama bir daha gelir miyim bilmiyorum. | Karışık · diğer | Olumsuz · fiyat | Duygu: Karışık yerine Olumsuz. Eksik konu: diğer. Fazla konu: fiyat. |
| 27 | İyi. | Olumlu · diğer | Olumlu · fiyat | Eksik konu: diğer. Fazla konu: fiyat. |
| 29 | Arıza çözüldü ancak randevu sistemi çok karışık, üç kez aramak zorunda kaldım. | Karışık · randevu | Karışık · iş_kalitesi, randevu | Fazla konu: iş_kalitesi. |
| 30 | Fena değildi. | Olumlu · diğer | Karışık · fiyat | Duygu: Olumlu yerine Karışık. Eksik konu: diğer. Fazla konu: fiyat. |
| 31 | Servis ekibi çok ilgiliydi, aracım söz verilen saatte hazırdı. Teşekkürler. | Olumlu · personel, süre | Olumlu · iş_kalitesi, süre | Eksik konu: personel. Fazla konu: iş_kalitesi. |
| 34 | Beklediğimden erken bitti, aradılar haber verdiler. | Olumlu · personel, süre | Olumlu · süre | Eksik konu: personel. |
| 35 | Bir yıldız bile fazla. | Olumsuz · diğer | Olumsuz · fiyat | Eksik konu: diğer. Fazla konu: fiyat. |

Makinece okunabilir tam ölçümler `nemotron_kalite_35.json` dosyasındadır.
