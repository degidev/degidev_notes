# Guía de Instalación de Laravel 12

## Requisitos Previos

1. PHP 8.1 o superior
2. Composer (gestor de paquetes de PHP)
3. MySQL 5.7.8 o superior
4. Node.js y npm (opcional para assets)

## Comprobar la instalación de PHP y Composer

```bash
php -v
composer -v
```


## Paso 1: Instalar Composer

1. Descarga e instala Composer desde: https://getcomposer.org/download/
2. Asegúrate de que Composer esté en tu PATH

## Paso 1.1: Instalar Laravel Installer

1. instalar laravel installer globalmente
```bash
composer global require laravel/installer
```
2. comprobar la instalación de laravel installer
```bash
laravel --version
```

## Paso 2: Crear un Nuevo Proyecto Laravel

Hay dos formas de crear un nuevo proyecto Laravel:

1. Usando Composer (método tradicional):
```bash
cd d:\ruta\a\tu\directorio
composer create-project laravel/laravel nombre-proyecto
```

2. Usando Laravel Installer (método más rápido):
```bash
laravel new nombre-proyecto
```



## Paso 3: Configurar las Variables de Entorno

1. Copia el archivo `.env.example` a `.env`
```bash
cp .env.example .env
```

2. Genera la clave de aplicación
```bash
php artisan key:generate
```

3. Configura la base de datos en el archivo `.env`
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=nombre_de_tu_base_de_datos
DB_USERNAME=root
DB_PASSWORD=contraseña
```

## Paso 4: Instalar Dependencias

```bash
composer install
npm install
```

## Paso 5: Generar los Assets

```bash
npm run build
```

## Paso 6: Configurar el Servidor de Desarrollo

1. Para iniciar el servidor de desarrollo:
```bash
php artisan serve
```
2. Usando composer (método más rápido):
```bash
composer run dev
```

2. Accede a tu aplicación en: http://localhost:8000

## Comandos Útiles

- Generar un controlador:
```bash
php artisan make:controller NombreController
```

- Generar un modelo:
```bash
php artisan make:model NombreModelo
```

- Generar una migración:
```bash
php artisan make:migration crear_tabla_nombre
```

- Ejecutar migraciones:
```bash
php artisan migrate
```

- Ejecutar migraciones con seeders:
```bash
php artisan migrate:fresh --seed
```

## Estructura Básica del Proyecto

```
laravel-app/
├── app/                  # Código de la aplicación
├── bootstrap/            # Archivos de inicialización
├── config/              # Archivos de configuración
├── database/            # Migraciones y seeders
├── public/              # Archivos públicos
├── resources/           # Vistas y assets
├── routes/              # Definición de rutas
├── storage/             # Archivos de almacenamiento
└── tests/              # Pruebas unitarias
```

## Recomendaciones

1. Mantén tu archivo `.env` fuera del control de versiones
2. Usa el comando `composer update` para actualizar dependencias
3. Documenta bien tus migraciones y seeders
4. Utiliza Laravel Mix para gestionar assets
5. Implementa pruebas unitarias desde el inicio

¡Listo! Ahora tienes un proyecto Laravel 12 listo para empezar a desarrollar. ¡Buena suerte con tu proyecto!