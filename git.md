## GIT    
* `git add` внести изменения в индекс (временное хранилище)
* `git commit -a -m "message"` только для отслеживаемых файлов (Untracked files игнорируются)
* откатить
  + убрать локальные изменения и вернуться к последнему push
    - `git checkout .` (checkout изменяет сам HEAD)
  + сместить указатель на один коммит назад
    - git reset --hard HEAD~1 (reset переместит то, на что указывает HEAD)
    - git push -f 
  + оставить нетронутыми индекс и дерево файлов проекта, вернуться к работе с указанным коммитом
    - git reset --hard HEAD~1 
    - git reset --soft 
    - git push -f 
  + безопасным способом (?)
    - `git stash save`
  + ?
    - `git revert <commit>` 
    - https://ru.stackoverflow.com/questions/431520/%D0%9A%D0%B0%D0%BA-%D0%B2%D0%B5%D1%80%D0%BD%D1%83%D1%82%D1%8C%D1%81%D1%8F-%D0%BE%D1%82%D0%BA%D0%B0%D1%82%D0%B8%D1%82%D1%8C%D1%81%D1%8F-%D0%BA-%D0%B1%D0%BE%D0%BB%D0%B5%D0%B5-%D1%80%D0%B0%D0%BD%D0%BD%D0%B5%D0%BC%D1%83-%D0%BA%D0%BE%D0%BC%D0%BC%D0%B8%D1%82%D1%83
* `git hist`
* `git log`
* `git log --name-status` список файлов коммита
* `gît commit "filename"` много информации
* alias in .gitconfig
* файл `.gitkeep` чтобы пустые папки попали в индексацию git
### submodule
* https://github.com/chaconinc/DbConnector
* submodules are not downloaded when you git clone
* git видит его как конкретный коммит этого репозитория, не отслеживает его содержимое пока вы не перейдёте в него
* `git submodule add`
* клинирование с субмодулями 
    + 1 way
       - git clone --recurse-submodules, инициализирует и обновит каждый подмодуль
    + 2 way
       - git clone (вместо подмодуля пустая папка)
       - git submodule init инициализация лок конфиг файла (появяется .gitmodules)
       - git submodule update получение всех данных этого проекта и извлечения соответствующего коммита, указанного в основном проекте
    + 3 way
       - `git clone`
       - `git submodule update --init`
    + 4 way
       - `git submodule update --init --recursive`
* проверить наличие изменений в подмодуле:
   + `cd libft`
   + `git fetch`
   + `git merge` для обновления локальной версии отслеживаемой ветки
* опубликовать изменения в подмодуле
    + `cd libft`
    + `git pull origin master`
* un truc comme la mlx tu utilises un gestionnaire de paquet (vcpkg, conan)
* if conflicts with my collegue:
  + если ошибку выдаст при твоем пуше, то git pull --rebase 
  + этим обновляешь ветку remote до текущей и можно после этого свои правки пушить
