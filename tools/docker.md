![docker-php-16-638](https://github.com/akostrik/general-culture/assets/22834202/7305c712-59d9-44e5-b67d-6ea0283d8b06)

## Linux Containers (LXC) 
* method for running multiple isolated Linux systems (containers) on a control host using a single Linux kernel
* the cgroups functionality of the kernel: limitation and prioritization of resources (CPU, memory, block I/O, network, etc.) without virtual machines
* the namespace isolation functionality of the kernel: isolation of an application's view of the operating environment (process trees, networking, user IDs, mounted file systems)
* provides an isolated environment for applications
* docker container != LXC (хоть используют те же технологии ядра Linux)
* OS-level virtualization 

## Union File System (UnionFS)
* вспомогательная файловая система, производящая каскадно-объединённое монтирование других файловых систем
* файлы и каталоги изолированных файловых систем прозрачно перекрываются, формируя единую связанную файловую систему
* каталоги, которые имеют тот же путь в объединённых ветвях, будут совместно отображать содержимое в объединённом каталоге
* есть приоритет одной ветви над другой, на случай, когда обе ветви содержат файл с идентичным именем
  + файловая система, которая работает создавая уровни, делая ее очень легковесной и быстрой
  + технология для хранения слоев
  + для сочетания уровней в один образ
* одна из причин, по которой docker легковесен — использование таких уровней
  + когда вы изменяете образ, например, обновляете приложение, создается новый уровень
  + без замены всего образа или его пересборки, только уровень добавляется или обновляется
  + раздается только обновление

## Docker 
* a set of platform as a service (PaaS) products
* a software that can package your application, its dependencies, system tools, system libraries, settings in a single virtual container
* инструмент объекто-ориентированного проектирования (дизайна) (вместо последовательно-процедурного вызова команд)
  + инкапсуляция, наследование, полиморфизм компонент => независимые компоненты: приложение + web-сервер + БД + сист библиотеки
  + конфигурация nginx ≈ часть веб-приложения
  + отделить приложение от инфраструктуры (независимо от версии PHP или Python на сервере, etc)
    - приложение внутри контейнера не имеет доступа к основной ОС
    - не важно, в каком окружении оно будет работать, есть ли там нужные зависимости и настройки
* packages an application and its dependencies in a container
* выкладывания контейнеров на ваши продакшены, как в дата центры так и в облака
* переносить приложение на другие операционные системы с поддержкой cgroups
* управлять жизненным циклом и сетевым окружением контейнеров
* развертывание
  + reproduces a run-time environments, OS-level virtualization, Linux дистрибутивы, установочные процессы
  + на уровне операционной системы
  + запускать в облачной инфраструктуре
  + упростить
* медленнее, чем запуск приложения на физическом сервере
* работает непосредственно в ОС => возможно внедрение зловредного кода в контейнеры и проникновение в ОС
* VM vs docker  
  + "масштабирование" = увеличение числа экземпляров приложения, чтобы обрабатывать больше запросов
    - Горизонтальное (scale out): добавляются новые экземпляры приложения/сервиса, например, запустить ещё несколько контейнеров/VM с тем же приложением
    - Вертикальное (scale up): увеличиваются ресурсы одного экземпляра (оперативка, процессоры и т.д.)
  +
    | VM                                                                                                                       | container                                                                      |
    | ------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------| 
    | создают полноценную виртуальную среду с ОС, эмулируют аппаратные ресурсы (нужно много memory, space, процессорное время) | не требуют полной загрузки ОС, используют одно ядро основной ОС, делят ресурсы |
    | volumes storage cannot be shared across the VM’s                                                                         | volumes storage can be shared across the host and the containers               |
    | difficult to scale up                                                                                                    | quick and easy, одной командой (docker-compose scale) или оркестраторами       |
* PID 1
  + PID 1 = systemd, mais dans un container c’est différent, il ne peux pas y avoir de systemd
  + первый процесс, который запускается в контейнере
  + отвечает за запуск и управление процессами внутри контейнера
  + другие процессы внутри контейнера получают PID от PID 1
  + если PID 1 завершится, контейнер остановится
  + обрабатывает системные сигналы (SIGTERM, ...)
    - если ваш основной процесс (например, веб-сервер) работает под PID 1, он должен корректно обрабатывать такие сигналы для правильного завершения работы
  + очищает дочерние процессы, чтобы избежать зомби-процессов
  + остальные настроены так, чтобы PID 1 их родитель
  + CMD в Dockerfile => Docker назначит PID 1 вашему основному процессу
  + не используйте скрипты оболочки в качестве PID 1
    - могут быть проблемы с управлением процессами
    - можно использовать, если только tini или dumb-init служит в качестве PID 1 и обрабатывает системные сигналы
  + `daemon off` для сервисов, которые по умолчанию запускаются в фоновом режиме
    - чтобы процесс оставался основным процессом с PID 1 и не запускался в фоне
  + si le service exit de facon anormale, le container doit pouvoir se restart (d'ou l'interet du PID 1)
    - `top || ps` vérifier que notre service à l'intérieur de notre container tourne bien en tant que PID 1 

## Как устроен
![Screenshot from 2024-04-06 01-09-43+](https://github.com/akostrik/general-culture/assets/22834202/4b0ea467-2d6b-45a7-b1f6-c9e22093b2dc)
![Screenshot from 2024-03-30 00-55-26](https://github.com/akostrik/general-culture/assets/22834202/3ea7709f-8248-4980-ad54-2b242f9e9b2a)
![Screenshot from 2024-06-08 03-12-20](https://github.com/privet100/general-culture/assets/22834202/9c28a3dd-b67d-4244-9044-478d598fc69f)

* the resource isolation features of the Linux kernel (cgroups, kernel namespaces) + a union-capable file system => to allow containers to run within a single Linux instance, avoiding the overhead of virtual machines
* микросервисная архитектуря
  + изменения в одной компоненте не затронут остальную систему
* контейнер в изолированной среде, не влияющей на основную операционную систему
  + виртуальная среда запускается из ядра основной ОС (в оличие от VM)
  + не создается виртуальное железо (в оличие от VM)
  + containers share the services of a single OS kernel => fewer resources than virtual machines  
* позволяет использовать на одном хосте различные версии языков, библиотек, etc
* libcontainer библиотека, абстрагирующая виртуализационные возможности ядра Linux
  + to use virtualization facilities provided directly by the Linux kernel, in addition to using abstracted virtualization interfaces via libvirt, LXC and systemd-nspawn
* проверенные технологии ядра, минимум своих решений
* a client-server application
* `/var/lib/docker/overlay2` writable layers, the various filesystem layers for images and containers
  + `/var/lib/docker/overlay2/eceb7b667587c3cc2a08d7c970eae723fdd8981b7a7580db19587434123a2681`
* `/var/lib/docker` your images, containers, local named volumes

## API
* REST API
* HTTP API
* specifies interfaces that programs can use to talk to the Docker daemon
* сервер ожидает запросов через API от Клиента и выполняет команду
* client uses API to communicate with the Engine
  + everything the Docker client can do can be done with the API
  + most of the client's commands map directly to API endpoints (e.g. `docker ps` is `GET /containers/json`)
* клиент и сервер могут:
  + работать на одной системе (общаются через REST API)
  + можно подключить клиент к удаленному демону docker (общаются через сокет)

## server daemon `dockerd`= Docker Engine ?
* фоновый процесс (демон)
* следует инструкциям из Dockerfile и Docker-compose.yml
* управляет:
  + docker-объектами
  + процессами докера: скачивание и создание образов, собирает образ, запуск, остановка контейнеров
  + коммуникацией между контейнерами
* `dockerd` запуск 

## client `docker`
* интерфейс к Docker
* command-line или графический
* получает команды от пользователя (создают контейнеры, управляют ими, создаёт/управляет/запускает контейнеризованные приложения)

## image (object)
* an entitie used to assemble an application
* a combined package, which contains everything that’s needed to run your application
* a standalone, executable software package which runs reliably on any server — Linux, Windows, macOS, public cloud or private cloud
* can be used to run the application as an isolated process
* неизменяемый файл, из которого можно неограниченное количество раз развернуть контейнер
* исполняемый пакет = код + среда выполнения + библиотеки + переменные окружения + конфигурационные файлы
* говорит docker-у, что находится в контейнере, какой процесс запустить, когда запускается контейнер, другие конфигурационные данные
* состоит из набора уровней, из слоёв-изменений
  + слои = папки, которые лежат в /var/lib/docker/aufs/diff/
  + в основе родительский образ, как правило, содержащий ОС
  + docker переиспользует готовые шаблоны для создания новых образов
  + когда docker скачивает образ, ему нужны только те слои, которых у него нет
* содержит
  + метаданные предустановленной программы
  + команду, которую следует выполнить при запуске контейнера
  + параметры, передаваемые предустановленной программе
* при запуске одного образа можно создать несколько контейнеров
* Образ ≈ класс в коде, контейнер ≈ объект, созданный из класса
* **композиция вместо наследования**: рядом контейнеры с разными сервисами + адаптер + transfer object + свяжу их через конфиг
* **наследование** для расширения существующего функционала в рамках инкапсуляции (для подключения к php расширений, системных библиотек)
* **An index** manages user accounts, permissions, search, tagging, etc that's in the public web interface
* **A registry** stores and serves up the actual image assets, delegates authentication to the index

## Container (object)
![Screenshot from 2024-05-13 14-33-01](https://github.com/privet100/general-culture/assets/22834202/028daefa-01ba-4f47-b948-fcbece2bce91)
* 1 container = 1 service = 1 развёрнутое и запущенное приложение (database, web server, web framework, test server, execute big data scripts)
* содержит все для работы приложения (системные программы, библиотеки, код, среды исполнения, настройки)
* изолирован от хостовой ОС и других контейнеров 
  + файловая система, имя хоста, пользователи, сетевая среда, процессы - отделены от остальной части системы
* технологии ядра Linux для изоляции контейнеров:
  + namespaces: контейнер работает в отдельном пространстве имен (process trees, network, user IDs, mounted file systems), с ограничением доступа к другим пространствам => параллельно не пересекающиеся друг с другом иерархии процессов, пользователеи, сетевых интерфейсов
    - PID Process ID изоляция иерархии процессов
    - NET Networking изоляция сетевых интерфейсов
    - PC InterProcess Communication управление взаимодействием между процессами
    - MNT Mount управление точками монтирования
    - UTS Unix Timesharing System изоляция ядра и идентификаторов версии
  + Контрольные группы (Cgroups): provide resource limiting for memory and CPU, контроль над распределением системных ресурсов, используемых контейнером
  + Средства управления привилегиями (Linux Capabilities): контейнеры запускаются с ограниченным набором привилегий
* создает уровень для чтения/записи сверху образа (используя unionFS)
  + хранящий временные данные, которые уничтожаются после удаления контейнера
  + их можно сохранить с помощью volumes или монтирования папки с хоста к контейнеру, изменения на хосте сразу отражаются в контейнере
* docker клиент говорит (с помощью программы docker или с помощью REST API) демону запустить контейнер
  + запускает клиент 
  + скачивает образ ubuntu
  + создает контейнер
  + инициализирует файловую систему
  + монтирует read-only уровень
  + инициализирует сеть/мост: создает сетевой интерфейс, который позволяет docker-у общаться хост машиной
  + находит и задает IP адрес
  + запускает приложение
* `docker run -i -t ubuntu /bin/bash`  
  + `docker` запускается клиент   
  + скачивает образ ubuntu
  + запускает новый контейнер
  + инициализирует файловую систему и монтирует read-only уровень
  + инициализирует сеть/мост: создает сетевой интерфейс, который позволяет docker-у общаться хост машиной
  + находит и задает IP адрес
  + запускает приложение
  + запускает команду `/bin/bash`  
* запускается без гипервизора
* контейнерам можно назначать лимиты ресурсов
* communicate with each other through channels

## Dockerfile (object, an entity used to assemble an application)
![Screenshot from 2024-03-29 23-08-11](https://github.com/akostrik/general-culture/assets/22834202/d92caf9d-11c3-4446-88aa-1eed25bd76f3)
* docker
  + считывает инструкции
  + собирает и возвращает образ, builds a Docker image  
  + набор софта, который мы хотим развернуть
  + настройки контейнера (порты, переменные окружения, ...)
  + добавление файла или директории
  + создание переменной окружения
  + запуск команды, что запускать, когда запускается контейнер этого образа
* каждая команда создаёт новый слой образа ≈ система снапшотов сохраняет изменения в виртуальной машине
* RUN создаёт статичный слой, изменения внутри которого записываются в образ, ничего не вызывают
* финальный Docker-образ = объединение всех слоев в один
* ENTRYPOINT 
  + un process par défaut, point d'entrée, главный процесс контейнера
  + не зависит от аргументов запуска контейнера
  + l'on ne peut pas l'override
  + гарантирует, что контейнер всегда запустит основное приложение (веб-сервер, базу данных и т.д.), независимо от переданных аргументов
  + НЕ записывает изменения в образ
  + faudrait que j’accède au bash du container pendant qu’il tourne et ça implique de demarrer le php-fpm et/ou le nginx soit même si je fait un CMD alors que si je fait un ENTRYPOINT je pense qu’il executera quand même et j’aurais pas à le faire enfin
  + `ENTRYPOINT ["nginx", "-g", "daemon off;"]`  
  + MariaDB:
    ```
    ENTRYPOINT ["docker-entrypoint.sh"] // подготавливает бд и запускает mysqld
    CMD ["mysqld"]
    ````
  + WordPress работает поверх PHP и веб-сервера (Nginx с PHP-FPM)
    ```
    FROM wordpress
    ENTRYPOINT ["docker-entrypoint.sh"] // подготавливает среду для WordPress
    CMD ["apache2-foreground"]          // запускает основной процесс веб-сервера
    ```
* CMD
  + is run once you start the container
  + définir la commande de démarrage par défaut du container
  + une commande par défaut que l'on peut override
  + à aucun moment durant le build la commande par défaut ne va être exécuté
  + НЕ записывает изменения в образ
  + можно использовать для передачи аргументов по умолчанию
* https://github.com/dnaprawa/dockerfile-best-practices
* 4 способа передамть контейнеру переменные окружения из .env   
  + 0) напрямую в Dockerfile не получится, так как Docker не поддерживает нативную загрузку .env файлов при сборке
  + 1) на этапе запуска контейнера: `docker run --env-file .env my_image` (рекомендуется)
  + 2) во время выполнения контейнера через docker-compose (рекомендуется): 
    ```
    services:
      app:
        image: your_image
        environment:
          - ENV_VAR1=${ENV_VAR1}
          - ENV_VAR2=${ENV_VAR2}
    ```
    запустите контейнер через docker-compose up
  + 3) вручную перенести значения в Dockerfile `ENV ENV_VAR1=value1`
  + 4) на этапе сборки образа с помощью docker build через Dockerfile:
    ```
    ARG ENV_VAR1
    ENV ENV_VAR1=${ENV_VAR1}
    ```
    Команда для сборки: `export $(cat .env | xargs) && docker build --build-arg ENV_VAR1=$ENV_VAR1 --build-arg ENV_VAR2=$ENV_VAR2 -t my_image .`  
    `export $(cat .env | xargs)` загружает переменные из файла .env в окружение оболочки, теперь ENV_VAR доступна в текущей оболочке, как если бы вы прописали их вручную  
    `cat .env` читает .env  
    `xargs` преобразует вывод cat в строку аргументов (ENV_VAR1=value1 ENV_VAR2=value2), чтобы передать её в export  
    `export $(...)` устанавливает переменные окружения в текущей сессии терминала
    `$()` позволяет взять результат выполнения команды внутри скобок и передать его как аргументы для export
    `docker build --build-arg ENV_VAR1=$ENV_VAR1 --build-arg ENV_VAR2=$ENV_VAR2 -t your_image .` передаёт переменные на этапе сборки Docker-образа, чтобы Docker мог их использовать
    `docker build` инициирует сборку Docker-образа  
    `--build-arg ENV_VAR=$ENV_VAR` передает значения переменной окружения, установленной через export, на этапе сборки Docker-образа, а Docker использует эти аргументы внутри Dockerfile при помощи ARG  
    `-t` задаёт тег для создаваемого Docker-образа, в данном случае образ будет называться my_image  

## Docker Compose CLI `docker-compose.yml` (a tool)
* to define and run multiple Docker containers as a single application, manage the whole lifecycle of your application:
  + start, stop, rebuild, configures services, networks, volumes, building images, ...
* пример: веб-сайт
  + первый сервис (контейнер) отвечает за функционирование сайта
  + второй сервис (контейнер) отвечает за базу данных для авторизации пользователей
* пример: веб-проект, состоящий из двух сайтов
  + первый позволяет создать интернет-магазин
  + второй для поддержки клиентов
  + оба сайта подключены к общей базе данных
* simplifie tes commandes docker
* в большинстве случаев лучше открывать порты в docker-compose.yml, а не в Dockerfile:
  + В docker-compose.yml можно легко изменить порты или создать несколько конфигураций для разных сред (разработка, тестирование, продакшн), не трогая образ Docker
  + Открытие портов в Dockerfile (EXPOSE) не делает их доступными извне контейнера, оно лишь указывает, какие порты внутри контейнера используются
  + В docker-compose.yml легко настраиваются сети и связываются контейнеры между собой
* nginx в Dockerfile:
  + для небольших проектов или автономных образов
  + для единичных контейнеров или образов	
  + образ должен быть самодостаточным и запускать Nginx автоматически при старте
  + образ Nginx должен быть автономным, без docker-compose
  + образ может быть развернут в различных окружениях без дополнительных конфигураций
  + требуется пересборка образа для изменений	
  + сложно масштабировать вручную	
  + нет встроенной поддержки сети контейнеров
* nginx в docker-compose:
  + для проектов с несколькими контейнерами и разными окружениями, многоконтейнерных сред, множество сервисов, которые должны взаимодействовать (например, Nginx + backend-сервис + база данных)
  + больше гибкости и удобства в управлении
  + конфигурировать, масштабировать и связывать несколько сервисов (контейнеров) в одной среде
  + гибкое управление, масштабирование, настройка среды на уровне нескольких контейнеров
  + менять конфигурацию без пересборки образа (например, перемаппить порты или подключить другие сервисы)
  + позволяет изменять команду, запускаемую контейнером, без пересборки образа, это удобно для тестирования разных команд или конфигураций
  + легко масштабировать через docker-compose up --scale
  + создание сетей для взаимодействия контейнеров

## A docker-network (object, an entitie used to assemble an application)
* a virtual software defined network that connects Docker containers
* allows containers to communicate with each other and the outside world
* provides an additional layer of abstraction over the underlying network infrastructure
* networking subsystem is pluggable, using drivers
* several drivers by default:
  + bridge (by default): an application in a container communicates with other containers on the same host
  + host: remove network isolation between the container and the Docker host, use the host's networking directly
  + overlay: connect multiple Docker daemons together
  + ipvlan: gives users total control over both IPv4 and IPv6 addressing
  + macvlan: allows to assign a MAC address to a container, making it appear as a physical device on your network
  + none: isolates a container from the host and other containers
  + third-party network plugins
* several types of networks:
  + Bridge: A bridge network is the default network type when you install Docker. It allows containers to communicate with each other and the host machine, but provides no access to the outside world.
  + Host: A host network uses the host machine's network stack and provides no isolation between the host and the container.
  + Overlay: An overlay network allows containers running on different Docker hosts to communicate with each other.
  + Macvlan: A Macvlan network allows a container to have its own IP address on the same subnet as the host machine.
* using the host network
  + a standalone containers binds directly to port 80 to the Docker host's network
  + as if the nginx were running directly on the host, by the host networking driver
* if you open docker to listen on the network
  + the docker API ports are frequently scanned on the internet, and you will find malware installed
  + ≈ a telnet server with root logins allowed without a password
  + configure mutual TLS between client and server
* окрыть с хостовой машины сайт, который на VM
  + VM имеет внешний сетевой интерфейс, подключенный к виртуальному коммутатору внутри хостовой системы, по этому адресу и стучитесь
  + пробросить еще один интерфейс как бридж и разрешить ему форвардить пакеты из сети 10.0.2.0 в вашу подсеть, после чего прописать маршруты на обеих сторонах
  + удалить стандартный NAT интерфейс в virtualbox и добавить вместо него bridge, чтоб ловил адрес в вашей сети, перенастроить nginx и апач на новый ip полученый из вашей сети
* `netstat -ntpl`  
* `docker network list`
* `docker network inspect myNetwork`
* `brctl show`
* `iptables -t nat --list`
* `ifconfig`
* `ip addr show` network interfaces
* `docker run -p 80:80 --name myCont -d` прокинуть порт и переназначить его снаружи 
* https://docs.docker.com/network/network-tutorial-standalone/
 
## Docker Volume (a tool, an object?)
* папка хоста, примонтированная к файловой системе контейнера
* отделены от контейнеров
* можно размещать в облаке 
* можно шифровать
* different drivers: local storage on your docker host by default, NFS volumes, CIFS/Samba shares, device-level block storage adapters
* Named Volumes
  + когда нужно долговременное хранение данных и их использование между разными контейнерами или после перезапусков, типичный выбор для хранения бд или логов
  + `volumes: mydata:/usr/share/nginx/html`: том `mydata` монтируется в контейнере по /usr/share/nginx/html
  + `volumes: mydata:`: создаётся автоматически
  + могут использоваться разными контейнерами
  + в /var/lib/docker/volumes на хосте вне контейнера (по умолчанию)
* Bind Mounts
  + `volumes: - ./myapp:/usr/share/nginx/html`
  + любая директория или файл на хостовой машине напрямую монтируются в контейнер
  + моментальная синхронизация данных: изменения в ./myapp на хосте будут сразу же отображаться внутри контейнера по пути /usr/share/nginx/html
  + полезно для разработки или когда нужно хранить данные в специфическом месте на хосте
* Anonymous Volumes
  + когда данные нужны только на время работы контейнера или для кэширования
  + хранятся до удаления контейнера
  + `volumes: - /usr/share/nginx/html` (контейнерный путь)
  + в /var/lib/docker/volumes
* хранение в оперативной памяти (tmpfs mounts или npipe mounts)
  + Tempfs Mount отменяют сохранение информации после ликвидации контейнера
* `docker run -it -v demo_volume:/data ubuntu:22.04`
  + `it` attaches your terminal to the container
  +  a volume `demo_volume` is mounted to `/data` inside the container

## Docker Swarm (a tool)
* a set of cooperating daemons that communicate through the Docker API
* turns a group of Docker engines into a single virtual Docker engine, собрать несколько узлов в единую виртуальную систему Docker и управлять ею
* deprecated
* nomad est plus répandu que swarm mdr

## Kubernetes = K8s
* программное обеспечение для оркестровки контейнеризированных приложений
  + автоматизации их развёртывания, масштабирования и координации в условиях кластера
* возможна поддержка технологий аппаратной виртуализации
* automatiser le déploiement, la montée en charge et la mise en œuvre de conteneurs d'application sur des grappes de serveurs
* fonctionne avec toute une série de technologies de conteneurisation (Docker, rkt, ...)
* souvent utilisé avec Docker
* compliqué

## Настройки
* `/etc/default` DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4 -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"
* `/etc/init.d/docker`
* `/etc/init/docker.conf` DOCKER_OPTS="-H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"
* `/etc/systemd/system/docker.service.d`
* `/etc/docker/daemon.json`
* `/lib/systemd/system/docker.service` ExecStart=/usr/bin/docker daemon -H fd:// -H tcp://0.0.0.0:
* never edit the service script directly, use systemctl edit docker.service

## Инспектировать
* контейнеры и images
  + `docker ps`, `docker ps -a`, `docker ls`, * `docker-compose ps -p` список контейнеров
  + `docker container list`   
  + `docker-compose logs`, `docker logs nginx`, `docker logs` логи контейнеров  
  + `docker images`, `docker image ls` просмотреть список доступных локально образов   
  + `docker image inspect` подробнее рассказывает о выбранном контейнере  
  + `/var/lib/docker/aufs/diff/` файлы контейнеров (если использует драйвер AUFS)  
  + `/var/lib/docker/aufs/diff/` образы  
  + `ls /var/www/public` the contents of the container’s /var/www/public   
* тома и сети
  + 'docker network ls' (!)
  + `docker network inspect myNetwork`  
  + 'docker volume ls' (!)
  + 'docker volume inspect wordpress', 'docker volume inspect mariadb' (!)
    - the result contains '/home/akostrik/data/'
    - your volumes are available in `/home/akostrik/data` folder of the host machine
* `systemctl status docker` служба docker   
* `cat /var/lib/docker/repositories | python -mjson.tool` list of the repositories on your host  
* `ls -al /var/lib/docker/graph`  
* `/var/lib/docker/containers/` служебная информация  
* `/etc/init.d/docker status`   
* `/etc/resolv.conf`  
* `docker exec` для выполнения команд внутри контейнера 
* `docker exec -it <container_id_or_name> /bin/bash` посмореть папку внутри конейнера для контейнеров Debian или Ubuntu
* `docker exec -it <container_id_or_name> /bin/sh` посмореть папку внутри конейнера в контейнерах Alpine или др легковесных дистрибутивов

## Начать новую жизнь
* ```
  docker stop $(docker ps -qa)              # пытается остановить контейнер, отправив SIGTERM, если долго нет ответа SIGKILL   
  docker kill $(docker ps -qa)         # посылает SIGKILL, выключает контейнер, игнорируя сохранение данных  
  docker rm $(docker ps -qa)                # удалить выключенный контейнер  
  docker rmi -f $(docker images -qa)
  docker volume rm $(docker volume ls -q)
  docker network rm $(docker network ls -q)
  docker system prune --all --force --volumes # cleanup unused containers, images (doesn't delete running containers)
	docker network prune --force
	docker volume prune --force
  docker image prune --all                  # remove unused images   
  /usr/sbin/service docker stop
  /usr/sbin/service docker start
  /etc/init.d/docker restart                # перезапустить демон  
  systemctl restart docker.service
  systemctl daemon-reload
  ```
* ```
  systemctl stop docker
  rm -rf /var/lib/docker  # Restart the engine in a completely empty state + lose all images, containers, named volumes, user created networks, ...
  systemctl start docker
  ```
## Прочее
* не надо `tail -f` для отслеживания логов контейнера, потому что
  + `docker logs` собирает и отображает логи через stdout 
  + системы ELK Stack, Prometheus, Grafana собирают и обрабатывают логи
  + `tail -f` позволяет просматривать файлы напрямую
  + нагрузка на систему
* не запусать процессы в фоновом режиме, потому что  
  + каждый контейнер / сервис изолирован и выполняет одну задачу / процесс
  + если основной процесс завершается, а фоновый продолжает работать, то контейнер завершает работу 
  + `docker logs` собирает и отображает логи в реальном времени, что не всегда доступно для фоновых процессов
  + утечки памяти или использование ресурсов могут остаться незамеченными в фоне
* не  использовать `network: host`
  + контейнеры получат прямой доступ к сети и портам хостам
    - это нарушит сетевую изоляцию
    - может привести к конфликтам с другими сервисами на хосте (например, контейнер попытается использовать порт 80, а на хосте уже запущен веб-сервер на этом порту)
    - вредоносный контейнер с доступом к сети хоста может получить доступ к локальным сервисам или сети хоста, что создаёт угрозу безопасности всей системы
    - docker-compose или системы оркестрации могут некорректно работать с таким контейнером
  + контейнеры могут оказаться сложными для развертывания на других хостах или в облачных инфраструктурах, где сетевые конфигурации могут отличаться
* не использовать `links` в docker-compose и `--link` в Dockerfile
  + links устарел
    - позволяет только одностороннюю связь между контейнерами (контейнер A может ссылаться на контейнер B, но контейнер B не может автоматически ссылаться на контейнер A, если это явно не указано)
    - не работает для динамических сетевых настроек
    - не поддерживает сложные сетевые топологии
    - плохо масштабируется в сложных системах
    - затрудняет поддержку и управление
  + есть эффективные и гибкие способы организации связи между контейнерами с помощью сетей и сервисов в DNS
    - контейнеры в одной сети (например, созданной через docker-compose) могут находить друг друга по имени контейнера, используя встроенную службу DNS
    - современные сетевые решения Docker (Bridge, Overlay Networks), обеспечивают масштабируемость, изоляцию и удобство в управлении
* не использовать `bash` и `sh` для запуска скрипта
  + несогласованность конфигураций
  + нарушение принципа "Infrastructure as Code" : вся инфраструктура должна быть определена в Dockerfile
  + использовать RUN, CMD, ENTRYPOINT для управления процессами внутри контейнера
  + использовать команды и конфигурации, прописанные в Dockerfile => создавать и управлять контейнерами предсказуемо и повторяемо

## Запустить несколько примеров
[Настроить VM](https://github.com/privet100/inception/blob/main/README.md)  
Проверять сайт:  
`startx                 # x-server для отрисовки графического окружения (GUI)`   
`wget http://127.0.0.1  # проверить без браузера`   

### Example 1
`docker run -it -v public:/var/www/public ubuntu:22.04   # attaches your terminal to it, public is mounted to /var/www/public in the container`
  
### Example 2: http://127.0.0.1 на VM и http://127.0.0.1:8080 на хостовой
~/**Dockerfile**:  
```
FROM nginx
```
~/usr/share/nginx/html/**index.html**:  
```
<html><body>Hello</body></html>
```  
`docker build . --tag mynginx     # BUILD image = first run, скачает образ nginx`  
`docker run -p 8080:80 -d mynginx # 80 контейнер -> 8080 хост, -d фоновый режим без привязки к текущей консоли`  

### Example 3: то же самое, но с docker-compose
~/**docker-compose.yml**:  
```
version: '3'
services:
  nginx:
    image: nginx:stable-alpine
    volumes:
      - ./public:/var/www/public          # /public VM = /var/www/public container 
      - ./nginx/conf.d:/etc/nginx/conf.d/
    restart: unless-stopped
    ports:
      - "80:80"
    container_name: myContainer
```
~/nginx/conf.d/**nginx.conf**:  
```
server {
  root    /var/www/public/html;
  location / {                     # искать файл в корне
    try_files $uri /index.html;    # if the received URI matches $uri, nginx serves it -> if fails `/index.html` (fall back) -> if fails 404 error   
    }
}
```
~/public/html/**index.html**:  тот же  
`wget http://127.0.0.1/index.html` проверить без браузера

### Example 4: https://127.0.0.1 на VM и https://127.0.0.1:4343 на хостовой 
`cd ~/project/srcs/requirements/tools/`  
`mkcert akostrik.42.fr` генерируем самоподписный сертификат  
`mv akostrik.42.fr.pem akostrik.42.fr.crt` поменять расширения, чтобы nginx их правильно читал   
`mv akostrik.42.fr-key.pem akostrik.42.fr.key`   
~/**docker-compose.yml**:   
```
version: '3'
services:
  nginx:
    image: nginx:stable-alpine
    volumes:
      - ./public:/var/www/public/
      - ./nginx/conf.d:/etc/nginx/conf.d/
      - /home/${USER}/project/srcs/requirements/tools:/etc/nginx/ssl # ключи, сертификаты # NEW
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"                                                                         # NEW
    container_name: myContainer
```
~/nginx/conf.d/**nginx.conf**:  
```
server {
  listen              80;
  listen              443 ssl;
  server_name         akostrik.42.fr www.akostrik.42.fr; 
  root                /var/www/public/html;
  ssl_certificate     akostrik.42.fr.crt;
  ssl_certificate_key akostrik.42.fr.key;
  ssl_protocols       TLSv1.2 TLSv1.3; 
  ssl_session_timeout 10m; 
  keepalive_timeout   70;
  location / {                                             
    try_files $uri /index.html;
  }
}
```
~/public/html/**index.html**: тот же  
`docker-compose down`  
`docker-compose up -d`  
В браузере: Advanced / Принять риск продолжить -> сайт загружается по ssl и доверяет самоподписному сертификату, соединение не считается безопасным  
`wget https://127.0.0.1/index.html --no-check-certificate` проверить без браузера

### Example 5: http://akostrik.42.fr и https://akostrik.42.fr на VM
`/etc/hosts`: добавляем алиас локального домена `akostrik.42.fr`   
