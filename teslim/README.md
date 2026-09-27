**Mustafa Ocak**

# Teslim dosyaları ve notebook'u çalıştırma

## Notebook ne yapıyor?

[`kod/veri_analizi.ipynb`](kod/veri_analizi.ipynb), `kod/veri/` içindeki müşteri, araç, servis ve geri bildirim CSV dosyalarını okur. Verileri birleştirip kontrol eder; churn hedefini ve müşteri özelliklerini oluşturur; keşifsel analiz, servis merkezi karşılaştırması ve Random Forest, XGBoost, Logistic Regression modellerinin değerlendirmesini yapar. Son hücrelerde model açıklamaları ve SHAP grafiği bulunur.

Notebook hücrelerini yukarıdan aşağıya sırayla çalıştırın. Çalışma dizini `teslim/kod/` olmalıdır; notebook içindeki `veri/` yolları bu dizine göredir.

## Kurulum

Python 3.13 ve `requirements.txt` içindeki paketler kullanılır. Proje kök dizininden:

```bash
cd teslim
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Notebook'u VS Code/Jupyter gibi bir notebook arayüzünde açın. Kernel olarak `teslim/.venv` Python yorumlayıcısını seçin. Gerekirse bu ortamı Jupyter kernel listesine eklemek için:

```bash
python -m ipykernel install --user --name aday-paketi-teslim --display-name "Python (aday-paketi-teslim)"
```

Notebook arayüzünde çalışma dizinini `teslim/kod/` olarak ayarlayın (bu klasörde `veri/` dizini görünmelidir), ardından **Run All / Tümünü Çalıştır** seçeneğini kullanın. `requirements.txt` Python analiz paketlerini ve `ipykernel`'i kurar; Jupyter arayüzü ayrıca sağlanmalıdır.

## Dosyalar ve üretilen çıktılar

- `RAPOR.md`: analiz raporu.
- `requirements.txt`: notebook'un Python bağımlılıkları.
- `kod/veri/`: notebook'un kullandığı kaynak CSV dosyaları.
- `kod/birlesik_veri.xlsx`: veri birleştirme hücresinin ürettiği çalışma kitabı.
- `kod/feature_dislanan_musteriler.csv`: özellik üretiminde dışlanan müşteri kayıtları.
- `ciktilar/`: notebook çalışırken oluşturulan servis merkezi tabloları ve grafikleri; klasörde rapora eklenen görseller de bulunur.

Notebook'u yeniden çalıştırmak mevcut çıktı dosyalarını güncelleyebilir. Kaynak CSV dosyaları `kod/veri/` altında tutulur.
