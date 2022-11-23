# Exorde Participation Module CLI v1.3.4a (Docker-UA) Guide

### Мінімальні вимоги:

* 2 або більше ядра ЦП
* 1 Гб пам'яті SSD
* 4 Гб оперативної пам'яті
* 100+ мб підключення інтернету

## Офіційні посилання на проект:

[Еxplorer Exorde](https://explorer.exorde.network/)

[Еxplorer Skale](https://light-vast-diphda.explorer.mainnet.skalenodes.com/)

[Таблиця лідерів](https://explorer.exorde.network/leaderboard)

[Сайт](https://exorde.network/)

[Дискорд](https://discord.gg/y9F5Qrtk)

[Телеграм](https://t.me/exorde)

---
### Встановлюємо Докер. Потрібна версія не старіша за `Version Docker 20.10.20`

Перед початком роботи переходимо в папку `root`:

```
cd /root
```

1. Оновлюємо індекси пакетів `apt` за допомогою `update`:

```
sudo apt update
```
2. Встановлюємо набір пакетів, необхідних для доступу до репозиторію Docker по HTTPS:

```
sudo apt install apt-transport-https ca-certificates software-properties-common curl
```

3. Тепер потрібно додати в apt GPG-ключ для роботи з репозиторієм Docker. GPG-ключі використовуються для перевірки підписів програмного забезпечення. Виконуємо цю команду:

```
curl -f -s -S -L https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

4. Додаємо репозиторій Docker в локальний список:

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```

5. Ще раз оновимо шндекс пакетів:

```
sudo apt update
```

6. Встановимо докер. Параметр `-y` в автоматичному режимі відповідає на всі питання установщика `Yes`:

```
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
```

7. Перевірте статус Docker: статус повинен виглядати так `Active: active (running) since Mon 2022-11-07 11:43:24 UTC; 3h 48min ago`

```
sudo systemctl status docker
```

Якщо вибиває помилку, повторіть пункт №6

---

## Запуск ноди:

Команда запускає модуль у фоновому режимі та працює постійно.

```
docker run \
-d \
--restart unless-stopped \
--pull always \
--name YOUR_NAME \
rg.fr-par.scw.cloud/exorde-labs/exorde-cli \
-m <YOUR_MAIN_ADDRESS> \
-l <LOGGING_LEVEL>
```

Одразу після запуску може відображатися версія 1.3.4a і помилка `[Faucet] Auto-Faucet n° 176 Failure... retrying. [Faucet] selecting Auto-Faucet n° 433` потрібно почекати 30 хв або більше, що б нода синхронізувалася, якщо цього не сталось, перезапустити ноду.

* Приклад:

```
docker run \
-d \
--restart unless-stopped \
--pull always \
--name exorde-cli \
rg.fr-par.scw.cloud/exorde-labs/exorde-cli \
-m 0x16f17726399DfF6fc84AD013BD9bCB70F39b42d3 \
-l 2
```

Даною командою можна створити кілька контейнерів з унікальною адресою воркера.

* Примітка до коду:

>`YOUR_MAIN_ADDRESS` - це адреса з кошелька Метамаска "Мережа Ethereum Mainnet", має бути дійсним (бажано новим і пустим).

>`YOUR_NAME` - це назва ноди, кожен раз має бути унікальною.

>`LOGGING` - це рівень обробки логів у модулі (це не настільки важливо для цього оновлення, за статистикою беріть 2 або 3):

* 1 = без логів
* 2 = загальна обробка логів
* 3 = валідація + збір і аналіз логів
* 4 = детальна валідація + збірка та аналіз логів (для усунення помилок)
---

### Корисні команди:

Аргумент пакетної обробки nod `$(docker ps -a -q)` його використовуйте, коли потрібно перезапустити, видалити, запустити всі вузли. Приклад: `docker start $(docker ps -a -q)`

Огляд створених контейнерів:

```
docker ps -a
```

Перевірка логів:

```
docker logs --follow  id ноди
```

* Приклад:

```
docker logs --follow  1f77bd5b66e1
```

Перевірка ревардів:

```
docker logs --follow exorde_1 -n 100 | grep "CURRENT REWARDS"
```

*exorde_1 - це назва ноди.

Переглянути завантаження на сервер:

```
top
```

Зупинити ноду:

```
docker stop ім'я/ідентифікатор ноди
```

Видалити ноду:

```
docker stop ім'я/ідентифікатор ноди
docker rm ім'я/ідентифікатор ноди
```

Перезавантажити модуль:

```
docker restart ім'я/ідентифікатор ноди
```

Переіменувати ноду:

```
docker rename oldname newname
```

---
