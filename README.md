# RedTeam Sim — Sanal Pentest Eğitim Oyunu

Tek dosyalık, tarayıcıda çalışan, **tamamen simüle edilmiş** bir sızma testi (pentest) eğitim oyunu. Gerçek bir ağa bağlanmaz, gerçek bir exploit çalıştırmaz — her komut ve senaryo önceden yazılmış, script'lenmiş bir simülasyondur. Amaç, siber güvenliğe yeni başlayanlara gerçek bir pentest sürecinin akışını (keşif → istismar → yetki yükseltme → yanal hareket → hedefe ulaşma) eğlenceli ve etkileşimli bir şekilde yaşatmaktır.

## Özellikler

- **Sanal masaüstü ortamı** — sürüklenebilir pencereler, görev çubuğu (taskbar), masaüstü ikonları
- **Terminal** — `nmap`, `hydra`, `ftp`, `nikto`, `find`, `theharvester` gibi gerçekçi sahte komutlar
- **Tarayıcı penceresi** — OSINT için sahte kurumsal site ve hedef web sayfaları
- **Chat penceresi** — "işveren" ile senaryo içi diyaloglar ve ipuçları
- **RDP penceresi** — yanal hareket (lateral movement) sonrası açılan ikinci (Windows) makine simülasyonu
- **Dinamik savunma (defender)** — riskli hareketler şüphe seviyesini artırır, rastgele aralıklarla IP banlanabilir
- **Rate-limiting / IDS mantığı** — aynı komutu art arda tekrarlamak ekstra şüphe yaratır
- **Honeypot mekaniği** — çok "temiz" görünen bir servise dikkatsizce yaklaşmak tuzağa düşürür
- **SUID / privilege escalation bulmacası** — düşük yetkili shell'den root'a çıkış
- **Proxy / IP değiştirme sistemi** — banlanınca oyunu bitirmek yerine kimlik değiştirip devam etme
- **Gerçekçi final** — hedef dosyayı bulup açtığında işverenden teşekkür mesajı, `oyna` komutuyla sıfırdan başlama

## Nasıl çalıştırılır

Kurulum veya bağımlılık gerekmez. Tek bir statik HTML dosyasıdır.

```bash
git clone <bu-repo>
cd redteam-sim
# index.html dosyasını herhangi bir tarayıcıda açın
```

veya basit bir yerel sunucu ile:

```bash
python3 -m http.server 8000
# tarayıcıda http://localhost:8000 adresine gidin
```

GitHub Pages ile de tek tıkla yayınlanabilir: Settings → Pages → Branch: main → kaydet.

## Oynanış özeti

1. **Tarayıcı**'yı açıp `targetcorp.local` adresine giderek OSINT yapın.
2. **Terminal**'de `nmap -A 10.10.10.5` ile hedefi tarayın (şüpheli derecede temiz görünen 8080 portuna dikkat — honeypot olabilir).
3. Açık portlardan birini kullanarak erişim kazanın (`ftp` + `get backup.zip`, veya `hydra` ile brute-force).
4. `ssh` ile düşük yetkili bir shell elde edin.
5. `find / -perm -4000` ile SUID açığını bulun, istismar ederek **root** olun.
6. `cat /root/.creds` ile ikinci makinenin bilgilerini bulun, `rdp <ip>` ile **yanal hareket** yapın.
7. Açılan **WIN-SRV** penceresinde Masaüstü → `top_secret.txt` dosyasını açarak görevi tamamlayın.
8. Terminalde `oyna` yazarak baştan başlayabilirsiniz.

Şüphe seviyeniz çok yükselirse IP'niz banlanır; `change-ip` ile (sınırlı sayıda) yeni bir kimlikle devam edebilirsiniz.

## Teknik notlar

- Saf HTML/CSS/JavaScript, framework veya build adımı yok.
- Tek dosya (`index.html`), harici bağımlılık içermez.
- Mobil ve masaüstü tarayıcılarda çalışacak şekilde tasarlanmıştır (dokunmatik sürükleme desteklidir).
- Tüm "komutlar", "taramalar" ve "exploit'ler" kurgusal/simüledir; gerçek siber güvenlik araçlarıyla hiçbir bağlantısı yoktur.

## Katkıda bulunma

Pull request'ler ve öneriler memnuniyetle karşılanır — yeni senaryolar, ek komutlar, zorluk seviyeleri veya görsel iyileştirmeler eklenebilir.

## Lisans

[MIT](./LICENSE) — özgürce kullanabilir, değiştirebilir ve dağıtabilirsiniz.

## Sorumluluk reddi

Bu proje tamamen eğitim amaçlıdır. Hiçbir gerçek sistem, ağ veya kişiye yönelik gerçek bir sızma testi aracı değildir; içindeki tüm komut çıktıları kurgusaldır. Gerçek sistemlere izinsiz erişim denemek yasa dışıdır.
