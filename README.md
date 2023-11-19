<h1 align="center"> Dymension Rollapp </h1>

Basit bir NFT Mint sitesi yapma.(!)

>@enzifiri hocanın Github Repository'sini kullanarak `Switch Network` ve `Connect Wallet` tuşlarını oluşturun.

[Enzifiri Hocanın Github Reposu](https://github.com/enzifiri/dApp-Starter-RC)

>Bu işlemi yaptıktan sonra bir NFT contract' ına ihtiyacımız var. Bunun için `https://remix.ethereum.org/` sitesini kullanabilirsiniz. 
Nasıl contract oluşturacağınızı bilmiyorsanız. Şu linklerden yardım alabilirsiniz ;

[NFT Contract Oluşturma ile Alakalı Video](https://www.youtube.com/watch?v=GwFQg8ROZfo&t)

[ChatGPT](https://chat.openai.com/) (En önemlisi bence bu)

>Bu kaynakları kullanarak bir contract oluşturabilirsiniz. 

Örnek ChatGpt prompt olarak ;

```
ChatGpt ye ben bir NFT mint ile alakalı bir dapp oluşturacağım. 
NFT mint contract'ı için Solidity kullanacağım. 
Oluşturacağım dappde mint tuşuna basan herkes mintleyebilsin. 
Arzı ...... olsun. 
Benim evm ağımın Adı ..... , 
ChainId ....... , 
Sembol ......... , 
Rpc .......... ,. 
Bana bunun için ERC721 contract yaz.
```

Bu şekilde contract oluşturuyoruz.

Önemli noktalar;

>`https://remix.ethereum.org/` sitesini kullanarak contract oluşturuyorsanız. 'Solidity Compiler' yaparken `Advanced Configurations`' tan `EVM Versions` kısmını `Paris` yapın.

Sonrasında aynı şekilde;
>https://remix.ethereum.org/ sitesinde `Solidity Compiler` kısmından `ABI`' yı kopyalayın. Ardından `/root/my-react-app/src` klasöründe `MyNftContractABI.json` oluşturun ve içeriğine `ABI`' yı yapıstırın.

>Bunları tamamladıktan sonra ChatGpt' ye aynı şekilde bir dapp yaptığınızı Mint tuşunun olacağını söyleyin. Ona contract adresini ve `ABI` dosyasının nerede olduğunu belirtin ve `/root/my-react-app/src` içindeki `App.js` dosyasının içindeki yazıyı gönderin. O sizin için bir kod oluşturacak o kodu `App.js` dosyasının içindeki kod ile değiştirin. Bu işlemleri yaptıktan sonra tekrardan sitenizi kontrol edin `http://localhost:3000` e girdiğinizde mint nft tuşu olması ve çalışması gerekiyor. Kodda hata olduğunu fark ederseniz hata ile beraber GPT ye sorun. 

Sayfanın frontend kısmını yapmak için;
>`/root/my-react-app/src` dizininin içinde `styles.css` dosyası oluşturun.

>Bunu `/root/my-react-app/public` içindeki `index.html` dosyasıyla bağlayın ve `App.js` içerisine import kodu ekleyin. Bilmiyorsanız GPT yardımcı olur size. Sonrasında o sayfayı istediğiniz gibi düzenleyin. Bilmiyorsanız GPT. Bunun aynısı yapıldığı için GPT ile birkaç farklı özellik daha ekleyin.

Tüm bu adımları tamamladıktan sonra bir domain alıp onu kendi sitenize yönlendirmeniz lazım.(Zorunlu değil)

1) Domain aldıktan sonra DNS Management bölümüne girin. 
```
Type: A  
Name:@ 
#Points to yerine başka bir şey yazıyor olabilir takılırsanız GPT
Points to : SİZİNIPADRESİNİZ 
```

Bu şekilde yönlendirmeyi yapmış oluyoruz sonrasında aşağıdaki işlemleri sunucumuzda sırasıyla yapıyoruz.

2) Nginx'i Yükleyin:

```
sudo apt update
sudo apt install nginx
```

3) Nginx'i Yapılandırın:

```
sudo nano /etc/nginx/sites-available/default
```

>Dosyayı açtıktan sonra, şu şekilde düzenleyin:

```
server {
    listen 80;
    server_name Yourdomain www.Yourdomain;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

>Yourdomain ve www.Yourdomain kısımlarını, satın aldığınız domain adınıza uygun olarak değiştirin.

>http://localhost:3000 kısmında `localhost` kısmına Sunucu ip nizi yazın.

4) Nginx'i Yeniden Başlatın:

```
sudo systemctl restart nginx
```

5) Güvenlik Duvarı Ayarlarını Yapın:
Eğer güvenlik duvarı kullanıyorsanız, güvenlik duvarınızı Nginx'in trafiğini kabul edecek şekilde ayarlayın:

```
sudo ufw allow 'Nginx Full'
```

6) SSL/TLS Sertifikası Kurulumu (İsteğe Bağlı):

Eğer `HTTPS` kullanmak istiyorsanız, Certbot'u kullanarak ücretsiz SSL sertifikası alabilirsiniz:

```
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx
```

>Çıkan sorulara isteğiniz doğrultusunda cevaplayınız.

@enzifiri 'nin Ssl sertifika reposuna ekleme yapılmıştır.
