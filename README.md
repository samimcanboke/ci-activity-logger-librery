# Codeigniter Activity Log Library
# Codeigniter Kullanıcı Log Sistemi

Etkinlik Günlüğü, bir web sitesindeki etkinliği kaydetmek ve sonuçları raporlama amacıyla almak için kullanılan bir Codeigniter kütüphanesidir.


  - Kullanımı kolaydır
  - Kayıt eklemek ve getirmek için basit sözdizimi
  - Her tür web sitesinde kullanım için bağımsız kütüphane

### Yükleme

Logger çalışmak için [Codeigniter](https://codeigniter.com) versiyon 3 gerektirir.

- Kütüphaneyi (Logger.php)  `/application/library` klasörüne kopyalayın.
- logger.sql i mysql üzerinde çalıştırınız. (yada tablonuzu kendiniz oluşturunuz.).

Kendiniz oluşturmak için aşağıdaki kodları sql bölümüne yapıştırınız:

```sql
CREATE TABLE `logger` (  `id` bigint(20) NOT NULL AUTO_INCREMENT,  
    `created_on` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,  
    `created_by`int(11) NOT NULL,  
    `type` varchar(255) NOT NULL,  
    `type_id` bigint(20) NOT NULL,  
    `token` varchar(255) NOT NULL, 
    `comment` text NOT NULL );

ALTER TABLE `logger`  ADD PRIMARY KEY (`id`);
```

Aşağıdaki satırları değiştirerek kütüphanedeki(Logger.php) alan adlarını değiştirebilirsiniz.

```php
private $table_fields = array(
    'id'         => 'id', //Değer Gerçek tablo alanı olacak
    'created_on' => 'created_on',
    'created_by' => 'created_by',
    'type'       => 'type',
    'type_id'    => 'type_id',
    'token'      => 'token',
    'comment'    => 'comment',
  );
```

### Çağırma
Logger kütüphanesini çağırmak için aşağıdakileri controller'ınıza ekleyin.

```php
$this->load->library('logger');
```

### Kullanımı 
**Günlük Etkinlikleri**
```php
$this->logger
     ->user(1) //Bu Eylemi oluşturan UserID'yi ayarla
     ->type('post') //Yazı, Sayfa, Giriş gibi giriş türü
     ->id(1) //İçerik ID'si
     ->token('delete') //Belirteç Tanımlama İşlemi
     ->log(); //Database'e yazma kısmı
```

**İçerik Getirme**
```php
$this->logger
     ->user(1) //Yalnızca Belirli Kullanıcıya Özel Girişleri istiyorsanız Ekleyin.
     ->type('post') //Yalnızca Ekle Belirli bir Girdi yazmak istiyorsanız Örneğin sadece postlar gibi.
     ->id(1) //Sadece ilgili yazı için yapılan hareketleri görmek için post id yi girin.
     ->token('delete') //Yalnızca Ekleme gibi Belirli bir giriş belirtmek istiyorsanız doldurun..
     ->get(); //Çağırın.
```

