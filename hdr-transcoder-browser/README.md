# HDR Transcoder Studio (Browser)

Tarayıcıda çalışan tek sayfalık FFmpeg WebAssembly video dönüştürücü.

## Çalıştırma

`index.html` dosyasını doğrudan `file://` olarak açmak yerine aynı klasörde yerel HTTP sunucusu başlatın. macOS/Linux/Windows'ta Python 3 varsa:

```bash
cd hdr-transcoder-browser
python3 -m http.server 8000
```

Ardından tarayıcıda `http://localhost:8000` adresini açın. İlk kullanımda FFmpeg WebAssembly dosyaları CDN'den indirileceği için internet bağlantısı gerekir.

## Kullanım

1. Video dosyasını seçin.
2. Kaynak HDR ise `HDR → SDR`; kaynak zaten SDR ise `SDR → SDR` seçin.
3. Çözünürlük, kodlama hızı ve CRF kalite seviyesini ayarlayın.
4. `Motoru yükle ve dönüştür` düğmesine basın.
5. İşlem bitince MP4 dosyasını indirin.

## Teknik notlar

- Dosya bir sunucuya gönderilmez; FFmpeg WebAssembly tarayıcıda çalışır. FFmpeg motor dosyaları CDN'den indirilir.
- `vo=gpu-next`, `gpu-api=d3d11` ve `hwdec=d3d11va` mpv oynatma ayarlarıdır. Tarayıcıdaki FFmpeg WASM bu ayarları kullanmaz ve kodlama CPU üzerinde çalışır.
- `bt.2390`, libplacebo'nun dinamik tepe parlaklığı analizi ve `fruit` dithering ile bire bir aynı çıktı üretilmez. HDR→SDR profili, FFmpeg `tonemap=mobius` ve `zscale` yazılım filtresiyle yaklaşık dönüşüm yapar.
- FFmpeg WASM masaüstü FFmpeg'den belirgin şekilde yavaş olabilir ve büyük dosyalarda bellek sınırına takılabilir. Önce kısa bir kliple test edin.
- Altyazılar ve ek veri akışları MP4 çıktısına aktarılmaz. Kaynak ve FFmpeg core desteğine bağlı olarak bazı kodekler açılmayabilir.
