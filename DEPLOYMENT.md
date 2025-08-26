# Guía de Despliegue en Koyeb

## Prerrequisitos
- Cuenta en Koyeb
- Git configurado en tu máquina local
- CLI de Koyeb instalado (opcional)

## Pasos para el Despliegue

### 1. Crear Base de Datos PostgreSQL en Koyeb

1. Ve a tu dashboard de Koyeb
2. Crea un nuevo servicio de base de datos
3. Selecciona PostgreSQL
4. Configura:
   - Nombre de la base de datos
   - Usuario y contraseña
   - Región (la más cercana a ti)
5. Anota los datos de conexión

### 2. Configurar Variables de Entorno

Actualiza el archivo `koyeb.yaml` con tus datos reales:
- `APP_URL`: URL de tu aplicación
- `DB_HOST`: Host de tu base de datos PostgreSQL
- `DB_DATABASE`: Nombre de tu base de datos
- `DB_USERNAME`: Usuario de la base de datos
- `DB_PASSWORD`: Contraseña de la base de datos

### 3. Desplegar la Aplicación

#### Opción A: Usando el Dashboard de Koyeb
1. Ve a tu dashboard de Koyeb
2. Crea un nuevo servicio web
3. Conecta tu repositorio de Git
4. Selecciona la rama principal
5. Configura el build:
   - Builder: Dockerfile
   - Dockerfile: Dockerfile
6. Configura las variables de entorno
7. Despliega

#### Opción B: Usando CLI de Koyeb
```bash
# Instalar CLI de Koyeb
npm install -g @koyeb/cli

# Login
koyeb login

# Desplegar
koyeb app init laravel-app --dockerfile Dockerfile --ports 8080:http --routes /:laravel-app
```

### 4. Configurar Dominio Personalizado (Opcional)

1. Ve a la configuración de tu servicio web
2. Añade tu dominio personalizado
3. Configura los registros DNS según las instrucciones de Koyeb

### 5. Ejecutar Migraciones

Una vez desplegada la aplicación, ejecuta las migraciones:
```bash
# Conectar a tu aplicación desplegada
koyeb app logs laravel-app

# Ejecutar migraciones (si tienes acceso SSH)
php artisan migrate --force
```

## Estructura de Archivos

- `Dockerfile`: Configuración de Docker para PHP 8.2 + Apache
- `koyeb.yaml`: Configuración del servicio en Koyeb
- `docker/apache.conf`: Configuración de Apache
- `.dockerignore`: Archivos a excluir del build

## Solución de Problemas

### Error de Permisos
Si tienes problemas de permisos, verifica que las carpetas `storage` y `bootstrap/cache` tengan permisos 775.

### Error de Base de Datos
Verifica que:
- Las credenciales de la base de datos sean correctas
- La base de datos esté accesible desde tu aplicación
- Las extensiones de PostgreSQL estén instaladas

### Error de Build
Si el build falla:
- Verifica que el Dockerfile esté correcto
- Revisa los logs de build en Koyeb
- Asegúrate de que todas las dependencias estén en `composer.json`

## Monitoreo

- Usa los logs de Koyeb para monitorear tu aplicación
- Configura alertas para errores críticos
- Monitorea el uso de recursos de tu base de datos
