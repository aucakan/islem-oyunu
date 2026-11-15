# İşlem Oyunu

2003 yapımı VB6 masaüstü klasiği **İşlem Oyunu (v1.03)**'nun Web Audio sesli, **33 dil destekli**, **`rekor.dat` uyumlu** ve indirilerek **çevrim dışı** oynanabilen tek dosyalık HTML5 web versiyonu.

---

## Özellikler

- **1:1 Klasik Oyun Motoru:** 2003 yılında Visual Basic 6.0 ile geliştirilen orijinal masaüstü oyununun durum makinesi, süre sınırları ve puanlama kurallarıyla birebir uyumlu.
- **Tek Dosya, Sıfır Bağımlılık:** Tüm HTML, CSS, JavaScript, SVG ikonlar ve Web Audio ses motoru tek bir `index.html` dosyasındadır. Kurulum, derleme veya internet bağlantısı gerektirmez.
- **Web Audio API Retro Ses Sentezleyici:** Harici ses dosyası olmadan üretilen 4 sentetik efekt:
  - Sayı çarkı dönme sesi (`tik`)
  - Son 5 saniye geri sayım uyarısı (`tak`)
  - Yeni soru / tur iptali efekti (`laser`)
  - Tam ve yaklaşık çözüm kutlama melodisi (`wow`)
- **33 Dil & Otomatik Algılama:** İlk açılışta tarayıcı dilini otomatik algılar; sağ üstteki menüden veya Ayarlar'dan 33 dil arasında geçiş yapılabilir. Arapça (`ar`) ve Farsça (`fa`) için sağdan sola (**RTL**) arayüz desteği içerir.
- **İki Farklı Tema:**
  - **Klasik Win98:** Orijinal `523 × 313 px` VB6 pencere oranları, `#c0c0c0` zemin ve klasik 3B düğme kenarlıkları.
  - **Modern:** Klasik renk paletini koruyan yuvarlatılmış modern pencere görünümü.
- **Özelleştirilebilir Çalışma Alanı:**
  - Nostaljik arka plan renk paleti (varsayılan `#3A6EA5` Win2000 Mavi, `#008080` Win98 Teal, `#1B3B6F` Win95 Lacivert, `#0F291E` CRT Fosfor Yeşili, `#C8C2B0` Kasa Beji ve `#18181B` Antrasit) + tam **HSV / Hex Özel Renk Seçici** ve derinlik geçişi (radyal gradyan) seçeneği.
  - Pencere ölçeği ön ayarları (`1×`, `1.25×`, `1.5×`, `Oto`) ve ekran boyutuna göre otomatik sınırlanan `0.45× – 3.00×` kaydırıcı / manuel ölçek girişi.
  - Pencereyi başlık çubuğundan sürükleyip taşıma, başlığa çift tıklayarak ortalama, kapatıp yeniden açma.
- **Kayıt & `rekor.dat` Uyumluluğu:** Orijinal VB6 `rekor.dat` ikili dosyalarını okur/yazar, tarayıcı içi devam kaydı tutar ve tek tıkla çevrim dışı `IslemOyunu.html` kopyası indirmenize olanak tanır.

---

## Kullanım

### 1. Doğrudan Tarayıcıda Açma
`index.html` dosyasını indirip Chrome, Safari, Firefox veya Edge üzerinde doğrudan açabilirsiniz (`file://` protokolü üzerinden internet olmadan çalışır).

### 2. Ayarlar Üzerinden Çevrim Dışı Kopya İndirme
Web üzerinden oynarken **Ayarlar → HTML İndir (Çevrimdışı)** düğmesine tıklayarak oyunun temiz başlangıç durumuna sahip bağımsız bir `IslemOyunu.html` kopyasını bilgisayarınıza indirebilirsiniz.

### 3. Yerel Sunucu veya GitHub Pages ile Yayınlama
Yerel ağda veya HTTP üzerinden servis etmek için:

```bash
python3 -m http.server 8080
```

GitHub Pages üzerinde yayınlamak için depo ayarlarından (**Settings → Pages**) kaynak dalı `main` ve klasörü `/ (root)` olarak seçebilirsiniz.

---

## Nasıl Oynanır?

Amaç, verilen **6 kaynak sayıyı** en fazla birer kez ve dört işlemi (`+`, `-`, `x`, `/`) kullanarak **hedef sayıya** (`100–999`) ulaşmaktır.

1. **Başlat** (`B`) butonuna basın. Sayı çarkı hedef sayıyı ve kullanılacak 6 sayıyı belirler.
2. **30 saniyelik** düşünme süresi başlar. Son 5 saniyede sayaç kırmızıya döner ve sesli uyarı verir.
3. Çözümü bulduğunuzda veya işlem yapmaya hazır olduğunuzda **Cevapla** (`C`) butonuna basın. Düşünme süresi durdurulur ve kalan saniye kaydedilir.
4. İşlem aşamasında her sayı ve operatör seçimi için **5 saniyeniz** vardır; yaptığınız her hamlede 5 saniyelik sayaç yenilenir.
   - Her başlangıç sayısı yalnızca **bir kez** kullanılabilir.
   - Ara sonuçlar üstteki **3 yuvaya** yazılır ve sonraki işlemlerde tekrar kullanılabilir.
   - Bölme işlemi (`/`) yalnızca **tam bölünebilen** durumlarda geçerli sonuç üretir (`num1 % num2 === 0`).
   - Her işlem `=` ile tamamlanır.

### Puanlama ve Cezalar

| Durum | Puan Değişimi |
| :--- | :--- |
| **Tam isabet** (`sonuç === hedef`) | `+ (2 × kalan süre)` |
| **1 farkla yaklaşık çözüm** (`\|sonuç - hedef\| === 1`) | `+ kalan süre` |
| **2 farkla yaklaşık çözüm** (`\|sonuç - hedef\| === 2`) | `+ (kalan süre / 2)` |
| **30 sn düşünme süresi dolarsa** | `-5` |
| **İşlem adımında 5 sn dolarsa** | `- max(10, 2 × kalan süre)` |
| **Yeni Soru (`Y`) ile turu iptal etme** | Düşünme aşamasında `-5`; Cevapla aşamasında `- max(10, 2 × kalan süre)` |

> **Not:** Toplam puan `0`'ın altına düşmez. Aktif puanınız mevcut rekoru geçtiğinde adınızı girmeniz istenir (en fazla 15 karakter) ve yeni rekor kaydedilir.

---

## Klavye Kısayolları

Klavye kısayollarını oyun altındaki bilgi çubuğu olarak **Ayarlar → Klavye kısayollarını göster** seçeneğinden açabilirsiniz:

| Tuş | İşlev |
| :--- | :--- |
| `B` | **Başlat** (Yeni tur başlatır) |
| `C` | **Cevapla** (Süreyi durdurur ve işlem butonlarını açar) |
| `R` | **Rekor** (Kayıtlı en yüksek skoru gösterir) |
| `Y` | **Yeni Soru** (Aktif turu iptal eder) |
| `1` – `6` | Kullanılacak 6 kaynak sayıdan birini seçer |
| `Q` / `W` / `E` veya `7` / `8` / `9` | 1., 2. ve 3. ara sonuç yuvasını seçer |
| `+` | Toplama |
| `-` | Çıkarma |
| `*` veya `x` | Çarpma |
| `/` | Bölme |
| `=` veya `Enter` | Eşittir (işlemi tamamlar / açık pencereyi onaylar) |
| `Esc` | Açık menü, renk seçici, panel veya diyalog penceresini kapatır |

---

## `rekor.dat` Dosya Formatı ve Kayıt Sistemi

Oyun, klasik masaüstü sürümünün `rekor.dat` dosyalarıyla çift yönlü uyumludur:

- **Klasik VB6 İkili Uyumluluk:** `Open "rekor.dat" For Random As #1` yapısına uygun olarak bayt `0`'da little-endian `Float32` rekor puanını, bayt `128`'de uzunluk + **Windows-1254 (CP1254)** Türkçe karakter kodlamasıyla oyuncu adını saklar.
- **Genişletilmiş HTML5 Meta Verisi:** Kayıt alınırken `256.` bayttan itibaren `"ISLM"` başlığıyla UTF-8 JSON meta verisi eklenir; böylece hem rekor hem de aktif oyun puanı (`devamPuan`) aynı `rekor.dat` dosyasında korunur.
- **Esnek Okuma Desteği:** Klasik VB6 `rekor.dat` (0 bayt boş dosya dâhil), HTML5 genişletilmiş `rekor.dat`, JSON ve düz metin (`isim;skor` veya 2 satırlı format) dosyalarını içe aktarabilir; yükleme sırasında **aktif puanla devam etme** veya **yalnızca rekoru yükleme** seçenekleri sunar.
- **Tarayıcı İçi Depolama (`localStorage`):**
  - `islem_oyunu_rekor_v1` — Kayıtlı rekor puan ve oyuncu adı
  - `islem_oyunu_kayit_v1` — "Son Kayıttan Devam Et" verisi
  - `islem_oyunu_prefs_v2` — Dil, tema, arka plan rengi, ölçek, ses ve kısayol tercihleri

---

## Desteklenen Diller (33 Dil)

<details>
<summary>Desteklenen tüm dilleri görüntüle</summary>

- Türkçe (`tr`)
- English (`en`)
- Español (`es`)
- Português (`pt`)
- Français (`fr`)
- Deutsch (`de`)
- Italiano (`it`)
- Русский (`ru`)
- 简体中文 (`zh-CN`)
- 繁體中文 (`zh-TW`)
- 日本語 (`ja`)
- 한국어 (`ko`)
- Bahasa Indonesia (`id`)
- हिन्दी (`hi`)
- العربية (`ar` — RTL)
- Nederlands (`nl`)
- Polski (`pl`)
- Українська (`uk`)
- Tiếng Việt (`vi`)
- ภาษาไทย (`th`)
- Bahasa Melayu (`ms`)
- Filipino (`tl`)
- فارسی (`fa` — RTL)
- Ελληνικά (`el`)
- Română (`ro`)
- Magyar (`hu`)
- Čeština (`cs`)
- Slovenčina (`sk`)
- Hrvatski (`hr`)
- Svenska (`sv`)
- Dansk (`da`)
- Norsk (`nb`)
- Suomi (`fi`)

</details>

---

## Orijinal Sürüm Hakkında

| Bilgi | Detay |
| :--- | :--- |
| **Orijinal Oyun** | İşlem Oyunu (v1.03) |
| **Geliştirici** | VAS Yazılım (2003) |
| **Orijinal Dosya** | `İşlemOyunu.exe` (56 KB, 32-bit x86) |
| **Altyapı** | Visual Basic 6.0 (`MSVBVM60`) & `winmm` |
| **Hedef Sistem** | Windows 95 / 98 / ME / 2000 / XP (Win32) |
| **HTML5 Uyarlaması** | Saf HTML5 / CSS / JS, Web Audio API, `rekor.dat` uyumu |
