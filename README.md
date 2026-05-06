# Лабораторная работа 4  
Вместо Travis будем использовать GitHub Actions.  
## Шаг1 - Подготовка репозитория  
1. Создадим публичный репозиторий lab04.
2. В терминале выполним клонирование предыдущей лабы и перенасроим удаленный репозиторий (аналогично прошлой лабе)   
  `export GITHUB_USERNAME=Brandon-Triks`  
  `cd ${GITHUB_USERNAME}/workspace`  
  `git clone https://github.com/${GITHUB_USERNAME}/lab03 projects/lab04`  -  клонируем lab03 в папку lab04  
  `cd projects/lab04`  
  `git remote remove origin`                                              - меняем привязку к удаленному репозиторию  
  `git remote add origin https://github.com/${GITHUB_USERNAME}/lab04`  
## Шаг2 - Создание конфигурации GitHub Actions  
1. Создадим папку для workflow  
  `mkdir -p .github/workflows`  
2. Создадим файл конфигурации ci.yml с помощью cat. Этот файл описывает, что делать серверу при пуше.  
   `cat > .github/workflows/ci.yml <<EOF`  
`name: CMake Build CI`  
` `  
`# Описываем события, на которые реагирует Action`  
`on:`  
`  push:`  
`    branches: [ master, main ]`  
`  pull_request:`  
`    branches: [ master, main ]`  
` `  
`jobs:`  
`  build:`  
`    # Запускаем на последней версии Ubuntu`  
`    runs-on: ubuntu-latest`  
` `  
`    steps:`  
`    # 1. Скачиваем код репозитория`  
`    - uses: actions/checkout@v3`  
` `  
`    # 2. Устанавливаем зависимости`  
`    - name: Install dependencies`  
`      run: |`  
`        sudo apt-get update`  
`        sudo apt-get install -y cmake build-essential`  
` `  
`    # 3. Конфигурируем проект (аналог cmake -H. -B_build ...)`  
`    - name: Configure CMake`  
`      run: cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install`  
` `  
`    # 4. Собираем проект`  
`    - name: Build`  
`      run: cmake --build _build`  
` `  
`    # 5. Тестируем установку`  
`    - name: Install target`  
`      run: cmake --build _build --target install`  
`EOF`  
## Шаг3 - Добавление значка статуса (Badge)  
Добавим badge в начало readme  
`export BADGE="\![Build Status](https://github.com/${GITHUB_USERNAME}/lab04/actions/workflows/ci.yml/badge.svg)"
ex -sc "1i|$BADGE" -cx README.md`  
Вывод:  
`ex -sc "1i|$BADGE" -cx README.md`  
## Шаг4 - Фиксация изменений и Push  
Отправляем изменения на GitHub.  
`git add .github/workflows/ci.yml`  
`git add README.md`  
`git commit -m "added GitHub Actions CI"`  
Вывод:  
`[main eb9eaf5] added GitHub Actions CI`  
`2 files changed, 37 insertions(+)`  
`create mode 100644 .github/workflows/ci.yml`  

`git push origin main`  
Вывод:  
`Перечисление объектов: 109, готово.`  
`Подсчет объектов: 100% (109/109), готово.`  
`При сжатии изменений используется до 2 потоков`  
`Сжатие объектов: 100% (72/72), готово.`  
`Запись объектов: 100% (109/109), 59.64 КиБ | 59.64 МиБ/с, готово.`  
`Total 109 (delta 27), reused 101 (delta 25), pack-reused 0 (from 0)`  
`remote: Resolving deltas: 100% (27/27), done.`  
`To https://github.com/Brandon-Triks/lab04`  
` * [new branch]      main -> main`

Удалив папки с артефактами сборки(_build и _install_) из репозитория, которые остались от предыдущей лабораторной работы и закоммитив изменения в репозиторий, заметим, что c Action все в порядке (возникал конфликт кэша CMake). См. рисунок:  

![Рисунок 1](https://raw.githubusercontent.com/Brandon-Triks/imagesl/main/action.png ) 
    
  



