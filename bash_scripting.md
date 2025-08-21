# 📒 Bash Scripting İleri Düzey Ders Notları

## 1. Bash Nedir?

- **Bash (Bourne Again Shell)**: GNU/Linux dünyasında en yaygın kullanılan shell (kabuk) yorumlayıcısıdır.
- **Shell:** Kullanıcı ile işletim sistemi arasında köprü görevi görür.
- **Script:** Bash komutlarının `.sh` dosyalarında sıralı olarak yazılmasıdır.

---

## 2. Script Çalıştırma Yöntemleri

```bash
bash script.sh          # bash yorumlayıcı ile çalıştırma
./script.sh             # dosya çalıştırılabilir hale getirildiyse
source script.sh        # scripti mevcut shell ortamında çalıştırır
```

> Not: `chmod +x script.sh` ile çalıştırılabilir yapılır.

---

## 3. Shebang (#!)

- Scriptin başında bulunur.
- Hangi yorumlayıcıyla çalıştırılacağını belirtir:

  ```bash
  #!/bin/bash
  #!/usr/bin/env bash   # daha taşınabilir yol
  ```

---

## 4. Değişkenler

### Basit değişkenler

```bash
name="Çetolif"
echo "Merhaba $name"
```

### Sayısal işlemler

```bash
sayi=5
echo $((sayi + 3))   # 8
```

### Array (Diziler)

```bash
arr=(elma armut muz)
echo ${arr[1]}      # armut
echo ${arr[@]}      # tüm elemanlar
```

### Associative Array (İlişkisel Dizi)

```bash
declare -A capital
capital[Turkey]="Ankara"
capital[France]="Paris"
echo ${capital[Turkey]}
```

---

## 5. Script Argümanları

```bash
#!/bin/bash
echo "Script adı: $0"
echo "Birinci argüman: $1"
echo "Tüm argümanlar: $@"
echo "Argüman sayısı: $#"
```

- `$0` → script adı  
- `$1, $2, ...` → parametreler  
- `$@` → tüm parametreler  
- `$#` → parametre sayısı  

---

## 6. Kullanıcıdan Girdi Alma

```bash
read -p "Adınızı girin: " ad
echo "Hoşgeldin $ad"
```

---

## 7. Koşullar (if)

```bash
if [ "$sayi" -gt 10 ]; then
  echo "10'dan büyük"
elif [ "$sayi" -eq 10 ]; then
  echo "10'a eşit"
else
  echo "10'dan küçük"
fi
```

### Karşılaştırma Operatörleri

- `-eq` → eşit
- `-ne` → eşit değil
- `-gt` → büyük
- `-lt` → küçük
- `-ge` → büyük eşit
- `-le` → küçük eşit
- `==` / `!=` → string karşılaştırma

---

## 8. Döngüler

### For

```bash
for i in 1 2 3; do
  echo "Sayı: $i"
done
```

### While

```bash
counter=1
while [ $counter -le 5 ]; do
  echo "Sayaç: $counter"
  ((counter++))
done
```

### Until

```bash
counter=1
until [ $counter -gt 5 ]; do
  echo "Sayaç: $counter"
  ((counter++))
done
```

---

## 9. Fonksiyonlar

```bash
function selamla() {
  echo "Merhaba $1"
}

selamla "Çetolif"
```

Return değeri `exit code` ile döner (`0` başarılıdır).

---

## 10. Dosya ve Dizin Kontrolleri

```bash
if [ -f "dosya.txt" ]; then
  echo "Dosya mevcut"
fi

if [ -d "klasor" ]; then
  echo "Klasör mevcut"
fi
```

---

## 11. Çıkış Kodları

- **Exit code**: Her komut `0` (başarılı) veya `!=0` (hata) döner.

```bash
echo $?
```

---

## 12. Hata Yönetimi

```bash
set -e           # hata olursa scripti durdur
set -u           # tanımsız değişken kullanılırsa hata ver
set -o pipefail  # pipe içinde hata olursa yakala
```

---

## 13. Case Yapısı

```bash
read -p "Seçiminiz: " secim
case $secim in
  1) echo "Bir";;
  2) echo "İki";;
  *) echo "Geçersiz";;
esac
```

---

## 14. İleri Konular

### 14.1. Subshell

- `(...)` ile subshell açılır, değişkenler parent shell'i etkilemez.

```bash
( cd /tmp && ls )
```

### 14.2. Command Substitution

```bash
tarih=$(date)
echo "Bugün: $tarih"
```

### 14.3. HereDoc

```bash
cat <<EOF > dosya.txt
Merhaba
Bash Script
EOF
```

### 14.4. Here String

```bash
grep "Merhaba" <<< "Merhaba Dünya"
```

### 14.5. Regex ile If

```bash
if [[ "abc123" =~ ^[a-z]+[0-9]+$ ]]; then
  echo "Eşleşti"
fi
```

---

## 15. Process Yönetimi

- `&` → arka planda çalıştır
- `jobs` → arka plan işleri
- `fg %1` → job'u öne al
- `bg %1` → job'u arka planda devam ettir
- `kill -9 PID` → süreci öldür

---

## 16. Sinyaller ve trap

### Sinyal (Signal)

Linux’te proseslere mesaj göndermek için kullanılan mekanizma.

- `SIGINT` → Ctrl+C (interrupt)
- `SIGTERM` → normal sonlandırma
- `SIGKILL` → zorla öldürme (yakalanamaz)
- `SIGHUP` → terminal kapandığında
- `SIGQUIT` → Ctrl+\

### trap ile yönetim

```bash
trap "echo 'Çıkış yapılıyor...'; exit" SIGINT SIGTERM
```

> Kullanıcı Ctrl+C’ye bastığında script temiz şekilde kapanır.

---

## 17. Debugging

- `bash -x script.sh` → adım adım göster
- `set -x` → debug aç
- `set +x` → debug kapat

---

## 18. Best Practices

- Her zaman `#!/usr/bin/env bash` ile başla.
- `set -euo pipefail` kullan.
- Fonksiyonlara anlamlı isim ver.
- Global değişkenleri büyük harf kullan (`MY_VAR`).
- Loglama için `echo` yerine `logger` veya `printf` tercih et.
- Scripti modüler yap (fonksiyonlara böl).
- Gereksiz `cat` kullanımından kaçın.

---

## 19. Örnek: Sağlam Script Şablonu

```bash
#!/usr/bin/env bash
set -euo pipefail

trap "echo 'Hata veya kesinti! Çıkılıyor...'; exit" SIGINT SIGTERM

function log() {
  echo "[$(date +%H:%M:%S)] $*"
}

function main() {
  log "Script başladı"
  for i in {1..5}; do
    log "Adım $i"
    sleep 1
  done
  log "Script tamamlandı"
}

main "$@"
```

---

## Ek

- **SIGINT:** *Signal Interrupt* demektir. Kullanıcı Ctrl+C tuşuna bastığında proses bu sinyali alır. Normalde scripti sonlandırır, ama `trap` ile yakalanabilir.  
- **SIGKILL:** Scriptin kesin olarak öldürülmesi için kullanılan sinyaldir, yakalanamaz veya engellenemez.  
- **SIGTERM:** Bir prosesin “nazikçe” kapatılması için kullanılan standart sinyaldir.  
