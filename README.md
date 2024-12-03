# Настройка CI/CD

### Какой проект выбран

Был выбран шаблон при создании React-приложения
```
npx create-react-app .
```

### Код файла ci-cd.yml

```
name: React CI/CD

on:
  push:
    branches: ["master"]

jobs:
  install:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install
        
  test1:
    runs-on: ubuntu-latest
    needs: install
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

  test2:
    runs-on: macos-latest
    needs: install
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

  test3:
    runs-on: windows-latest
    needs: install
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
        
  build:
    runs-on: ubuntu-latest
    needs: install
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run build
        run: npm run build
        
  deploy:
    runs-on: ubuntu-latest
    needs: [test1, test2, test3, build]
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: rm origin
        run: git remote remove origin

      - name: set email
        run: git config --global user.email "action@action.com"

      - name: set username
        run: git config --global user.name "action"

      - name: add origin
        run: git remote add origin https://${{ secrets.TOKEN }}@github.com/ggermashev/sf-cicd.git

      - name: Run deploy
        run: npm run deploy
```

### Описание джобов

#### install
Установка зависимостей

#### test{1,2,3}
Тесты на ОС windows, linux, mac.
Запускаются после install

#### build
Сборка проекта
Запускается после install

#### deploy
Публикация проекта на [gh-pages](https://ggermashev.github.io/sf-cicd/)
Запускается после test{1,2,3}, build

#### Какие ивенты тригерят джобы

Джобы тригерятся при push в master

#### Какие github-actions были использованы

- actions/checkout@v3

    Клонирование кода из репозитория

- actions/setup-node@v3

    Установка Node.js
