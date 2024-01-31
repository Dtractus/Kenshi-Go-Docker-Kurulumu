# Kenshi-Go-Docker-Kurulumu

> [!NOTE]
> Kenshi yakın zamanda typescript yerine GOLang ile çalışacağını dile getirdi ve testlerine başladı. Test etmek isteyenler için oluşturulmuş bir repodur.
<br>
Kenshi Go versiyonunu Docker ile test etmek için hazırsanız başlayalım.
<br>

## Sunucu Güncelleme ve Docker Kurulumu
```
# Öncelikle sunucudaki güncellemelerimizi ve yükseltmelerimizi yapalım.
sudo apt update -y && sudo apt upgrade -y

# Ardından sunucumuza HTTPS üzerinden indireceğimiz kaynak için sorun çıkmaması adına ve linkten indirdiğimiz zip dosyasını unzip ederken sorun yaşamamak için aşağıda yazan kodları girelim.
sudo apt install apt-transport-https ca-certificates curl software-properties-common wget unzip

# Docker GPG anahtarını sunucumuza ekleyelim
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Docker'ı kaynaklarımıza ekleyelim 
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Eklediğimiz kaynağın tanınması için yeniden güncelleyelim
sudo apt update

# Ubuntu yerin Docker üzerinden kurulum yapacağımızı garanti altına alalım
apt-cache policy docker-ce

```
> [!WARNING]
> En son yazmış olduğumuz kod bize bir ekran çıktısı verecektir. Buradaki çıktıda Installed: (none) olmasına dikkat edelim.
```
docker-ce:
  Installed: (none)
  ...
  ...
  5:20.10.14~3-0~ubuntu-focal 500
  500 https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
  5:20.10.13~3-0~ubuntu-focal 500
  500 https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
  ...
  ...

## Ve ardından Docker'ı yükleyelim
sudo apt install docker-ce

## Docker'ın kurulduğundan emin olmak için aşağıdaki kodu çalıştıralım.
sudo systemctl status docker
```
> [!WARNING]
> En son yazmış olduğumuz kod bize aşağıdaki gibi bir çıktı vermelidir.

```
Output
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     ...
     ...
     ...
```
<br>

## Kenshi Unchained Go Versiyon Kurulumu

```
# wget ile dosyamızı indirelim
wget https://github.com/KenshiTech/unchained/releases/download/go/unchained-go-docker.zip

# İndirmiş olduğumuz dosyamızı unzipleyelim
unzip  unchained-go-docker.zip

# Unzip ile ortaya çıkan dosyamızın içine girelim
cd unchained-go-docker

# cp komutu ile dosya içerisinde bulunan conf.worker.yaml.template isimli dosyamızı conf.worker.yaml olarak kopyalayalım.
cp conf.worker.yaml.template conf.worker.yaml

# Oluşturduğumuz dosyanın içerisine girelim
cp conf.worker.yaml.template conf.worker.yaml

# Aşağıdaki komut ile kopyaladığımız dosya içeriğini düzenleyelim
nano conf.worker.yaml
```

> [!WARNING]
> Nano ile içerisine girdiğimizde değiştirmemiz gereken ilk kısım "name" kısmıdır. Typescript çalıştırıyorsanız lütfen aynı ismi girin ve dosyanın sonuna typescriptinizde bulunan secretKey ve publicKey'i mutlaka ekleyin.<br><br>
> Eğer Kenshi'yi ilk defa çalıştırıyorsanız (Typescript dahi hiç çalıştırmamışsanız) sadece name kısmını düzenleyip çıkabilirsiniz.<br><br>
> Gerekli alanları düzenledikten sonra sırasıyla CTRL + X, Y ve Enter diyerek editörden çıkabilirsiniz.

```
# Yukarıdaki işlemleri hallettiysek şimdi dosyamıza çalıştırma izni verelim.
chmod +x unchained.sh

# Artık çalıştırmaya hazırız! Aşağıdaki kod ile çalıştıralım.
./unchained.sh worker up -d
```

> [!TIP]
> ./unchained.sh worker logs -f  kodu ile loglarınızı kontrol edebilirsiniz.<br><br>
> Herhangi bir sorun yoksa ekran görüntünüz aşağıdaki gibi olmalıdır.<br><br>

![Screenshot_6](https://github.com/Dtractus/Kenshi-Go-Docker-Kurulumu/assets/55835876/9060921b-9e56-401e-bba1-9c0ba4b290fa)
