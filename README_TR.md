# 🎬 cllicllDownloader

**YouTube MP3 & MP4 indirici — en iyi kalite, akıllı fallback, macOS odaklı deneyim.**

`cllicllDownloader`, YouTube videolarını **en yüksek mümkün MP4** ve/veya **320 kbps MP3** olarak alan,
**akıllı fallback zinciri** (ayrı akışlar → biçim 22 → biçim 18) kullanan hafif bir Python CLI aracıdır.

> **Tüm modern Mac'lerde** (Intel & Apple Silicon) sorunsuz çalışır, varsayılan olarak ev klasörlerini kullanır ve
> **tarayıcı çerezlerini** okuyarak yaş sınırlı / hesap gerektiren videolara erişebilir.

---

## ✨ Özellikler

- 🎧 **MP3 (320 kbps)** — en iyi ses + isteğe bağlı thumbnail (kapak) gömme
- 🎥 **MP4 birleştirme** — en iyi video + en iyi ses FFmpeg ile, MP4 öncelikli
- 🔄 **Akıllı fallback zinciri** — `bv*+ba` → `22 (720p)` → `18 (360p)`
- 🍪 **Çerez desteği** — Chrome, Brave, Edge, Vivaldi veya Safari (macOS Keychain onayı)
- 🏠 **Güvenli varsayılan yollar** — `~/Music/cllicllDownloader/Music` (MP3), `~/Movies/cllicllDownloader/Video` (MP4)
- 🧰 Esnek bayraklar: bitrate, özel çıktı klasörleri, thumbnail gömmeyi kapatma vb.
- 🧱 Net hata mesajları ve Ctrl‑C (KeyboardInterrupt) yakalama

---

## 📦 Gereksinimler

- macOS 10.13+ (Intel veya Apple Silicon)
- Python 3.9+
- Homebrew (önerilir)
- **FFmpeg** ve **yt-dlp**

---

## ⚙️ Kurulum (3 adımda)

```bash
# 1) Bağımlılıkları kur
which brew >/dev/null 2>&1 || /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install ffmpeg yt-dlp python@3.12 pipx
pipx ensurepath

# (Apple Silicon kullanıcıları geçici olarak kabuğa tanıtmak isteyebilir)
# eval "$(/opt/homebrew/bin/brew shellenv)"

# 2) Script'i yürütülebilir yap
chmod +x clliclldownloader

# 3) PATH'e ekle
mkdir -p ~/.local/bin
mv clliclldownloader ~/.local/bin/
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

> Gatekeeper dosyayı karantinaya alırsa şu komutla temizleyebilirsin:
>
> ```bash
> xattr -dr com.apple.quarantine ~/.local/bin/clliclldownloader
> ```

---

## 🚀 Kullanım

```bash
# En iyi MP3 (320 kbps) → ~/Music/cllicllDownloader/Music
clliclldownloader "https://www.youtube.com/watch?v=VIDEO_ID"

# En iyi MP4 → ~/Movies/cllicllDownloader/Video
clliclldownloader "https://www.youtube.com/watch?v=VIDEO_ID" --type video

# MP4 + MP3 aynı anda
clliclldownloader "https://www.youtube.com/watch?v=VIDEO_ID" --type both

# Tarayıcı çerezleriyle (yaş sınırlı/hesap gerektiren videolar)
clliclldownloader "https://www.youtube.com/watch?v=VIDEO_ID" --cookies

# Özel klasör + thumbnail gömmeyi kapat
clliclldownloader "https://www.youtube.com/watch?v=VIDEO_ID" --type mp3 \  --music-dir ~/Desktop/Music --no-thumb
```

### Varsayılan Çıktı Klasörleri

| Tür | Yol                      |
| --- | ------------------------ |
| MP3 | `~/Music/cllicllDownloader/Music`  |
| MP4 | `~/Movies/cllicllDownloader/Video` |

---

## 🔧 Parametreler

| Bayrak        | Açıklama                                                              |
| ------------- | --------------------------------------------------------------------- |
| `--type`      | `mp3`, `video` veya `both` (varsayılan: `mp3`)                        |
| `--cookies`   | Tarayıcı çerezlerini okumayı dener (Chrome/Brave/Edge/Vivaldi/Safari) |
| `--bitrate`   | MP3 bitrate (kbps) değeri (varsayılan: `320`)                         |
| `--music-dir` | MP3 çıktı klasörü                                                     |
| `--video-dir` | MP4 çıktı klasörü                                                     |
| `--no-thumb`  | MP3 dosyalarına thumbnail gömmeyi kapatır                             |

---

## 🔐 Çerezler & Keychain

`--cookies` kullandığında, macOS Keychain erişim izni isteyebilir. İzin vermek, giriş gerektiren (örn. yaş sınırlı) videolara erişimi mümkün kılar.

**İpucu:** Safari bazı sürümlerde kısıtlı olabilir; Chrome/Brave genellikle daha sorunsuz çalışır.

---

## 🧰 Sorun Giderme

- **FFmpeg bulunamadı**  
  Homebrew ile kur: `brew install ffmpeg`

- **Gatekeeper engelledi**  
  Karantina niteliğini temizle:

  ```bash
  xattr -dr com.apple.quarantine ~/.local/bin/clliclldownloader
  ```

- **Çerez çıkarımı başarısız**  
  Farklı bir tarayıcı dene (Chrome/Brave). Komutu çalıştırmadan önce tarayıcıyı kapat.

- **“Unsupported URL” veya format hataları**  
  URL'nin standart bir YouTube izleme bağlantısı olduğundan emin ol. Ayrı akışlar yoksa araç otomatik olarak `22` (720p) veya `18` (360p) biçimlerine düşer.

---

## ❓ SSS

**S: Playlist indirebilir miyim?**  
C: Araç `--noplaylist` ile gelir. Dilersen satır satır URL okuyan küçük bir kabuk betiğiyle toplu indirme yapabilirsin.

**S: Dosya adı şablonunu değiştirebilir miyim?**  
C: Evet, script içindeki `outtmpl` değerini (`%(uploader)s - %(title)s.%(ext)s` gibi) düzenleyebilirsin.

**S: Linux/Windows’ta çalışır mı?**  
C: Küçük ayarlamalarla muhtemelen evet. Bu proje macOS varsayılanları ve Keychain davranışına odaklanır.

---

## 🗺️ Yol Haritası

- Opsiyonel playlist modu
- Dosya adı şablon preset’leri
- Tek satır kurulum için `pipx`/Homebrew formülü
- Gelişmiş ilerleme göstergesi / logging anahtarı

---

## 🤝 Katkı

Issue ve PR’ler memnuniyetle kabul edilir! Büyük değişikliklerden önce lütfen bir öneri/planla Issue açın.

---

## 🔒 Güvenlik Notları

- Çerez erişimi yereldir ve macOS Keychain tarafından izinli/korumalıdır.
- Kurmadan önce script’i gözden geçirmen önerilir.

---

## 🪪 Lisans

MIT License © 2025 Celal İÇELLİ
