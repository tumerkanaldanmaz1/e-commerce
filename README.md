E-commerce Data Analysis

Bu proje, bir e-ticaret veri setini analiz etmek amacıyla Python kullanarak çeşitli veri temizleme, SQL sorgulama ve görselleştirme işlemleri gerçekleştirmektedir. Projede, satış verileri üzerinde analiz yaparak en çok satan ürünler, hangi ülkelerde daha fazla satış yapıldığı ve en çok harcama yapan müşteriler gibi bilgiler elde edilmiştir.

İçerik

Dosya Yükleme ve Veri Okuma: Kaggle'dan indirilen e-ticaret veri seti, Google Colab üzerinde yüklenip işlenmiştir.
Veri Temizliği: Eksik müşteri ID'leri çıkarılmış, negatif miktarlar (iptal işlemleri) ayıklanmış ve doğru veri tiplerine dönüştürülmüştür.
SQL ile Veri Analizi: pandasql kütüphanesi kullanılarak SQL sorguları ile en çok satan ürünler, ülkelere göre satışlar ve en çok harcama yapan müşteriler gibi analizler yapılmıştır.
Power BI/Tableau için Veri Hazırlığı: Günlük satış verileri CSV formatında dışa aktarılmıştır.
Proje Adımları

1. Dosya Yükleme
Proje, kullanıcıdan CSV dosyasını yüklemesini ister ve dosya Google Colab'a yüklenir. Veri seti Türkçe karakterleri doğru şekilde okuyabilmek için özel bir encoding ile yüklenir.

from google.colab import files
import pandas as pd

# Dosya yükleme kutusunu aç
uploaded = files.upload()
# Yüklenen dosyanın adını otomatik al
file_name = next(iter(uploaded))

# CSV'yi oku (Türkçe karakterler için özel encoding)
df = pd.read_csv(file_name, encoding='ISO-8859-9')

# İlk 5 satırı göster
df.head()
2. Veri Temizliği
Eksik değerler ve iptal edilen satışlar ayıklanmış, veri seti temizlenmiştir. Ayrıca tarih sütunu doğru formatta işlenmiştir.

df['InvoiceDate'] = pd.to_datetime(df['InvoiceDate'])
df = df.dropna(subset=['CustomerID'])  
df['CustomerID'] = df['CustomerID'].astype(int)
df['IsCancelled'] = df['Quantity'] < 0
df_sales = df[~df['IsCancelled']] # Sadece satışlar (iptal olmayanlar)
3. SQL ile Veri Analizi
Veri, SQL sorguları ile analiz edilmiştir. pandasql kütüphanesi ile SQL sorguları yazılarak en çok satan ürünler, ülkelere göre satışlar ve en çok harcama yapan müşteriler bulunmuştur.

Örnek SQL sorgusu:

query1 = """
SELECT Description, SUM(Quantity) AS TotalQuantity
FROM df_sales
GROUP BY Description
ORDER BY TotalQuantity DESC
LIMIT 10
"""
topproducts = pysqldf(query1)
4. Power BI/Tableau İçin Veri Hazırlığı
Günlük satış verileri toplanarak dışa aktarılmış ve görselleştirme araçlarına yüklenebilecek bir formatta CSV dosyası olarak kaydedilmiştir.

df_sales['Date'] = df_sales['InvoiceDate'].dt.date
df_sales['TotalPrice'] = df_sales['Quantity'] * df_sales['UnitPrice']
df_powerbi = df_sales[['InvoiceNo', 'Date', 'CustomerID', 'Country', 'Description', 'Quantity', 'UnitPrice', 'TotalPrice']]
df_powerbi.to_csv('powerbi_data.csv', index=False)
Gereksinimler

Proje, aşağıdaki Python kütüphanelerini kullanmaktadır:

pandas: Veri işleme ve analiz için.
pandasql: SQL sorguları yazmak için.
google.colab: Google Colab ortamında çalışmak için.
Bu kütüphaneler, Python 3 ile uyumludur ve aşağıdaki pip komutlarıyla yüklenebilir:

pip install pandas
pip install pandasql
Kullanım

Google Colab ortamında projeyi açın.
Dosyayı yüklemek için files.upload() fonksiyonunu kullanın.
Veriyi yükledikten sonra, veri temizliği ve analiz adımlarını sırasıyla çalıştırın.
SQL sorguları ile analizlerinizi gerçekleştirin.
Sonuçları Power BI veya Tableau gibi araçlarda görselleştirmek için CSV dosyalarını dışa aktarın.
Katkıda Bulunma

Eğer bu projeye katkıda bulunmak isterseniz, aşağıdaki adımları izleyebilirsiniz:

Bu depo ile ilgili herhangi bir iyileştirme öneriniz varsa, lütfen bir pull request açın.
Projeye yeni özellikler eklemek için katkıda bulunabilirsiniz.
Lisans

Bu proje MIT Lisansı altında lisanslanmıştır.
