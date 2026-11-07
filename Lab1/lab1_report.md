University: ITMO University<br>
Faculty: FTMI<br>
Course: Облачные платформы как основа технологического предпринимательства<br>
Year: 2025/2026<br>
Group: U4125<br>
Author: Shishkina Sofya Anatolievna<br>
Lab: Lab1<br>
Date of create: 07.11.2026<br>
Date of finished:


# Лабораторная работа №1

Работа по обзору Google Cloud и исследованию основных сервисов.

# Цель работы

Ознакомиться с основными возможностями и преимуществами облачной платформы Google Cloud.

# Ход работы

- Зайдите в вкладку IAM, создайте service account с ролью Storage Admin.

> В качестве нейминга используйте первую букву имени и фамилию, а также добавьте -sa-lab1. Например Ivan Filianin = ifilianin-sa-lab1.

![lab1-1](images/lab1-1.png)

![lab1-2](images/lab1-2.png)

- Создать минимальный compute engine (виртуальную машину) с Machine type e2-micro в режиме spot.

> В качестве нейминга используйте первую букву имени и фамилию, а также добавьте -vm-lab1. Например Ivan Filianin = ifilianin-vm-lab1.

![lab1-3](images/lab1-3.png)

![lab1-4](images/lab1-4.png)

Также прописан Service Account sshishkina-sa-lab1 с `Access scope` "Allow full access to all Cloud APIs:":

![lab1-5](images/lab1-5.png)

С помощью утилиты gcloud найдите бакет lab1-bucket-itmo и скопируйте 3 файла в локальную папку на VM. Используя команду ls -lah отобразите что эти файлы хранятся у вас на VM.

Для этого октроем подключение по `ssh` в браузере:

![lab1-6](images/lab1-6.png)

![lab1-7](images/lab1-7.png)

Для просмотра файлов в бакете воспользуемся командой `gcloud storage ls gs://BUCKET_NAME`:

```
gcloud storage ls gs://lab1-bucket-itmo/
```

![lab1-8](images/lab1-8.png)

Видим 3 файла. Чтобы скопировать их в папку, создадим с помощью `mkdir` папку, например, `papka`:

![lab1-9](images/lab1-9.png)

Для копирования файлов выполним команду копирования с помощью `gcloud`:

```
gcloud storage cp gs://lab1-bucket-itmo/pic1.jpg gs://lab1-bucket-itmo/pic2.jpg gs://lab1-bucket-itmo/pic3.jpeg papka
```

![lab1-10](images/lab1-10.png)

Первые два файла скопировались успешно. При копировании третьего возникла ошибка: вместо `pic3.jpeg` было указано `pic3.jpg`. После исправления имени третий файл также был скопирован.

Теперь перейдём в папку `papka` с помощью `cd` и отобразим файлы в этой директории:

![lab1-11](images/lab1-11.png)

Видим все 3 файла. Каталоги `.` (текущий каталог) и `..` (родительский каталог - `~`) видны из-за флага `-a` в команде `ls`.

- Поменяйте права доступа для вашего service account с `Storage Admin` на `Compute Viewer`, попробуйте повторить пункт с копированием данных, сделать выводы.

Сделаем это в разделе `IAM & Admin`:

![lab1-12](images/lab1-12.png)

Попробуем ещё раз скопировать файл (скопируем 1 файл):

```
gcloud storage cp gs://lab1-bucket-itmo/pic1.jpg papka
```

![lab1-13](images/lab1-13.png)

Копировать всё равно даёт. Подождем некоторое время, чтобы настройки применились.

Прождав 10 минут и повторив копирование получаем тот же эффект.

Откроем `Cloud Storage` -> `Buckets` -> `lab1-bucket-itmo` -> `Permissions`:

![lab1-14](images/lab1-14.png)

Видим, что на уровне бакета настроены публичные разрешения, которые сохраняют возможность чтения независимо от удалённой роли service account. Роль Compute Viewer сама по себе не предоставляет прав на чтение объектов Cloud Storage.

- Удалите за собой все созданные сервисы.

Для этого удалим машину с названием `sshishkina-vm-lab1` в разделе `https://console.cloud.google.com/compute/instances`.

Также удалим сервисный аккаунт `sshishkina-sa-lab1` в разделе `https://console.cloud.google.com/iam-admin/serviceaccounts`.

# Итог работы

В ходе работы созданы сервисный аккаунт и виртуальная машина Compute Engine. С помощью Google Cloud CLI три файла скопированы из Cloud Storage на VM. После замены роли Storage Admin на Compute Viewer скачивание осталось доступным благодаря публичным разрешениям бакета.

