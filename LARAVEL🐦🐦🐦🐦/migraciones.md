# Migraciones en Laravel 12

## Introducción
Las migraciones son como el control de versiones para tu base de datos. Te permiten:
- Crear y modificar tablas
- Mantener un historial de cambios
- Sincronizar la estructura de la base de datos entre diferentes entornos

## Crear una Migración

1. **Crear Tabla Nueva**
```bash
php artisan make:migration crear_tabla_productos --create=productos
```

2. **Modificar Tabla Existente**
```bash
php artisan make:migration agregar_columna_stock_a_productos --table=productos
```

3. **Migración Básica**
```bash
php artisan make:migration actualizar_tabla_productos
```

**Estructura del Nombre**:
- `crear_tabla_nombre` (Crear tablas)
- `agregar_columna_nombre_a_tabla` (Agregar columnas)
- `modificar_columna_nombre_en_tabla` (Modificar columnas)
- `eliminar_columna_nombre_de_tabla` (Eliminar columnas)
- `renombrar_tabla_antigua_a_nueva` (Renombrar tablas)

## Estructura Básica de una Migración

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CrearTablaProductos extends Migration
{
    public function up()
    {
        Schema::create('productos', function (Blueprint $table) {
            $table->id();
            $table->string('nombre');
            $table->decimal('precio', 8, 2);
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('productos');
    }
}
```

## Tipos de Columnas y Sus Características

### Tipos de Datos Numéricos

```php
// Tipos enteros
$table->tinyInteger('columna'); // -128 a 127
$table->smallInteger('columna'); // -32768 a 32767
$table->mediumInteger('columna'); // -8388608 a 8388607
$table->integer('columna'); // -2147483648 a 2147483647
$table->bigInteger('columna'); // -9223372036854775808 a 9223372036854775807

// Tipos decimales
// Los parámetros (8,2) representan:
// - Primer número (8): Número total de dígitos (incluyendo los decimales)
// - Segundo número (2): Número de dígitos después del punto decimal

// Ejemplo: $table->decimal('precio', 8, 2)
// - Puede almacenar números como: 999999.99 (8 dígitos totales, 2 decimales)
// - No puede almacenar: 1000000.00 (excede los 8 dígitos totales)
// - No puede almacenar: 999999.999 (excede los 2 decimales)

// Tipos de precisión
$table->float('columna', 8, 2); // Precisión variable (menos precisa)
$table->double('columna', 15, 8); // Precisión alta (más precisa)
$table->decimal('columna', 8, 2); // Precisión fija (recomendado para dinero)

// Ejemplos prácticos
$table->decimal('precio', 10, 2); // Para precios (hasta 99999999.99)
$table->decimal('cantidad', 5, 2); // Para cantidades (hasta 999.99)
$table->decimal('tasa', 6, 4); // Para tasas (hasta 99.9999)

// Tipos especiales
$table->unsignedBigInteger('columna'); // Entero no negativo
$table->unsignedInteger('columna'); // Entero no negativo
```

### Tipos de Datos de Texto

```php
// Tipos de texto
$table->char('columna', 10); // Caracteres fijos
$table->string('columna', 255); // Caracteres variables
$table->text('columna'); // Texto largo (65,535 caracteres)
$table->mediumText('columna'); // Texto muy largo (16,777,215 caracteres)
$table->longText('columna'); // Texto extenso (4,294,967,295 caracteres)

// Tipos especiales
$table->enum('columna', ['valor1', 'valor2', 'valor3']); // Lista de valores
$table->json('columna'); // Datos JSON
$table->jsonb('columna'); // Datos JSON binario
```

### Tipos de Datos de Fecha y Tiempo

```php
// Fechas y tiempos
$table->date('columna'); // YYYY-MM-DD
$table->time('columna'); // HH:MM:SS
$table->dateTime('columna'); // YYYY-MM-DD HH:MM:SS
$table->timestamp('columna'); // Timestamp con zona horaria
$table->timestampTz('columna'); // Timestamp con zona horaria

// Campos de auditoría
$table->timestamps(); // created_at y updated_at
$table->softDeletes(); // deleted_at para eliminación suave
```

### Tipos de Datos Binarios

```php
// Tipos binarios
$table->binary('columna'); // Datos binarios
$table->uuid('columna'); // UUID (Universally Unique Identifier)
$table->uuidMorphs('columna'); // Relaciones polimórficas con UUID
```

### Tipos de Datos Especiales

```php
// Tipos especiales
$table->ipAddress('columna'); // Dirección IP
$table->macAddress('columna'); // Dirección MAC
$table->geometry('columna'); // Geometría
$table->point('columna'); // Punto geográfico
```

## Restricciones y Modificadores de Columnas

```php
// Modificadores básicos
$table->string('columna')->nullable(); // Permite NULL
$table->string('columna')->default('valor'); // Valor por defecto
$table->string('columna')->unique(); // Valor único

// Restricciones adicionales
$table->string('columna')->after('otra_columna'); // Posiciona después de otra columna
$table->string('columna')->charset('utf8'); // Especifica charset
$table->string('columna')->collation('utf8_unicode_ci'); // Especifica collation

// Restricciones de longitud
$table->string('columna')->length(255); // Longitud máxima
$table->string('columna')->min(10); // Longitud mínima

// Restricciones de validación
$table->string('columna')->required(); // Requerido
$table->string('columna')->email(); // Formato email
$table->string('columna')->url(); // Formato URL
```

## Relaciones y Foreign Keys

```php
// Relaciones básicas
$table->foreignId('user_id')->constrained(); // Relación con users
$table->foreignId('category_id')->constrained('categories'); // Relación con categories

// Opciones de foreign key
$table->foreignId('user_id')->constrained()
    ->onDelete('cascade'); // Elimina en cascada
    ->onUpdate('cascade'); // Actualiza en cascada

// Relaciones polimórficas
$table->morphs('imageable'); // Relación polimórfica
$table->uuidMorphs('imageable'); // Relación polimórfica con UUID

// Relaciones múltiples
$table->foreignId('user_id')->constrained()
    ->foreignId('post_id')->constrained()
    ->foreignId('comment_id')->constrained();

// Índices compuestos
$table->index(['columna1', 'columna2']); // Índice compuesto
$table->unique(['columna1', 'columna2']); // Índice único compuesto
```

## Ejemplos de Uso Común

```php
// Tabla de usuarios con relaciones
Schema::create('users', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('email')->unique();
    $table->timestamp('email_verified_at')->nullable();
    $table->string('password');
    $table->rememberToken();
    $table->timestamps();
    $table->softDeletes();
});

// Tabla de posts con relaciones
Schema::create('posts', function (Blueprint $table) {
    $table->id();
    $table->foreignId('user_id')->constrained()->onDelete('cascade');
    $table->string('title');
    $table->text('content');
    $table->timestamps();
});

// Tabla de comentarios con relaciones
Schema::create('comments', function (Blueprint $table) {
    $table->id();
    $table->foreignId('post_id')->constrained()->onDelete('cascade');
    $table->foreignId('user_id')->constrained()->onDelete('cascade');
    $table->text('content');
    $table->timestamps();
});

// Tabla de likes con relaciones polimórficas
Schema::create('likes', function (Blueprint $table) {
    $table->id();
    $table->morphs('likeable'); // Puede ser post o comment
    $table->foreignId('user_id')->constrained()->onDelete('cascade');
    $table->timestamps();
});
```



## Ejecutar Migraciones

```bash
# Ejecutar todas las migraciones
php artisan migrate

# Ejecutar migraciones con seeders
php artisan migrate --seed

# Ejecutar migraciones específicas
php artisan migrate --path=/database/migrations/2023_01_01_000000_crear_tabla_productos.php
```

## Rollback de Migraciones

```bash
# Rollback de la última migración
php artisan migrate:rollback

# Rollback de todas las migraciones
php artisan migrate:reset

# Rollback y luego re-migrar
php artisan migrate:refresh

# Rollback y re-migrar con seeders
php artisan migrate:fresh --seed
```

## Modificar Tablas Existentes

```php
public function up()
{
    Schema::table('productos', function (Blueprint $table) {
        $table->string('descripcion')->after('nombre');
        $table->decimal('descuento')->default(0);
    });
}

public function down()
{
    Schema::table('productos', function (Blueprint $table) {
        $table->dropColumn('descripcion');
        $table->dropColumn('descuento');
    });
}
```

## Renombrar Tablas y Columnas

```php
public function up()
{
    Schema::rename('old_table', 'new_table');
    
    Schema::table('productos', function (Blueprint $table) {
        $table->renameColumn('old_column', 'new_column');
    });
}

public function down()
{
    Schema::rename('new_table', 'old_table');
    
    Schema::table('productos', function (Blueprint $table) {
        $table->renameColumn('new_column', 'old_column');
    });
}
```

## Mejores Prácticas

1. **Nombres de migraciones**: Usa nombres descriptivos y en inglés
2. **Operaciones en `up()`**: Añade nuevas columnas al final
3. **Operaciones en `down()`**: Especifica exactamente la inversa de `up()`
4. **Foreign Keys**: Añádelas después de crear las tablas relacionadas
5. **Indexes**: Añádelos después de las columnas
6. **Documentación**: Documenta cambios importantes en las migraciones

## Comandos Útiles

```bash
# Ver el estado de las migraciones
php artisan migrate:status

# Crear una migración con múltiples tablas
php artisan make:migration crear_tablas_relacionadas --create="users,posts,comments"

# Ejecutar migraciones específicas
php artisan migrate --path=/database/migrations/2023_01_01_000000_crear_tabla_productos.php

# Ver el SQL que se ejecutará
php artisan migrate --pretend

# Ejecutar migraciones específicas
php artisan migrate --path=/database/migrations/2023_01_01_000000_crear_tabla_productos.php
```

## Resolución de Problemas

1. **Migraciones bloqueadas**: Usa `php artisan migrate:fresh`
2. **Problemas con foreign keys**: Verifica el orden de las migraciones
3. **Problemas con índices**: Verifica que las columnas existen
4. **Errores de sintaxis**: Usa `php artisan migrate --pretend` para ver el SQL

## Consejos Adicionales

1. **Version Control**: Mantén las migraciones en control de versiones
2. **Testing**: Prueba tus migraciones en un entorno de desarrollo
3. **Documentación**: Documenta cambios importantes en las migraciones
4. **Backup**: Haz backup antes de ejecutar migraciones en producción
5. **Rollback**: Siempre implementa un rollback correcto

¡Listo! Ahora tienes toda la información necesaria sobre migraciones en Laravel 12. ¡Buena suerte con tus migraciones!