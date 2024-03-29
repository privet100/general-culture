* Cairo, initiation https://github.com/shramee/starklings-cairo1, https://book.cairo-lang.org/
* [C++](https://github.com/akostrik/CPP_modules_42)
* [Python](https://github.com/akostrik/Python-for-Data-Science)
* [Docker](https://github.com/akostrik/general-culture/blob/main/docker.md)
* [nginx](https://github.com/akostrik/general-culture/blob/main/nginx.md)
* [git](https://github.com/akostrik/general-culture/blob/main/git.md)
* [xserver](https://github.com/akostrik/general-culture/blob/main/xserver.md)
* [certificat](https://github.com/akostrik/general-culture/blob/main/certificat.md)
* [regular-expressions](https://github.com/akostrik/general-culture/blob/main/regular-expressions.md)
* [android](https://github.com/akostrik/general-culture/blob/main/android.md)
* [shell](https://github.com/akostrik/general-culture/blob/main/shell.md)
* [representation-of-real-numbers](https://github.com/akostrik/general-culture/blob/main/representation-of-real-numbers.md)
* [threads](https://github.com/akostrik/general-culture/blob/main/threads.md)
* [ssh](https://github.com/akostrik/general-culture/blob/main/ssh.md)
* [git](https://github.com/akostrik/general-culture/blob/main/git.md)
* [git](https://github.com/akostrik/general-culture/blob/main/git.md)
* [git](https://github.com/akostrik/general-culture/blob/main/git.md)
  
<details><summary>Tools, applications, technical details</summary>

* не запускается chrome или brave
`
rm -rf ~/.config/google-chrome/Singleton*
rm -rf ~/.config/BraveSoftware/Brave-Browser/SingletonLock
`
* не запускается vscode: `rm -rf ~/.cache // vscode` 
* редакторы кода: VScode, subl
* VS Code "editor.insertSpaces": true
* https://account.jetbrains.com/licenses  
* на студ компах Докер и веб сервер устанавливать в виртуалку, а саму виртуалку в Sgoinfre (не goinfre, которая привязана к конкретному компу, а Sgoinfre, которая работает на всех станциях и хранит файлы несколько месяцев)
* Practice the exam just like you would in the real exam https://github.com/JCluzet/42_EXAM
</details>

<details><summary>Virtualbox</summary>

* snapshot:
   + Виртуальная машина должна быть выключена
   + virtualbox, меню "снимки"
* backup:
   + сжать большие файлы
   + сохранить файлы где хочешь
   + на школьном маке: создать папку с таким же названием и по тому же пути в goinfre, скачать туда файлы, распаковать, запустить virtualbox (не менять конфигурацию в virtualbox)
   + на другом компе: скачать и разархивировать конфигурацию, virtualbox - Инструменты - зелёный плюсик "Добавить", указав папку с файлом Debian.vbox и прочими файлами
* `.vdi` file has
   + a pre-header: signatures and a format version mark, it exists as a sanity check to determine that the file is really in VDI format, and is not some random file renamed to a .vdi extension
   + the real header
   + data
* Error
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
   + Install HxD Hex Editor to open .vdi file as streams of bytes (http://download.cnet.com/HxD-Hex-Editor ... tag=button)
   + pre-header = the first 72 bytes (http://forums.virtualbox.org/viewtopic.php?t=52)
   + create a new vm `test.vdi` with all default options
   + Open `test.vdi` and copy its first 72 bytes
   + overwrite them in the invalid vdi file that you want to fix
   + reboot your vm using the modified vdi

</details>

<details><summary>Web</summary>

# Web
* Смена локального домена: сменить алиас нашего локального домена (127.0.0.1) на something.42.fr
  + `/etc/hosts`

</details>





<details><summary>Algo</summary>

# Merge-insertion sort = Ford-Johnson algorithm 
* modification of insertion sort
* takes into account this property of binary search: the maximal number of comparisons to perform a binary search on a sorted sequence is the same for $2^n$ elements and $2^{n+1}−1$ elements (looking for an element in a sorted sequence of 8 or 15 elements requires the same number of comparisons), ensures that the size of the insertion area is $2^n−1$ as often as possible
* the worst case: less comparaisons than insertion sort
* the worst case: less comparaisons than merge sort
* [Ford, Lester R., and Selmer M. Johnson. “A Tournament Problem.” The American Mathematical Monthly, vol. 66, no. 5, 1959, pp. 387–389. JSTOR](www.jstor.org/stable/2308750)
* [The Art of Computer Programming by Donald Knuth, Volume 3, section 5.3.1](https://warwick.ac.uk/fac/sci/dcs/teaching/material-archive/cs341/fj.pdf)
* [Ford-Johnson merge-insertion sort](https://codereview.stackexchange.com/questions/116367/ford-johnson-merge-insertion-sort)
* Mahmoud, Hosam M. Sorting: A Distribution Theory. John Wiley & Sons. (October 14, 2011)
* [Morwenn. “Ford-Johnson Merge-Insertion Sort”. Code Review Stack Exchange. 10 Jan. 2016](https://codereview.stackexchange.com/questions/116367/ford-johnson-merge-insertion-sort)
* https://en.wikipedia.org/wiki/Merge-insertion_sort#Algorithm  
* https://codereview.stackexchange.com/questions/116367/ford-johnson-merge-insertion-sort  
* https://github.com/decidedlyso/merge-insertion-sort/blob/master/README.md  
* https://github.com/PunkChameleon/ford-johnson-merge-insertion-sort  
* https://github.com/decidedlyso/merge-insertion-sort
* [A variant of the Ford–Johnson algorithm that is more space efficient](https://www.researchgate.net/publication/222571621_A_variant_of_the_Ford-Johnson_algorithm_that_is_more_space_efficient)
* [Improvements to the Ford-Johnson algorithm](https://link.springer.com/article/10.1007/BF01934989)
* [The Ford-Johnson Sorting Algorithm Is Not Optimal, Manacher, Glenn K. 1979](https://dl.acm.org/doi/10.1145/322139.322145)
* https://www.youtube.com/watch?v=w1QXGe295sI
</details>

<details><summary>AI</summary>

* Bard devient Gemini
* for code: copilot 10€ в месяц, 1 месяц бесплатно, связан с github  
* for code: tabnine 3 месяца бесплатно, не связан с github
</details>

<details><summary>Blokchain</summary>

* starkware  
* starknet
* onlyDust
* https://www.youtube.com/channel/UCl6oyLa4CblZRurgwZwpgPQ 
</details>

<details><summary>Starknet</summary>

* secority scaling technologie
* Starknet France: https://t.me/+LreNgBuiV2VmZjVk
* Explorer Starknet: https://voyager.online/, https://starkscan.co/
* Starknet docs: https://docs.starknet.io/documentation/
* Starknet community forum: https://community.starknet.io/
* CoreStars (le plus gros groupe Starknet dev de la planete): https://t.me/sncorestars
* OnlyDust: https://t.me/+n3C1mzmYORs5MDZk
* Only dust: https://onlydust.com
* Quelques labo proposant des stages: KSS (https://github.com/keep-starknet-strange), Kasar (https://github.com/kasarlabs), LambdaClass (https://github.com/lambdaclass), Nethermind (https://github.com/nethermindeth), Carbonable (https://github.com/carbonable-labs) etc
</details>

<details><summary>Verify a project</summary>

   + условие
   + evaluation
   + valgrind
   + warnings 
   + каждая операция - а что, если не пройдёт
   + аргументы, вводимые данные
      - INT_MAX
      - 0
      - "\0"
      - ""0
      - NULL
      - EOF
      - BUFSIZE > 0
      - "-3"
      - "03" "0"
      - --3
      - "-"
      - "21474833649"
      - 2 5 6 + 3
      - 2\ a\
      - "2 5 6+3"
      - "" ""
      - "+" "++"
      - буквы вместо чисел
      - echo "*22" | ./a.out
   + valgrind, особенно для ошибок
   + norminette в тч libft
   + header vscode ctrl alt H, vim fn+F1, Emacs ctrl + c -> ctrl + h
   + libft не подмодуль
   + убрать запрещённые функции (в т.ч. printf) в т.ч. из libft
   + удалить gitignore, mlx, чекеры, libft/readme, скрытые файлы
   + -Wall -Wextra -Werror
   + клонировать и проверить 
</details>

<details><summary>Vocabulary and little notes</summary>

* snapshot: the state of a system at a particular point in time
* GCC = GNU Compiler Collection: a compiler supporting various programming languages, hardware architectures and operating systems
* multimedia framework
   + для работы с аудио- и видеоданными
    + ядро мультимедийных приложений
    + состоит из системы плагинов (кодеков, фильтров, (де)мультиплексаторов, вывода на экран, работы с файлами и т. п.), которые можно соединить для конвейерной обработки аудио/видео потока
    + плагины Windows
        - Audio Compression Manager (ACM)
        - DirectShow — (1996) (до 1997 г. назывался Active Movie)
        - DirectX Media Objects (DMOs)
        - Media Foundation — (2007) (начиная с Windows Vista)
        - QuickTime
        - Video for Windows (VfW) — (1992)
        - Windows Media
    + плагины Mac OS X
        - QuickTime
    + плагины Linux и платформенно-независимые с открытым кодом
        - FFmpeg
        - GStreamer
        - Helix DNA
        - Phonon
        - xine
        - Media Lovin' Toolkit, `.mlt`, набор средств для приложений, работающих с видео потоками, основа систем нелинейного монтажа Kdenlive, Flowblade, OpenShot, Shotcut
        - libVLC
    + плагины Проприетарные кроссплатформенные
        - Adobe Director
        - Adobe Flash
        - Java Media Framework (JMF)
        - Microsoft Silverlight
* `mkcert` генерация самоподписного сертификата, как правило используются для локальной разработки
* **cgroups**  a Linux kernel feature that limits, accounts for, and isolates the CPU, memory, disk I/O, etc usage of a collection of processes  
**Docker Engine** software that hosts the containers  
**REST** (representational state transfer) = a software architectural style
* guides the design and development of the architecture for the World Wide Web
* defines a set of constraints for how the architecture of a distributed, Internet-scale hypermedia system, such as the Web, should behave
* emphasises uniform interfaces, independent deployment of components, the scalability of interactions between them, and creating a layered architecture to promote caching to reduce user-perceived latency, enforce security, and encapsulate legacy systems
* provides stateless, reliable web-based applications

**Alpine**
* Легковесная система
* легковесный apk вместо привычного apt
* нет полноценного bash, вместо него sh
* [свой набор репозиториев](https://pkgs.alpinelinux.org/packages "список пакетов alpine").
   
**Linux kernel namespaces**
* partitions kernel resources such that one set of processes sees one set of resources while another set of processes sees a different set of resources
* the same namespace for a set of resources and processes, but those namespaces refer to distinct resources ((process IDs, host-names, user IDs, file names, some names associated with network access, and Inter-process communication)
* resources may exist in multiple spaces 
* are a fundamental aspect of containers in Linux
* Linux system starts out with a single namespace of each type, used by all processes, then processes create additional namespaces and join different namespaces
* изоляция групп процессов друг от друга
* в Linux шесть стандартных пространств имён: mnt, IPC, net, usr, pid, uts
* у каждого контейнера своё пространство имён и процессы, запущенные внутри этого пространства имён, контейнер не имеет доступа к чему-либо снаружи его пространства имён

**`init`**
* /sbin/init the first process started during booting of the OS by the kernel
* continues running until the system is shut down
* is a daemon
* is the direct or indirect ancestor of all other processes
* automatically adopts all orphaned processes
* PID 1
* загружает систему: запускаются стартовые сценарии, которые выполняют проверку и монтирование файловых систем, запуск необходимых демонов, настройку ядра (в том числе загрузку модулей ядра согласно установленному оборудованию, настройку IP-адресов, таблиц маршрутизации, ...), запуск графической оболочки
* основная информация для загрузки в /etc/inittab
* is in /sbin/init
* the system executes scripts /etc/rc.d directory
* **systemd** that includes an init daemon, with concurrent starting of services, service manager, ..., replaces `init` from 2019
     
**Daemon**
* strict def: its parent process terminates and the daemon is assigned `init` as its parent process and has no controlling terminal
* general def: any background process
* is not under the direct control of an interactive user
* is created either by a process forking a child and then immediately exiting, thus causing `init` to adopt the child, or by `init` directly => the parent (shell or `init`) receives exit notification
* the system often starts daemons at boot time that will respond to network requests, hardware activity, ...
* the process names of a daemon end with the letter d
* Ex: syslogd is a daemon that implements system logging facility, sshd is a daemon that serves incoming SSH connections, cron
* detaches from the invoking session
* dissociates from the controlling tty
* creates a new session and becoming the session leader of that session
* becoming a process group leader
* sets the root directory (/) as the current working directory => does not keep any directory in use that may be on a mounted file system
* changes the umask to 0 to allow open(), creat(), and other operating system calls to provide their own permission masks and not to depend on the umask of the caller
* redirects stdin, stdout, stderr to /dev/null or a logfile
* closes all the other file descriptors inherited from the parent process

</details>

<details><summary>Social projects</summary>

https://shtab.navalny.com/  
https://www.patreon.com/shtab_navalny  

</details>

<details><summary>Linens</summary>

# Linens
* [The On-Line Encyclopedia of Integer Sequences (OEIS)](https://oeis.org/)
* [42 peer finder](https://find-peers.codam.nl/Paris)
* 42 https://42evaluators.com/rncp/
* https://meta.intra.42.fr/issues/new?tag_names[]=Pedagogy
* [42 evals CVb3d2023](https://rphlr.github.io/42-Evals/) 
* [42 subjects](https://github.com/rphlr/42-Subjects)
* https://ivan-shamaev.ru/docker-compose-tutorial-container-image-install/  
* 42 https://meta.intra.42.fr/articles/title-level-7
* https://docs.google.com/spreadsheets/d/1aC5gSKkQkffmUdCiBm4GMsitl5r-zMKmD2Gwi1roxYY/edit#gid=2101605241
* discord staff pédago si un claster bruyant
* discord staff gèrent les pcs et les serveurs de l'école, pas les gens
* Pour les SFP: vous déclarez être" en formation" lors de l’actualisation PE, même quand vous êtes en stage
* SFP & GEN Déclaration d'absence https://docs.google.com/forms/d/e/1FAIpQLSc6Rlu-rPcHkW04KNz43AOLBW-B3d1Hkhc-lbfnA5cTq3YwQg/viewform
* https://stackexchange.com/sites
</details>

* https://habr.com/ru/companies/reksoft/articles/792496/ 
* https://stackoverflowteams.com/c/42network/questions
* `./PmergeMe `shuf -i 1-100000 -n 3000 | tr "\n" " "`` start a programm with random arguments
* https://www.random.org/
* https://2qbit.com/true-random-number-generator/?utm_source=yandex&utm_medium=cpc&utm_campaign=95384643&utm_content=14959768887&utm_term=генератор%20случайных%20чисел&yclid=9011125947744387071
* создать анкету https://app.dragnsurvey.com/en/login
* Telegram блокировали в России, Иране, Афганистане и Китае
  + полное блокирование не удалось достичь нигде
  + с замедлением / через VPN-сервисы / частные сети и "серые" клиенты продолжает работать
  + российские провайдеры имеют на своих базах оборудование, которое фильтрует, мониторит и записывает трафик, и в некоторых регионах можно замедлить
  + страна с тоталитарным режимом и с такой же архитектурой телеком-сетей, где контролируется трафик и внешние каналы, может попытаться побороть, запретить и отключить Telegram
  + технологических возможностей на примере россии или КНДР или КНР что-то запретить или замедлить
  + я считаю, что TG без инвестиций в сети не запретим и не отключим. Это технически невозможно (Александ Глущенко)
  + сама платформа создана таким образом, чтобы предотвращать блокировку
  + можно писать провайдерам, чтобы они блокировали направления трафика, порты и т.д. Заниматься чем-то таким, что будет похоже на усилия по блокировке. У нас "ВКонтакте" заблокирован. Но он же работает, если надо. Если будет закон "блокировать воз" - это начнут делать, потому что закон есть закон. это будет очень некачественно получаться.А Telegram со своей стороны начнет противодействовать.
