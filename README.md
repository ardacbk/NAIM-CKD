# NAIM ile Eksik Klinik Verilerde CKD Sınıflandırması

[NAIM](https://doi.org/10.1016/j.aiopen.2026.02.005) (Not Another Imputation Method) modeli [UCI Chronic Kidney Disease](https://archive.ics.uci.edu/dataset/336/chronic+kidney+disease) verisine uygulanmış ve eksik değerleri medyan / en sık değerle doldurulan SVM, KNN ve Random Forest ile karşılaştırılmıştır.

## Dosyalar

| Dosya / klasör | İçerik |
| --- | --- |
| `naim_ckd.ipynb` | Bütün deneyler: veri analizi, sabit bölme, eksiklik maskeleri, 5 model, tanı deneyleri |
| `data/` | Veri dosyası `chronic_kidney_disease_full.arff` |
| `outputs/` | Defterin kaydettiği çıktılar: bölme, eksiklik maskeleri, tahminler, eğitim geçmişleri, sonuç tabloları |
| `requirements.txt` | Python bağımlılıkları |

## Bağımlılıklar

Deneyler Google Colab'da T4 GPU ile çalıştırılmıştır: Python 3.13, PyTorch 2.11, scikit-learn 1.6.1, pandas 2.2.3, numpy 2.1.3. Colab'da hepsi hazır gelir. Yerelde çalıştırmak için:

```bash
pip install -r requirements.txt
```

## NAIM kaynak kodu

Defter [cosbidev/NAIM](https://github.com/cosbidev/NAIM) reposunu commit `2f15af8`'e sabitleyerek indirir ve şu üç dosyayı değiştirmeden kullanır:

- `CMC_utils/models/tabular/naim/naim.py` ve `tabular_tokenizer.py`: model ve özellik gömme
- `CMC_utils/data_augmentation/tabular.py`: eğitimde maskeleme (`missing_augmentation`)
- `CMC_utils/regularizers/l1_l2_regularizers.py`: L1 / L2 cezaları

`CMC_utils` paketinin tamamı hydra, xgboost, pytorch-tabnet gibi eski sürümlere sabitlenmiş bağımlılıklar gerektirdiği için yalnızca bu dosyalar yüklenir.

## Çalıştırma (Google Colab)

1. Drive'ınızda `MyDrive/cinar-lab/data/` klasörünü oluşturup `data/chronic_kidney_disease_full.arff` dosyasını içine koyun. Dosya orada yoksa defter, çalışırken Colab'ın yükleme penceresini açar. Defter dosyanın MD5 değerini kontrol eder (`0c9f5965ed459e2f70002c278c39b03b`).
2. `naim_ckd.ipynb` dosyasını Colab'da açın ve **Çalışma zamanı → Çalışma zamanı türünü değiştir → T4 GPU** seçin.
3. **Çalışma zamanı → Tümünü çalıştır.** İlk hücrelerden biri Drive izni ister; izin verilirse çıktılar `MyDrive/cinar-lab/outputs/` altına, verilmezse geçici diske (`/content/cinar-lab/outputs/`) yazılır.
4. Bölme ve maskeler bir kez üretilip kaydedilir. Eğitim döngüleri her koşunun sonucunu biter bitmez kaydeder; bağlantı koparsa defter yeniden çalıştırıldığında bitmiş koşular atlanır ve eğitim kaldığı yerden devam eder.

T4 GPU'da baştan sona çalıştırma yaklaşık 1,5 saat sürer.

## Kaynaklar

- Caruso, C. M., Soda, P. ve Guarrasi, V. (2026). Not another imputation method: A transformer-based model for missing values in tabular datasets. *AI Open*, 7, 96–122. https://doi.org/10.1016/j.aiopen.2026.02.005
- NAIM kaynak kodu: https://github.com/cosbidev/NAIM (commit `2f15af8`)
- Rubini, L. J., Soundarapandian, P. ve Eswaran, P. (2015). Chronic Kidney Disease. UCI Machine Learning Repository.
