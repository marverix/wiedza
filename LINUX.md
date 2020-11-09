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

### Czekaj aż port jest otwarty

```sh
bash -c 'while ! nc -z 127.0.0.1 8001 ; do sleep 1 ; done'
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


## Zarządzanie sprzętem

### Sprawdzanie bad-sectorów

```sh
sudo badblocks -v /dev/sdaX
```

### Dobre GUI do dysków

```sh
sudo apt install gnome-disk-utility
```

### Secure Erase Disk

1. Sprawdź czy dysk nie jest w stanie _frozen_. Jeżeli tak to reboot, albo uśpij i obódź kompa:

    ```sh
    sudo hdparm -I /dev/sdX | grep frozen
    ```

1. Ustaw hasło

    ```sh
    sudo hdparm --user-master u --security-set-pass NULL /dev/sdX
    ```

1. Rozpocznij (UWAGA: NIE MOŻNA TEGO ZATRZYMAĆ! ODŁĄCZENIE ZASILANIA === BRICK DYSKU!!!)

    ```sh
    sudo hdparm --user-master u --security-erase-enhanced NULL /dev/sdX
    ```
