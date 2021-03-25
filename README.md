![Alt text](assets/logo.jpg?raw=true "BitrixDock")

# Bitrix Docker
Bitrix Docker позволяет легко и просто запускать **Bitrix CMS** на **Docker**
За основу брал проект из репозитории https://github.com/bitrixdock/bitrixdock и доработал ее с дополнительными функциями

## Введение
Bitrix Docker дает возможность установки или восстановление существующего проекта предоставляя готовые сервисы PHP, NGINX, MySQL и многие другие.
Есть возможность установки несколько сайтов и установка сертификата от LetsEncrypt.

### Преимущества данной сборки
- Сервис PHP запакован в отдельный образ, чтобы избавить разработчиков от долгого компилирования.
- Остальные сервисы так же "причёсаны" и разворачиваются моментально.
- Ничего лишнего.

## Порядок разработки в Windows
Если вы работаете в Windows, то требуется установить виртуальную машину, тестировалось на Ubuntu 18.04.
Также если вы разворачиваете Bitrix Docker как локальную машину для разработки, в ```C:\Windows\System32\drivers\etc\hosts``` вы должны прописать IP машины виртуальной и ваши локальные тестовые домены.
Пример:

![hosts_windows](https://raw.githubusercontent.com/darbit-ru/bitrix_docker/master/hosts_windows.png)

Ваш рабочий проект должен хранится в двух местах, первое — локальная папка с проектами на хосте (открывается в IDE), второе — виртуальная машина
(например ```/var/www/bitrix/test1```, ```/var/www/bitrix/test2```). Проект на хосте мапится в IDE к гостевой OC.

## Установка
```
cd /var/www/ && curl -fsSL https://raw.githubusercontent.com/darbit-ru/bitrix_docker/master/install.sh -o install.sh && chmod +x install.sh && ./install.sh
```

После команды, вам отобразится выбор опции

![install_options](https://raw.githubusercontent.com/darbit-ru/bitrix_docker/master/install_options.png)

```
I - установка / добавление нового сайта (при первом запуске следуйте инструкции что будут появляться в командной строке)
R - удаление сайта
S - генерация и добавление SSL сертификата от LetsEncrypt.
F - создание FTP аккаунта к папке сайта
D - удаление FTP аккаунта
```

**Примечание:** при опции R (удаление сайта), из docker контейнера удаляется SSL сертификат, БД сайта и все FTP аккаунты.

### Демонстрация работы

[![Демонстрация работы инструмента](https://img.youtube.com/vi/TKO1J4EOXK4/0.jpg)](https://www.youtube.com/watch?v=TKO1J4EOXK4)



### Проверка статуса Bitrix Docker

Чтобы проверить, что все сервисы запустились посмотрите список процессов ```docker ps```.
Посмотрите все прослушиваемые порты, должны быть 80, 11211, 9000 ```netstat -plnt```.
Наберите домен в браузере и наслаждайтесь.


Если у вас всё получилось, будем благодарны за звёздочку :)
Ошибки ждём в [issue](https://github.com/darbit-ru/bitrix_docker/issues)
Приятной работы!

## Как заполнять подключение к БД
![db](https://raw.githubusercontent.com/darbit-ru/bitrix_docker/master/db.png)

## Примечание
- По умолчанию для сайтов стоит папка ```/var/www/bitrix/```, все новые добавляемые сайты будут в данной папке.
- В настройках подключения требуется указывать имя сервиса, например для подключения к базе нужно указывать "db", а не "localhost". Пример [конфига](configs/.settings.php) с подключением к mysql и memcached.
- Для загрузки резервной копии в контейнер используйте команду: ```cat /var/www/bitrix/backup.sql | docker exec -i mysql /usr/bin/mysql -u root -p123 bitrix```
- При использовании php74 в production удалите строку с `php7.4-xdebug` из файла `php74/Dockerfile`, сам факт его установки снижает производительность Битрикса и он должен использоваться только для разработки

## Отличие от виртуальной машины Битрикс
Виртуальная машина от разработчиков Битрикс решает ту же задачу, что и Bitrix Docker - предоставляет готовое окружение. Разница лишь в том, что Docker намного удобнее, проще и легче в поддержке.

Как только вы запускаете виртуалку, Docker сервисы автоматически стартуют, т.е. вы запускаете свой минихостинг для проекта и он сразу доступен.

Если у вас появится новый проект, вы можете добавить его через опцию ```I```. И не требуется плодить виртуальные машины если вы работаете на виртуальной машине в windows.

P.S.
Виртуальная машина от разработчиков Битрикс на Apache, а у нас на Nginx, а он работает намного быстрее и кушает меньше памяти.

## Планы
В данной сборке я использую Elastic Search. В планах доделать:
1. Выбор И/ИЛИ (elastic search, sphinx) в установщике.
2. Генерация файлов индекса для sphinx для сайтов.
3. Настройка smtp отпарвки писем, как для каждого сайта, так и для общей машины.

## Поддержите проект
Не имею корыстных целей и посему выкладываю инструмент на всеобщее пользование. Если у вас есть возможность и желание поддержать покупкой кофе, чтобы мне ночами удобно было делать такие инструменты, то, буду благодарен.

https://yoomoney.ru/to/410011111919168
