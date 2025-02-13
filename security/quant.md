* Шифрование с открытым ключом
   + Современные криптографические системы (RSA, эллиптические кривые (ECC), ...) основываются на математических задачах (факторизация больших чисел, нахождение дискретных логарифмов, ...), которые с использованием классических компьютеров занимают огромное количество времени
   + Квантовые компьютеры, используя алгоритм Шора, могут решить эти задачи за полиномиальное время
     - RSA с ключом 2048 бит может быть легко взломан на квантовом компьютере
* Симметричное шифрование
  + Алгоритмы симметричного шифрования (AES,...) не так уязвимы:
    - квантовые компьютеры могут ускорить некоторые операции (поиск по грубой силе с помощью алгоритма Гровера, ...), но этот эффект гораздо менее разрушителен
    - Для AES с 256-битным ключом это будет означать, что для квантового компьютера потребуется примерно 2^128 шагов для взлома, что всё ещё считается безопасным
* Квантовые компьютеры пока не достигли нужного уровня мощности
  + Экспериментальные квантовые компьютеры могут решать лишь очень простые задачи
  + в ближайшие годы не ожидается, что квантовые компьютеры будут способны взломать современные криптографические алгоритмы
  + это может занять десятилетия, и даже тогда, вероятно, потребуется время для того, чтобы квантовые компьютеры достигли требуемого уровня стабильности и масштабируемости для взлома криптографических систем
* Постквантовая криптография
  + Разрабатывают новые алгоритмы
    - Некоторые алгоритмы уже на стадии стандартизации (проекты NIST по постквантовой криптографии, ...)
  + используют задачи на решетках, кодирования, и др
  + На переходный период могут быть разработаны гибридные системы, это позволит обеспечить безопасность данных на несколько лет вперед
