‎
## Laboratory work 4
#Автроизация и настройка окружения 
```bash
export GITHUB_USERNAME=<имя_пользователя>
export GITHUB_TOKEN=<полученный_токен>
cd ${GITHUB_USERNAME}/workspace
pushd .
source scripts/activate
```
#Установка Ruby и Travis CLI
```sh
\curl -sSL https://get.rvm.io | bash -s -- --ignore-dotfiles
echo "source $HOME/.rvm/scripts/rvm" >> scripts/activate
. scripts/activate
rvm autolibs disable
rvm install ruby-2.4.2
rvm use 2.4.2 --default
gem install travis
```
#Клонирование и настройка репозитория 
```sh
git clone https://github.com/${GITHUB_USERNAME}/lab03 projects/lab04
cd projects/lab04
git remote remove origin
git remote add origin https://github.com/${GITHUB_USERNAME}/lab04
```
#Создание конфигурации Travis CLI
```sh
cat > .travis.yml <<EOF
language: cpp
EOF
$ cat >> .travis.yml <<EOF
script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
$ cat >> .travis.yml <<EOF
addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
```
# Авотризация в Travis CLI
```sh
travis login --github-token ${GITHUB_TOKEN}
travis lint
```
#Добавление значка и отправка изменений 
```sh
ex -sc '1i|<фрагмент_вставки_значка>' -cx README.md
git add .travis.yml
git add README.md
git commit -m"added CI"
git push origin main
```
#Мониторинг и управление Travis CLI
```sh
travis lint
travis accounts
travis sync
travis repos
travis enable
travis whatsup
travis branches
travis history
travis show
```
