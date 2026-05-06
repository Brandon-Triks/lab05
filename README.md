# Лабораторная работа №03
## Шаг1 - подготовка репозитория
  Зададим переменную окружения своим логином на github и скопируем репозиторий lab02 в папку lab03, используя следующие команды:  
`export GITHUB_USERNAME=Brandon-Triks`  
`git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03`  
  Результат вывода:  
`Клонирование в «projects/lab03»...
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 15 (delta 1), reused 9 (delta 0), pack-reused 0 (from 0)
Получение объектов: 100% (15/15), готово.
Определение изменений: 100% (1/1), готово.`  
    Удалим текущую связь с удалённым репозиторием, а также добавим новый адрес для имени origin с помощью команд: 
`git remote remove origin  
git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git`  
## Шаг2 - ручная сборка
* Прежде чем использовать CMake, попробуем собрать проект вручную командами компилятора g++.  
Соберём объектный файл и статическую библиотеку:  
`g++ -std=c++11 -I./include -c sources/print.cpp`  
`ar rvs print.a print.o`  
Вывод:  
`ar: создаётся print.a`
`a - print.o`  
* Соберём и запустим примеры:
    Example 1:  
    `g++ -std=c++11 -I./include -c examples/example1.cpp`  
    `g++ example1.o print.a -o example1`  
    `./example1 && echo` 

  Вывод - `hello`  
    * Example 2:  
      `g++ -std=c++11 -I./include -c examples/example2.cpp`  
      `g++ example2.o print.a -o example2` 
      `./example2` 
      `cat log.txt && echo`
  
  Вывод - `hello` 

* Удалим мусор, оставшийся после ручной сборки:  
 `rm -rf example1.o example2.o print.o print.a example1 example2 log.txt`  
 
 ## Шаг3 - написание CMakeLists.txt  
 Напишем CMakeLists.txt.  
 * Инициализация проекта  
    `cat > CMakeLists.txt <<EOF`  
    `cmake_minimum_required(VERSION 3.4)`  
    `project(print)`  
    `EOF`
  * Настройка стандарта C++   
    `cat >> CMakeLists.txt <<EOF`  
    `set(CMAKE_CXX_STANDARD 11)`  
    `set(CMAKE_CXX_STANDARD_REQUIRED ON)`
    `EOF`  
  * Создание библиотеки и путей поиска заголовков  
    `cat >> CMakeLists.txt <<EOF`  
    `add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)`  
    `include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)`
    `EOF`  
   * Первая попытка сборки  
    `cmake -H. -B_build`  
    `cmake --build _build`  
    Вывод:  
    `[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o`  
    `[100%] Linking CXX static library libprint.a`  
    `[100%] Built target print`  
    
  ## Шаг4 - Сборка исполняемых файлов через CMake  
  Добавим в конфиг сборку наших примеров (example1 и example2).  
  * Добавление исполняемых файлов в конфиг  
    `cat >> CMakeLists.txt <<EOF`  
    `add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)`  
    `add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)`  
    `EOF`  
  * Линковка (связывание) их с нашей библиотекой print  
    `cat >> CMakeLists.txt <<EOF`  
    `target_link_libraries(example1 print)`  
    `target_link_libraries(example2 print)`  
    `EOF`  
   * Финальная сборка и проверка
    `cmake --build _build`  
    `./_build/example1 && echo`  
    `./_build/example2`  
    `cat log.txt && echo`  
    `rm -rf log.txt`   
      
     В обоих случаях вывело сообщение hello.

  ## Шаг5 - написание CMakeLists.txt  
  * Скачаем эталонный файл
    `git clone https://github.com/tp-labs/lab03 tmp`  
    `mv -f tmp/CMakeLists.txt .`  
    `rm -rf tmp`  
    
    Вывод:  
    `Клонирование в «tmp»...`  
    `remote: Enumerating objects: 91, done.`  
    `remote: Counting objects: 100% (30/30), done.`  
    `remote: Compressing objects: 100% (9/9), done.`  
    `remote: Total 91 (delta 23), reused 21 (delta 21), pack-reused 61 (from 1)`  
    `Получение объектов: 100% (91/91), 1.02 МиБ | 2.59 МиБ/с, готово.`  
    `Определение изменений: 100% (41/41), готово.`  
     
     * Сборка с установкой (install)
    `cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install`  
    `cmake --build _build --target install`  
    `tree _install`  
    Результат представлен на рисунке ниже:  
    
    ![Рисунок 1](https://raw.githubusercontent.com/Brandon-Triks/imagesl/main/пик.jpg )  
      
  ## Шаг6 - завершение  
  Отправим изменения в новый репозиторий  
   * `git add CMakeLists.txt`  
   * `git commit -m "added CMakeLists.txt"`  
   * `git push origin master`

 
      
    
    
  



