# Przydatne programy

## Google Chrome - bez Snapa

```sh
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
sudo apt-get update
sudo apt-get install google-chrome-stable # (albo -beta)
```

## Ungoogled Chromium

```sh
echo 'deb http://download.opensuse.org/repositories/home:/ungoogled_chromium/Ubuntu_Focal/ /' | sudo tee /etc/apt/sources.list.d/home-ungoogled_chromium.list > /dev/null
curl -s 'https://download.opensuse.org/repositories/home:/ungoogled_chromium/Ubuntu_Focal/Release.key' | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/home-ungoogled_chromium.gpg > /dev/null
sudo apt update
sudo apt install -y ungoogled-chromium ungoogled-chromium-driver

# Create alias
ln -s /usr/bin/chromium /usr/bin/chromium-browser || true
```

## Flameshot

### Instalacja

https://flameshot.js.org/#/getting-start

```sh
sudo apt install flameshot
```

### Komendy

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
