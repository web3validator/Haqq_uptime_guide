# haqq-Alert-Monitoring-Setup


## Для моніторингу ми будемо викорстовувати комплексний інструмент моніторингу мереж Tendermint - Tenderduty
Детальніше можна ознайомитись на офіційному гітхабі https://github.com/lesnikutsa/tenderduty

Даний моніторинг TenderDuty v2 дозволяє здійснювати контроль за нодами та, зокрема, бачити висоту мережі, статус валідатора, аптайм, підписані та пропущені блоки. Також можливе підключення оповіщень у телеграм та дискорд.

Установка можлива різними способами, але я використовуватиму установку через Docker, хоча немає принципової різниці

Нам знадобиться окремий сервер або сервер із уже встановленою нодою (нодами). Також потрібно буде знайти відкриті RPC проекта або відкрити свою на основній (не бажано).

Отже почнемо

### оновлюємо репозиторії
```
sudo apt update && sudo apt upgrade -y

```
### встановлюємо необхідні утиліти
```
sudo apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y

```

### встановлення docker
```
apt install apt-transport-https ca-certificates curl software-properties-common -y && \
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && \
apt update && \
apt-cache policy docker-ce && \
sudo apt install docker-ce -y && \
docker --version

```

<img width="290" alt="Знімок екрана 2022-09-18 о 00 27 37" src="https://user-images.githubusercontent.com/102728347/190877040-9782fa59-bd8b-4354-82f3-f670af6c5add.png">


### встановлюєм tenderduty

відкриваємо нову сесію в скрін 
```
screen -S tenderduty
```
```
cd ~
mkdir -p tenderduty && cd tenderduty
docker run --rm ghcr.io/blockpane/tenderduty:latest -example-config >config.yml
```

### У нас створився конфіг який можно подивитись командою
```
nano $HOME/tenderduty/config.yml
```
або переглянути на цьому ж репозиторії -> [config](https://github.com/MaxMavaIll/haqq-Alert-Monitoring-Setup/blob/main/config.yml)

### Для налаштування моніторингу нам достатньо змінити у конфігу:


1) ім'я мережі 
```
sed -i 's/Osmosis/haqq/' ~/tenderduty/config.yml
```

2) chain-id
```
sed -i 's/osmosis-1/haqq_54211-2/' ~/tenderduty/config.yml
```

3) valoper_address
```
sed -i 's/osmovaloper1xxxxxxx.../haqqvaloper1zqdc83g38lcmcn3pr7c5p5wkz9wf4lqzefejgk/' ~/tenderduty/config.yml
#ви можете його знайти в експлорері натиснувши на свого валідатора за посиланням -> [explorer](https://haqq.explorers.guru/validators)
```

4) url https://haqq-t.rpc.manticore.team:443 - це є RPC проекту також можуть бути
```
sed -i 's/tcp:\/\/localhost:26657/https:\/\/haqq-t.rpc.manticore.team:443/' ~/tenderduty/config.yml
sed -i 's/https:\/\/some-other-node:443/https:\/\/haqq-t.rpc.manticore.team:443/' ~/tenderduty/config.yml
```
<img width="848" alt="Знімок екрана 2022-09-18 о 02 13 14" src="https://user-images.githubusercontent.com/102728347/190879259-a0722c98-628a-44ee-8799-491ff81ed3ca.png">

<img width="510" alt="Знімок екрана 2022-09-18 о 02 13 31" src="https://user-images.githubusercontent.com/102728347/190879262-31257bf1-c98d-45e2-ab33-d94f572d4712.png">

Після налаштування конфігу запускаємо докер:

```shell
docker run -d --name tenderduty -p "8888:8888" -p "28686:28686" --restart unless-stopped -v $(pwd)/config.yml:/var/lib/tenderduty/config.yml ghcr.io/blockpane/tenderduty:latest
```
-p "28686:28686" - тут може будь який вільний порт 

та можемо глянути логування:

```shell
docker logs -f --tail 20 tenderduty
```
<img width="982" alt="Знімок екрана 2022-09-18 о 02 21 14" src="https://user-images.githubusercontent.com/102728347/190879393-516e4204-2c8e-4440-a50f-245b9310096d.png">


### А тепер ми можемо переглянути інформацію у браузері заходимо по адресі:

http://твій-айпі:8888/

перед цим також важлиов відкрити цей порт якщо у вас стоїть файрвол
```shell
sudo ufw allow 8888
```
ось так воно виглядає

<img width="1419" alt="Знімок екрана 2022-09-18 о 02 21 58" src="https://user-images.githubusercontent.com/102728347/190879414-ccfc8cd1-3e4f-481f-bfd0-97824d7af4e3.png">


Вітаю ви встановили моніторинг для мережі Haqq!
Тепер можемо приступити до сповіщання в телеграм. Гайд розмістив на цьому ж репозиторії за посиланням тут -> [create_bot](https://github.com/MaxMavaIll/haqq-Alert-Monitoring-Setup/blob/main/Create_Haqq_bot.md)
