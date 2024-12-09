Proyecto API Client con Laravel y Vue.js
Este proyecto consiste en un cliente API desarrollado con Vue.js (frontend) y un servidor API desarrollado con Laravel (backend). Sigue estas instrucciones para configurar y ejecutar el proyecto en tu entorno local.

Requisitos previos
Tener instalado:

Git
PHP (versión 7.4 o superior)
Composer
Node.js y npm (versión 14 o superior)


Instrucciones:

git clone https://github.com/Mario-1421/api-prac2-mario-panos.git
Esto crea 2 carpetas: "Backend" y "Frontend". Accede a ellas con 2 ventanas de terminal.

Backend
Instala las dependencias de Laravel: composer install
Migrar la base de datos: php artisan migrate
Crear clave de la app: php artisan key:generate

Frontend

Instalar dependencias: npm install

Inicia el backend:
php artisan serve

Inicia el frontend, Frontend (Vue.js)
npm run serve

Accede al frontend en http://localhost:8080 y al backend en http://localhost:8000.

Créditos
Proyecto desarrollado por [Mario Paños].
