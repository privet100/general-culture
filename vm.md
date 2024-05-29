### Backup
   + сжать большие файлы
   + сохранить файлы где хочешь
   + на школьном маке: создать папку с таким же названием и по тому же пути в goinfre, скачать туда файлы, распаковать, запустить virtualbox (не менять конфигурацию в virtualbox)
   + на другом компе: скачать и разархивировать конфигурацию, virtualbox - Инструменты - зелёный плюсик "Добавить", указав папку с файлом Debian.vbox и прочими файлами

### Error
```
"Could not open the medium '/mnt/nfs/homes/akostrik/sgoinfre/Inception/Inception.vdi'.
VDI: error reading pre-header in '/mnt/nfs/homes/akostrik/sgoinfre/Inception/Inception.vdi' (VERR_IS_A_DIRECTORY).
VD: error VERR_VD_VDI_INVALID_HEADER opening image file '/mnt/nfs/homes/akostrik/sgoinfre/Inception/Inception.vdi' (VERR_VD_VDI_INVALID_HEADER).
Result Code: 
NS_ERROR_FAILURE (0x80004005)
Component: 
MediumWrap
Interface: 
IMedium {ad47ad09-787b-44ab-b343-a082a3f2dfb1}" 
```
* Install HxD Hex Editor to open .vdi file as streams of bytes (http://download.cnet.com/HxD-Hex-Editor ... tag=button)
* pre-header = the first 72 bytes (http://forums.virtualbox.org/viewtopic.php?t=52)
* create a new vm `test.vdi` with all default options
* Open `test.vdi` and copy its first 72 bytes
* overwrite them in the invalid vdi file that you want to fix
* reboot your vm using the modified vdi

### Получить доступ к какому-либо сервису хостовой машины
* по умолчанию
  + сетевые интерфейсы VM и хостовой машины изолированы
  + нет возможности получить доступ к сервисам (веб-серверу, ssh, ...) гостевой системы из основной 
  + технология NAT
    - настройка сети для виртуальной машины
    - с VM просматривать интернет-сайты и выполнять различные действия в сети
    - с VM не будете видеть хостовый комп и вашу домашнюю сеть
    - у VM виртуальная сеть, в которой только она одна
  + в Вашей системе есть "Виртуальный" сетевой адаптер "VirtualBox Host-Only Enthernet Adapter" (win) 
* 1 способ
  + пробросить пробросить порт сервиса в хостовую систему через Port Forwardign Rules
  + у вас должен быть внешний белый ip
  + опция NAT
  + Имя
  + Протокол - протокол, по которому работает сервис, например, tcp
  + Адрес хоста - адрес на хостовой машине, подключения к которому будут направляться на виртуальную машину, можно написать 127.0.0.1 или ip в локальной сети
    - нельзя указывать 127.0.0.1, на этом адресе вы сможете получать и передавать пакеты только на него же, даже на виртуалку не сможете, потому что на виртуалке свой собственный 127.0.0.1
    - `ifconfig` узнать адрес хоста в локальной сети
  + Порт хоста - подключения к которому нужно перенаправлять на VM
  + Адрес VM - на который нужно направлять подключения
    - Если вы хотите, что бы весь интернет смог подключиться к вашему виртуальному серверу, то оставьте IP гостя пустым
  + Порт гостя - на который будут перенаправлены подключения с этого порта
    - порт с которого вы перенаправляете и на который перенаправляете не должны совпадать
    - если перенаправлять с порта 80 на 80, то работать не будет
    - например 8080 на 80 ок
    - вместо таблицы Port Forwardign Rules это можно сделать командой `VBoxManage modifyvm "Имя машины" --natpf1 "rulename,tcp,127.0.0.1,8080,,80"`
* 2 способ
  + нет внешнего белого ip
  + запустить сеть виртуальной машины в режиме "Сетевой мост" вместо NAT
  + виртаулку автоматически видно всем из сети
  + на виртуалке нет интернета
  + в Port Forwardign Rules айпи = 0.0.0.0
* 3 способ
  + настроить на NAT
  + поставить мост
  + `ifconfig` узнать IP виртуальной машины
* 4 способ
  + тип подключения в настройках сетевого адаптера "Виртуальный адаптер хоста"
  + если к виртуалке нужен доступ только с хоста, по умолчанию виртуалке присваивается адрес из сети 192.168.56.0/24, на хосте при этом 192.168.56.1
  + сможете обращаться к ВМ по ее IP адресу
  + выдать виртуальной машине отдельный IP адрес
  + можно обращаться к любым ее портам и использовать любые сетевые возможности
  + без проброса портов

`wget http://localhost/index.html` проверить без браузера