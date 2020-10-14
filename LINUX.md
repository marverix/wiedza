# Linux

## Przydatne rzeczy

### Czekaj aż będzie Internet

```bash
echo "Waiting for Internet ..."
while ! timeout 0.2 ping -c 1 -n 8.8.8.8 &> /dev/null
do
    printf "%c" "."
done
echo "OK"
```

### Czekaj na unlock apt

```bash
echo "Waiting to unlock apt..."
while sudo fuser /var/lib/dpkg/lock /var/lib/dpkg/lock-frontend /var/lib/apt/lists/lock /var/cache/apt/archives/lock >/dev/null 2>&1; do
   sleep 1
done
echo "OK"
```

### Usunięcie z pliku linii pasującej do regexpa

```sh
sed -i '/wyrazenie/d' /jakis/plik
```

### Wyszukaj i podmień w pliku

```sh
sed -i 's/wyszukaj/podmien/g' /jakis/plik
```

### Uruchom jako root przy reboocie

Pod warunkiem, że `/root/fistboot.sh` jest skrypt

```sh
echo '@reboot root bash /root/firstboot.sh >> /var/log/firstboot.log 2>&1' >> /etc/crontab;
```

### Znajdź pliki zmienione w ostatniej 1 minucie

```sh
find /jakas/sciezka -mmin 1 -ls
```

### Znajdź pliki zawierające tekst

```sh
grep -rnw /jakas/sciezka -e 'wyrazenie'
```

### Weź tylko pierwszy match grepa

Przykład z użyciem chromium-browser, czyli dzięki temu mogę pobrać major wersję przeglądarki

```sh
chromium-browser --version | grep -o -m 1 -E '[0-9]+' | head -1
```

Link: https://stackoverflow.com/questions/14093452/grep-only-the-first-match-and-stop

### Silent mode w wypadku błędu

Po prostu użyj `|| true`

Link: https://unix.stackexchange.com/questions/118217/chmod-silent-mode-how-force-exit-code-0-in-spite-of-error/193282

### Ustaw tytuł terminala dla skryptu bashowego

```bash
echo -ne "\033]0;tutaj wstaw tytul\007"
```

### Listowanie plików jako Array

```bash
ls | jq -R -s -c 'split("\n")[:-1]'
```

Link: https://stackoverflow.com/a/32354503
