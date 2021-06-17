# Przydatne programy

## Internety

### Google Chrome - bez Snapa

```sh
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
sudo apt-get update
sudo apt-get install google-chrome-stable # (albo -beta)
```

### Ungoogled Chromium

Wersja Chromium z wyciętymi wszystkimi rzeczami związanymi z Google.

```sh
echo 'deb http://download.opensuse.org/repositories/home:/ungoogled_chromium/Ubuntu_Focal/ /' | sudo tee /etc/apt/sources.list.d/home-ungoogled_chromium.list > /dev/null
curl -s 'https://download.opensuse.org/repositories/home:/ungoogled_chromium/Ubuntu_Focal/Release.key' | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/home-ungoogled_chromium.gpg > /dev/null
sudo apt update
sudo apt install -y ungoogled-chromium ungoogled-chromium-driver

# Create alias
ln -s /usr/bin/chromium /usr/bin/chromium-browser || true
```

## Grafika

### Flameshot

Bardzo fajny program do robienia screenshotów i rysowania na nich

#### Instalacja

https://flameshot.js.org/#/getting-start

```sh
sudo apt install flameshot
```

#### Komendy

Otwórz narzędzie do zaznaczania

```sh
flame gui --path /home/marverix/Pulpit
```

Zrób screena obecnego ekranu

```sh
flameshot screen --path /home/marverix/Pulpit
```

Zrób screena całego ekranu (wszystkich monitorów na raz)

```sh
flameshot full --path /home/marverix/Pulpit
```

## Multimedia

### ffmpeg

#### Usuń dźwięk

```sh
ffmpeg -i $input_file -c copy -an $output_file
```

### Fre:ac

Chyba najlepszy program na Linuxa do ripowania Audio CD. Dużo opcji, dekoderów, koderów, itp

```sh
sudo snap install freac --beta
```

## Terminal

### Alacritty

https://github.com/alacritty/alacritty

Terminal emulator renderujący się na GPU

```sh
sudo apt install alacritty
```

### fish

https://github.com/fish-shell/fish-shell

Alternatywa dla bash i zsh

```sh
sudo apt-add-repository ppa:fish-shell/release-3
sudo apt-get update
sudo apt-get install fish
```

### nnn

https://github.com/jarun/nnn

Zarąbista alternatywa dla Midnight Commander'a

```sh
sudo apt install nnn
```

Wiki:

https://github.com/jarun/nnn/wiki/Usage

Skróty klawiszowe:

https://camo.githubusercontent.com/4e48642e051960d7e05b8eb44a3dd355c2e3d28f/687474703a2f2f7667782e66722f6e6e6e2d636865617473686565742e706e67

### Screenfetch

https://github.com/KittyKatt/screenFetch

Prosty tool, który wyświetla w terminalu krótkie podsumowanie maszyny.

```sh
sudo apt install screenfetch
```

### ncdu

https://dev.yorhel.nl/ncdu

Analizator przestrzeni dyskowej w terminalu. Robi skan zaczynając (domyślnie) od aktualnego katalogu i po zakończeniu
możemy zobaczyć który katalog ile zajmuje itp.

```sh
sudo apt install ncdu
```
