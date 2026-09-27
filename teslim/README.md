**Mustafa Ocak**

# Teslim dosyaları ve notebook'u çalıştırma

## Notebook ne yapıyor?

[`kod/veri_analizi.ipynb`](kod/veri_analizi.ipynb), `kod/veri/` içindeki müşteri, araç, servis ve geri bildirim CSV dosyalarını okur. Verileri birleştirip kontrol eder; churn hedefini ve müşteri özelliklerini oluşturur; keşifsel analiz, servis merkezi karşılaştırması ve Random Forest, XGBoost, Logistic Regression modellerinin değerlendirmesini yapar. Son hücrelerde model açıklamaları ve SHAP grafiği bulunur.

Notebook hücrelerini yukarıdan aşağıya sırayla çalıştırın. Çalışma dizini, `veri/` klasörünün bulunduğu `kod/` dizini olmalıdır. Aşağıdaki kurulum komutları teslim klasörünü otomatik bulur; komutları depo kökünden veya depo içindeki başka bir klasörden başlatabilirsiniz.

## Kurulum

Python 3.13 ve `requirements.txt` içindeki paketler kullanılır. Aşağıdaki blok, geçerli dizinden yukarı doğru arama yaparak `requirements.txt` ile `kod/veri_analizi.ipynb` dosyalarının bulunduğu teslim klasörünü bulur. Böylece `teslim/` başka bir depo klasörünün içinde olsa da ya da komutu depo içindeki farklı bir dizinden başlatsanız da yol elle sabitlenmez:

```bash
TESLIM_DIR="$(python3 - <<'PYCODE'
from pathlib import Path

start = Path.cwd().resolve()
for base in (start, *start.parents):
    for candidate in (base, base / "teslim"):
        if (candidate / "requirements.txt").is_file() and (candidate / "kod" / "veri_analizi.ipynb").is_file():
            print(candidate)
            raise SystemExit(0)
raise SystemExit("Teslim klasörü bulunamadı: requirements.txt ve kod/veri_analizi.ipynb aranıyor.")
PYCODE
)"
cd "$TESLIM_DIR"
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Notebook'u VS Code/Jupyter gibi bir notebook arayüzünde açın. Dosya yolu `$TESLIM_DIR/kod/veri_analizi.ipynb`, çalışma dizini ise `$TESLIM_DIR/kod/` olmalıdır. Kernel olarak `$TESLIM_DIR/.venv` Python yorumlayıcısını seçin. Gerekirse bu ortamı Jupyter kernel listesine eklemek için:

```bash
python -m ipykernel install --user --name aday-paketi-teslim --display-name "Python (aday-paketi-teslim)"
```

Notebook arayüzünde çalışma dizinini `$TESLIM_DIR/kod/` olarak ayarlayın (bu klasörde `veri/` dizini görünmelidir), ardından **Run All / Tümünü Çalıştır** seçeneğini kullanın. `requirements.txt` Python analiz paketlerini ve `ipykernel`'i kurar; Jupyter arayüzü ayrıca sağlanmalıdır.

## Dosyalar ve üretilen çıktılar

- `RAPOR.md`: analiz raporu.
- `requirements.txt`: notebook'un Python bağımlılıkları.
- `kod/veri/`: notebook'un kullandığı kaynak CSV dosyaları.
- `kod/birlesik_veri.xlsx`: veri birleştirme hücresinin ürettiği çalışma kitabı.
- `kod/feature_dislanan_musteriler.csv`: özellik üretiminde dışlanan müşteri kayıtları.
- `ciktilar/`: notebook çalışırken oluşturulan servis merkezi tabloları ve grafikleri; klasörde rapora eklenen görseller de bulunur.

Notebook'u yeniden çalıştırmak mevcut çıktı dosyalarını güncelleyebilir. Kaynak CSV dosyaları `kod/veri/` altında tutulur.
