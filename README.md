# RAG_project
Apache Sunucu Günlük Kayıtlarına Dayalı RAG Tabanlı LLM Destekli Q&A Uygulaması

## Genel Bakış:

Bu proje, Apache sunucu günlük kayıtlarını kullanarak sorulara yanıt vermeyi amaçlayan bir Retrieval-Augmented Generation (RAG) sistemi oluşturmayı içerir. Proje, esas olarak Google Colab'da geliştirilmiş olup, veri ön işleme işlemi Jupyter Notebook'ta gerçekleştirilmiştir. Uygulama, günlük kayıt verilerini gömmek için sentence transformers, vektör depolama ve sorgulama için ChromaDB, yanıtları oluşturmak için ise Gemini 1.5 Flash büyük dil modelini (LLM) kullanır.

### Veri Seti:
->Kaynak: Apache sunucu günlük kayıtları Kaggle'dan elde edilmiştir.
#### Ön İşleme:
->Jupyter Notebook'ta Pandas kullanılarak günlük verileri işlenmiş ve CSV formatına dönüştürülmüştür.
->Format sorunları nedeniyle veri seti 380 satıra düşürülmüştür.

### Model Seçimi:
#### İlk Model: distiluse-base-multilingual-cased-v1
->Başlangıçta cümle gömme için seçilmiş ancak daha sonra proje ihtiyaçları için yetersiz bulunmuştur.
#### Güncellenmiş Model: sentence-transformers/multi-qa-distilbert-cos-v1
->Daha iyi performansı ve daha geniş token kapasitesi nedeniyle tercih edilmiştir.

### Uygulama Detayları:
#### Verilerin ChromaDB'ye Yüklenmesi:
->İşlenmiş CSV dosyası, Google Colab'da aşağıdaki fonksiyon kullanılarak ChromaDB'ye yüklenir:

##### chroma_client, chroma_collection = load_multiple_csvs_to_ChromaDB(collection_name, sentence_transformer_model)
##### load_multiple_csvs_to_ChromaDB()
Bu fonksiyon:
->CSV dosyasını okur.
->Verileri satırlara böler.
->Metni tokenlere çevirir.
->Tokenlere meta veri ekler.
->İşlenmiş veriyi ChromaDB koleksiyonuna yazar.


#### Belgelerin Sorgulanması:
Belirli bir sorgu ile ilgili belgeleri ChromaDB'den sorgulamak için:

##### retrieved_documents = retrieveDocs(chroma_collection, query, 10)
##### retrieveDocs()
Bu fonksiyon:
->Sorgu ile eşleşen belgeleri ChromaDB vektör veri tabanında arar.
->En uygun 10 belgeyi döndürür.

### Büyük Dil Modeli (LLM) Entegrasyonu
Gemini 1.5 Flash modeli, Google Colab'da çalıştırılarak, sorgulanan belgelerden yanıtlar oluşturmak için kullanılır.

#### Sistem İsteği: 
->Colab'da bir sistem isteği belirlenir ve LLM'in bağlama uygun yanıtlar oluşturması sağlanır.
#### Manuel Testler: 
->RAG sistemi, Colab'da çeşitli sorgular girilerek ve LLM tarafından üretilen yanıtlar değerlendirilerek manuel olarak test edilmiştir.

### Son RAG Sistemi Kurulumu
->RAG modeli, vektör veri tabanına bağlanır.
->Sistem Google Colab'da dağıtılır ve performansının doğruluğu test edilir.
->Sistem, son olarak Gradio arayüzüne entegre edilerek kullanıma sunulur.

### Gradio Arayüzü
->Proje tamamlandıktan sonra, temel proje basit bir Gradio arayüzüne taşınmış ve bu arayüz üzerinden kullanılabilir hale getirilmiştir. Gradio, kullanıcıların web tabanlı bir arayüzde sorgular yapmasına ve sonuçları görüntülemesine olanak tanır.

### Model Performans Testi
Model, toplamda 10 farklı soru ile test edilmiştir. Test sonuçlarına göre:

->8 Doğru Yanıt: %80 doğruluk oranı.
->2 "Cevap Bulunamadı" Yanıtı: %20 oranında cevap bulamama durumu.
->Halüsinasyon: Test sırasında model hiçbir halüsinasyon üretmemiştir.

### Projenin Çalıştırılması
#### Veri Ön İşleme: 
->Jupyter Notebook'ta belirtilen talimatlara göre veri ön işleme işlemini gerçekleştirin.
#### Verileri ChromaDB'ye Yükleme:
->Sağlanan Colab not defterini kullanarak işlenmiş veriyi ChromaDB'ye yükleyin.
#### Sistemi Sorgulama:
->Veritabanına sorgu yapmak için geri getirme fonksiyonlarını çalıştırın.
->Colab'da RAG sistemi tarafından üretilen yanıtları değerlendirin.
#### Gradio Arayüzü:
->Gradio arayüzünü başlatarak sistemi web üzerinden kullanın.
