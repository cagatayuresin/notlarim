
# 📚 SiteScope ile Sistem İzleme (Detaylı Ders Notları)

## 1. Giriş

Sistem izleme, **BT altyapılarının performansını, kullanılabilirliğini ve güvenilirliğini** sağlamak için kritik bir süreçtir. SiteScope, bu ihtiyacı karşılamak üzere **HP (şimdiki adıyla Micro Focus)** tarafından geliştirilmiş **agentless (ajansız)** bir izleme çözümüdür.  

- **Agentless yaklaşım:** İzlenen sunuculara ek yazılım (agent) yüklemeye gerek yoktur.  
- **Hedef:** Proaktif olarak sorunları tespit etmek, kapasite planlaması yapmak ve SLA’lerin sağlandığını garanti altına almak.  

---

## 2. SiteScope’un Genel Özellikleri

- **Web tabanlı yönetim arayüzü** sayesinde her yerden erişim.  
- **Hazır monitor çeşitliliği** (Windows, Linux, Oracle, SQL, SAP, IIS, Exchange vb.).  
- **Dashboard ve raporlama** özellikleri.  
- **Esnek uyarı mekanizması** (e-posta, SMS, SNMP trap, script tetikleme).  
- **Ölçeklenebilirlik:** Küçük yapılardan büyük kurumsal yapılara kadar uygulanabilir.  
- **Kapsam:**  
  - Donanım izleme (CPU, RAM, disk, network)  
  - Uygulama izleme (IIS, Apache, WebLogic, JBoss vb.)  
  - Ağ cihazı izleme (SNMP ile switch, router, firewall)  
  - Web URL ve API uç noktası izleme  
  - Log dosyası takibi  
  - Servis, port, süreç izleme  

---

## 3. Mimarisi ve Çalışma Prensibi

### 3.1. Agentless Mimarisi

- SiteScope, hedef sistemlere **SSH (Linux), WMI (Windows), SNMP, HTTP, JDBC** gibi standart protokollerle bağlanır.  
- Bu sayede ajan kurulumu gerekmez, bakım maliyeti azalır.  

### 3.2. İzleme Döngüsü

1. Kullanıcı, **monitor** oluşturur (ör: CPU Monitor).  
2. Monitor, belirli aralıklarla sistemi sorgular.  
3. Elde edilen veriler eşik değerlerle (threshold) karşılaştırılır.  
4. Durum dashboard’a yansıtılır (Yeşil = Normal, Sarı = Warning, Kırmızı = Critical).  
5. Eşik değer aşılırsa uyarı tetiklenir (alerting).  
6. Veriler raporlanır ve geçmiş trend analizleri yapılır.  

---

## 4. SiteScope Bileşenleri

### 4.1. Monitor

- SiteScope’un temel yapı taşıdır.  
- İzlenmek istenen sistemsel bileşenler için kullanılır.  
- Örnekler:  
  - **CPU Monitor:** İşlemci kullanımını ölçer.  
  - **Memory Monitor:** Bellek kullanımını ölçer.  
  - **Disk Space Monitor:** Disk doluluk oranını kontrol eder.  
  - **Service Monitor:** Bir Windows/Linux servisini izler.  
  - **URL Monitor:** Web uygulamasının yanıt süresini ve erişilebilirliğini test eder.  
  - **Log File Monitor:** Belirlenen kelime/hata satırının loglara düşüp düşmediğini kontrol eder.  

### 4.2. Template (Şablon)

- Birden fazla monitor’ü gruplandırmak için kullanılır.  
- Yeni sistem eklenirken hızlı kurulum sağlar.  

### 4.3. Alerts (Uyarılar)

- Eşik değer aşıldığında çalışır.  
- Desteklenen yöntemler:  
  - E-posta gönderimi  
  - SMS  
  - SNMP trap  
  - Script veya batch dosyası çalıştırma (otomatik müdahale)  

### 4.4. Dashboard

- İzlenen tüm sistemlerin sağlık durumunu görsel arayüzde sunar.  
- Kritik sistemler için renk kodlu durum bilgisi verir.  

---

## 5. Kurulum ve İlk Yapılandırma

1. **Kurulum:** Windows veya Linux üzerine SiteScope kurulabilir.  
2. **İlk giriş:** Web tabanlı arayüzden yönetim yapılır.  
3. **Yeni monitor ekleme:** İzlenecek sistem seçilir (ör. Windows CPU).  
4. **Threshold tanımlama:** Normal, Warning, Critical eşikleri belirlenir.  
5. **Alert tanımlama:** Bildirim yöntemleri ayarlanır (örn. kritik durumda SMS gönder).  

---

## 6. Uyarı Senaryoları

### Örnek: CPU İzleme

- **Monitor:** CPU Monitor  
- **Threshold:**  
  - Warning → %70 üzeri  
  - Critical → %90 üzeri  
- **Alert:**  
  - E-posta: "Sunucu CPU kritik seviyede"  
  - SNMP trap: NOC ekranına düşer  
  - Script: CPU yüksekse servis restart edilsin  

---

## 7. Raporlama ve Kapasite Planlama

- SiteScope, geçmiş performans verilerini saklar.  
- **Trend analizi:** Örn. disk kullanımının 3 ayda %50’den %80’e çıkması → kapasite arttırma planı.  
- **SLA raporları:** Servislerin uptime/downtime süreleri raporlanır.  

---

## 8. SiteScope’un Avantajları

- Agentless → kolay kurulum ve düşük bakım maliyeti.  
- Kullanıcı dostu web arayüzü.  
- Çok sayıda hazır monitor desteği.  
- Esnek alerting mekanizması.  
- SLA ve raporlama desteği.  

---

## 9. Dezavantajları

- Büyük ölçekli yapılarda ağ trafiğini artırabilir.  
- Agent’lı çözümler kadar derin metrik toplayamayabilir.  
- Lisans maliyeti yüksektir.  
- Modern bulut ortamlarında Prometheus gibi çözümlere göre esnekliği sınırlı kalabilir.  

---

## 10. Rakip Çözümlerle Karşılaştırma

| Özellik        | SiteScope                  | Nagios/Icinga        | Zabbix             | Prometheus+Grafana      |
| -------------- | -------------------------- | -------------------- | ------------------ | ----------------------- |
| Lisans         | Ticari                     | Açık kaynak          | Açık kaynak        | Açık kaynak             |
| Çalışma        | Agentless                  | Agent + plugin       | Agent + agentless  | Agent (exporter)        |
| Arayüz         | Web tabanlı                | Basit web arayüz     | Gelişmiş arayüz    | Grafana entegrasyonu    |
| Kullanım Alanı | Kurumsal altyapılar        | Esnek, düşük maliyet | Orta-büyük yapılar | Container/K8s ağırlıklı |
| Alerting       | E-posta, SMS, SNMP, script | E-posta, SMS, plugin | Gelişmiş alerting  | Alertmanager üzerinden  |

---

## 11. En İyi Pratikler

- **Threshold değerlerini doğru belirle:** Çok düşük eşikler → gereksiz alarmlar (false positive).  
- **Alert suppression (bastırma):** Aynı anda çok sayıda alarm üretiminin önüne geç.  
- **Monitor sayısını optimize et:** Gereksiz monitor’ler sistemi yorabilir.  
- **Çoklu alarm kanalı kullan:** Kritik sistemlerde hem SMS hem e-posta.  
- **Düzenli rapor analizi yap:** Kapasite planlaması için.  

---

## 12. Örnek Sorular ve Cevaplar

### 1. SiteScope’un agentless mimarisi ne avantaj sağlar?  

**Cevap:** İzlenen sistemlere ek yazılım yüklenmez → bakım maliyeti azalır, yönetim kolaylaşır.  

### 2. Monitor nedir? Örnek veriniz  

**Cevap:** İzlenen bileşenleri temsil eden nesnedir. Örn: CPU Monitor, Disk Space Monitor.  

### 3. Threshold kavramı neden önemlidir?  

**Cevap:** Normal, Warning ve Critical seviyeler belirlenerek kritik sorunlara erken müdahale edilir.  

### 4. SiteScope ile CPU izleme nasıl yapılır?  

**Cevap:** CPU Monitor eklenir, eşikler tanımlanır (%70 Warning, %90 Critical), ardından alarm senaryosu belirlenir (e-posta, SMS).  

### 5. SiteScope ile Nagios arasındaki temel fark nedir?  

**Cevap:** SiteScope ticari ve agentless, Nagios açık kaynak ve daha çok agent/plugin tabanlıdır.  

---

## 13. Genel Özet

- **SiteScope = kurumsal düzeyde agentless izleme çözümü.**  
- Çok sayıda monitor, şablon ve güçlü alerting desteği var.  
- Avantajları: kolay kurulum, geniş monitor yelpazesi.  
- Dezavantajları: maliyet, büyük yapılarda trafik yükü.  
- Alternatifler: Nagios, Zabbix, Prometheus+Grafana.  
