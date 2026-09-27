**Mustafa Ocak**

# Teslim dosyaları ve notebook'u çalıştırma

## Notebook ne yapıyor?

[`kod/veri_analizi.ipynb`](kod/veri_analizi.ipynb), `kod/veri/` içindeki müşteri, araç, servis ve geri bildirim CSV dosyalarını okur. Verileri birleştirip kontrol eder; churn hedefini ve müşteri özelliklerini oluşturur; keşifsel analiz, servis merkezi karşılaştırması ve Random Forest, XGBoost, Logistic Regression modellerinin değerlendirmesini yapar. Son hücrelerde model açıklamaları ve SHAP grafiği bulunur.

Notebook hücrelerini yukarıdan aşağıya sırayla çalıştırın. Çalışma dizini, `veri/` klasörünün bulunduğu `kod/` dizini olmalıdır. Aşağıdaki kurulum komutları deponun en dış klasörünü otomatik bulur. Sanal ortam (`.venv`) bu klasörde oluşturulur; böylece `teslim/` daha büyük proje klasörünün içindeyken de konumu doğru kalır.

## Kurulum

Python 3.13 ve `requirements.txt` içindeki paketler kullanılır. Aşağıdaki blok Git depo kökünü bulur; depo Git olmadan açılmışsa `teslim/requirements.txt` ile `teslim/kod/veri_analizi.ipynb` dosyalarını içeren dış klasörü arar. Sanal ortamı depo kökünde oluşturur ve bağımlılıkları teslim klasöründen kurar:

```bash
PROJECT_ROOT="$(python3 - <<'PYCODE'
from pathlib import Path
import subprocess

start = Path.cwd().resolve()
ancestors = (start, *start.parents)

def is_delivery(path):
    return (path / "requirements.txt").is_file() and (path / "kod" / "veri_analizi.ipynb").is_file()

# Git deposu varsa en dış dizin olarak onu kullan.
for base in ancestors:
    result = subprocess.run(
        ["git", "-C", str(base), "rev-parse", "--show-toplevel"],
        capture_output=True, text=True, check=False,
    )
    if result.returncode == 0:
        root = Path(result.stdout.strip()).resolve()
        if is_delivery(root / "teslim") or is_delivery(root):
            print(root)
            raise SystemExit(0)

# Git yoksa, teslim/ klasörünü barındıran en yakın dış proje klasörünü bul.
for base in ancestors:
    if is_delivery(base / "teslim"):
        print(base)
        raise SystemExit(0)
    if is_delivery(base):
        print(base.parent if base.name == "teslim" else base)
        raise SystemExit(0)

raise SystemExit("Proje kökü bulunamadı: teslim/requirements.txt ve teslim/kod/veri_analizi.ipynb aranıyor.")
PYCODE
)"
if [ -f "$PROJECT_ROOT/teslim/requirements.txt" ]; then
  TESLIM_DIR="$PROJECT_ROOT/teslim"
else
  TESLIM_DIR="$PROJECT_ROOT"
fi
python3 -m venv "$PROJECT_ROOT/.venv"
source "$PROJECT_ROOT/.venv/bin/activate"
python -m pip install --upgrade pip
python -m pip install -r "$TESLIM_DIR/requirements.txt"
```

Notebook'u VS Code/Jupyter gibi bir notebook arayüzünde açın. Dosya yolu `$TESLIM_DIR/kod/veri_analizi.ipynb`, çalışma dizini ise `$TESLIM_DIR/kod/` olmalıdır. Kernel olarak `$PROJECT_ROOT/.venv` Python yorumlayıcısını seçin. Gerekirse bu ortamı Jupyter kernel listesine eklemek için:

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
