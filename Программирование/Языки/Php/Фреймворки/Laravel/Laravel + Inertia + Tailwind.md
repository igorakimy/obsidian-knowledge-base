Установить Laravel: 
```bash
curl -s "https://laravel.build/example-app" | bash
```

Установить React:
```bash
npm i react react-dom
```

Установить Vite плагин для React:
```bash
npm install --save-dev @vitejs/plugin-react
```

Установить Inertiajs для php: 
```bash
composer require inertiajs/inertia-laravel
```

Добавить следующее содержимое в файл `resources/view/app.blade.php`:
```html
<!DOCTYPE html>  
<html lang="ru">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport"  
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">  
    <meta http-equiv="X-UA-Compatible" content="ie=edge">  
    @vite('resources/js/app.js')  
    @inertiaHead  
</head>  
<body>  
    @inertia  
</body>  
</html>
```

Создать middleware для Inertia: 
```bash
php artisan inertia:middleware
```

Зарегистрировать middleware для Inertia в файле `bootstrap/app.php`: 
```php
->withMiddleware(function (Middleware $middleware) {  
    $middleware->web(append: [  
        HandleInertiaRequests::class  
    ]);  
})
```

Установить Inertiajs для React: 
```bash
npm install @inertiajs/react
```

Добавить следующий javascript код в файл `resources/js/app.js`:
```jsx
import { createInertiaApp } from '@inertiajs/react'  
import { createRoot } from 'react-dom/client'  
  
createInertiaApp({  
    resolve: name => {  
        const pages = import.meta.glob('./Pages/**/*.jsx', { eager: true })  
        return pages[`./Pages/${name}.jsx`]  
    },  
    setup({ el, App, props }) {  
        createRoot(el).render(<App {...props} />)  
    },  
})
```

Переименовать файл `app.js` в `app.jsx`(не забыть поменять имя в тех местах, где файл подключается).

Подключить react-плагин для Vite в файле `vite.config.js`:
```js
import { defineConfig } from 'vite';  
import laravel from 'laravel-vite-plugin';  
import react from '@vitejs/plugin-react';  
  
export default defineConfig({  
    plugins: [  
        laravel({  
            input: ['resources/js/app.jsx'],  
            refresh: true,  
        }),  
        react(),  
    ],  
});
```

Добавить директиву **@viteReactRefresh** в файл `resources/views/app.blade.php` перед импортом js файла:
```html
@viteReactRefresh  
@vite('resources/js/app.jsx')  
@inertiaHead
```

Установить TailwindCSS: 
```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

В файле `tailwind.config.js` изменить секцию content:
```js
/** @type {import('tailwindcss').Config} */  
export default {  
  content: [  
      './resources/**/*.blade.php',  
      './resources/**/*.jsx',  
      './resources/**/*.js',  
  ],  
  theme: {  
    extend: {},  
  },  
  plugins: [],  
}
```

Добавить следующие директивы в файл `resources/css/app.css`:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Импортировать файл `resources/css/app.css` в файле `resources/js/app.jsx`:
```js
import './bootstrap'
import '../css/app.css'
```

Добавить алиас для javascript файлов при импорте в файле конфигурации `vite.config.js`:
```js
resolve: {  
    alias: {  
        '@': '/resources/js'  
    }  
}
```

Установить пакет (Ziggy) для удобной маршрутизации со стороны фронтенда:
```bash
composer require tightenco/ziggy
```

Установить пакет для загрузки всех определений типов в Node.js:
```bash
npm install -D @types/node
```

Добавить алиас для пакета ziggy-js в файле `vite.config.js`:
```js
import path from 'path'  
  
export default defineConfig({  
	// ...
    resolve: {  
        alias: {  
            '@': '/resources/js',  
            'ziggy-js': path.resolve('/vendor/tightenco/ziggy')  
        }  
    }  
});
```

Добавить директиву **@routes** в файл `resources/views/app.blade.php` после **@inertiaHead:**:
```html
@vite('resources/js/app.js')  
@inertiaHead
@routes
```

Добавить следующую настройку,  необходимую для запуска фронтенда и бэкенда на одном хосте:
```js
// ...
server: {  
  hmr: {  
    host: 'localhost',  
  },  
},
```