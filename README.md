# 🏠 Homepage Docker

[![GitHub](https://img.shields.io/badge/GitHub-Repo-blue?logo=github)](https://github.com/gethomepage/homepage)
[![Docker](https://img.shields.io/badge/Docker-ghcr.io%2Fgethomepage%2Fhomepage-blue?logo=docker)](https://github.com/gethomepage/homepage/pkgs/container/homepage)
[![License](https://img.shields.io/badge/License-MIT-green?logo=opensourceinitiative)](https://github.com/gethomepage/homepage/blob/main/LICENSE)

## 📋 Descripción general

**Homepage** es un dashboard de aplicaciones moderno, completamente estático, rápido y seguro, altamente personalizable con integraciones para más de 100 servicios y traducciones a múltiples idiomas. Fácilmente configurable mediante archivos YAML o a través del descubrimiento de etiquetas de Docker.

Es tu startpage ideal para el día a día y un compañero útil a lo largo del mismo, ofreciendo características como búsqueda rápida, marcadores, soporte meteorológico, una amplia gama de integraciones y widgets, un diseño elegante y moderno, y un enfoque en el rendimiento. Todo esto se ejecuta de forma estática en Docker, lo que garantiza tiempos de carga instantáneos y una alta seguridad al proxyar todas las solicitudes API.

## ✨ Características principales

- ⚡ **Rápido**: El sitio se genera estáticamente en tiempo de compilación para tiempos de carga instantáneos
- 🔒 **Seguro**: Todas las solicitudes de API a servicios backend se proxyan, manteniendo tus claves API ocultas
- 🏗️ **Multi-arquitectura**: Imágenes Docker construidas para AMD64 y ARM64
- 🌍 **Multi-idioma (i18n)**: Soporte para más de 40 idiomas
- 🔖 **Marcadores de servicios y web**: Agrega enlaces personalizados a tu página de inicio
- 🐳 **Integración con Docker**: Estado y estadísticas de contenedores, descubrimiento automático de servicios vía etiquetas
- 🔌 **Integración de servicios**: Más de 100 integraciones de servicios, incluyendo apps *arr y autoalojadas populares
- 📊 **Widgets de información y utilidad**: Clima, hora, fecha, búsqueda y más
- 🎨 **Altamente personalizable**: Soporte para temas personalizados, CSS y JS, layouts y localización

## 📋 Requisitos del sistema

- Docker
- Docker Compose
- PUID/PGID (opcional, para permisos de usuario/grupo)
- Puerto TCP: **3000**
- Volumen para configuración: Una ruta local donde se almacenará la configuración (ej: `/path/to/config`)
- Volumen para Docker Socket: `/var/run/docker.sock` (opcional, para integración Docker)
- **HOMEPAGE_ALLOWED_HOSTS**: Variable de entorno requerida (ej: `gethomepage.dev`)

> ⚠️ **Aviso de seguridad**: Homepage puede acceder a información personal. Si es accesible desde redes no confiables, **DEBE** estar detrás de un proxy inverso (y/o VPN) que aplique autenticación, TLS y valide estrictamente los encabezados Host. Una opción de inicio de sesión OIDC integrada o contraseña simple está disponible (opt-in) como guardia.

## 🐳 Instalación

### docker-compose.yml

```yaml
services:
  homepage:
    image: ghcr.io/gethomepage/homepage:latest
    container_name: homepage
    environment:
      - HOMEPAGE_ALLOWED_HOSTS=gethomepage.dev # Requerido, puede necesitar puerto. Ver gethomepage.dev/installation/#homepage_allowed_hosts
      - PUID=1000 # Opcional, tu ID de usuario
      - PGID=1000 # Opcional, tu ID de grupo
    ports:
      - "3000:3000"
    volumes:
      - /path/to/config:/app/config # Asegúrate de que tu directorio de configuración local exista
      - /var/run/docker.sock:/var/run/docker.sock:ro # Opcional, para integraciones Docker
    restart: unless-stopped
```

### Pasos de instalación

```bash
# 1. Guarda el compose como docker-compose.yml en un directorio
# 2. Asegúrate de crear el directorio de configuración local primero:
mkdir -p /path/to/config

# 3. Inicia Homepage
docker compose up -d

# 4. Verifica los logs
docker compose logs -f homepage
```

## ⚙️ Configuración

1. Todas las configuraciones se realizan a través de archivos YAML en el volumen de configuración (`/path/to/config`)
2. Para ejemplos iniciales, copia el directorio `src/skeleton` del repositorio de Homepage a tu volumen de configuración
3. Reinicia el contenedor Docker para que tome los nuevos archivos de configuración
4. Edita los archivos YAML en tu directorio de configuración para añadir tus aplicaciones y enlaces
5. Homepage puede descubrir automáticamente servicios Docker a través de etiquetas en tus contenedores Docker
6. Personaliza temas, diseños y añade widgets explorando los archivos de configuración
7. Puedes añadir CSS y JavaScript personalizados para un control total del estilo

## 🚀 Primeros pasos

1. **Configuración inicial**
   - Accede a `http://localhost:3000`
   - Homepage se inicializará con una configuración básica
   - Copia el directorio `src/skeleton` del repositorio de Homepage a tu volumen de configuración (`/path/to/config`) para obtener archivos de configuración de ejemplo
   - Reinicia el contenedor Docker para que tome los nuevos archivos de configuración

2. **Añadir servicios y bookmarks**
   - Edita los archivos YAML en tu directorio de configuración (`/path/to/config`)
   - Consulta la documentación de servicios y marcadores para añadir tus aplicaciones y enlaces
   - Homepage puede descubrir automáticamente servicios Docker a través de etiquetas en tus contenedores Docker

3. **Personalizar y añadir widgets**
   - Explora los archivos de configuración en `/path/to/config` para personalizar temas, diseños y añadir widgets
   - La documentación de widgets te guiará para añadir clima, hora, búsquedas y más
   - Puedes añadir CSS y JavaScript personalizados para un control total del estilo

## 💡 Casos de uso

- 🏠 **Dashboard principal de homelab**: Punto de entrada único para todos tus servicios autoalojados
- 🔍 **Monitor de estado de contenedores**: Visualiza el estado y estadísticas de tus contenedores Docker en tiempo real
- 📚 **Gestor de marcadores centralizado**: Organiza enlaces a servicios web, documentación y herramientas internas
- 🌤️ **Panel de información personal**: Widgets de clima, hora, fecha y búsquedas rápidas
- 🔐 **Portal seguro para familia/equipo**: Con autenticación OIDC o contraseña simple detrás de proxy inverso

## 🔒 Acceso remoto seguro

Para exponer Homepage de forma segura a internet:

1. **Proxy inverso obligatorio**: Usa Traefik, Nginx Proxy Manager, Caddy o similar
2. **TLS/SSL**: Configura certificados válidos (Let's Encrypt recomendado)
3. **Autenticación**: Habilita OIDC (Authelia, Authentik, Keycloak) o contraseña simple en Homepage
4. **Validación de Host**: Configura `HOMEPAGE_ALLOWED_HOSTS` con tu dominio público exacto
5. **VPN alternativa**: Restringe acceso solo a red VPN (Tailscale, WireGuard) sin exponer puertos públicos

## 🛠️ Gestión y mantenimiento

| Acción | Comando |
|--------|---------|
| Ver estado | `docker compose ps` |
| Ver logs | `docker compose logs -f homepage` |
| Detener Homepage | `docker compose down` |
| Actualizar versión | `docker compose pull homepage && docker compose up -d homepage` |
| Acceder shell (troubleshooting) | `docker compose exec homepage bash` |
| Backup configuración | `docker cp homepage:/app/config ./homepage-config-backup` |
| Restore configuración | `docker compose down && docker compose up -d && docker cp ./homepage-config-backup/* homepage:/app/config/ && docker compose restart homepage` |
| Monitorear consumo | `docker stats homepage` |

## 📝 Licencia

Este proyecto utiliza la imagen oficial de **Homepage** licenciada bajo **MIT License**.
- Repositorio oficial: [github.com/gethomepage/homepage](https://github.com/gethomepage/homepage)
- Licencia: [MIT](https://github.com/gethomepage/homepage/blob/main/LICENSE)

---

> 📖 **Guía completa**: [Cómo instalar y configurar Homepage en Docker](https://genbyte.blogspot.com/2026/10/como-instalar-y-configurar-homepage-en.html)  
> 🎥 **Video tutorial**: [Canal YouTube Genbyte](https://www.youtube.com/@genbyte)  
> 💬 **Comunidad**: [Discord Homepage](https://discord.gg/homepage) | [Documentación oficial](https://gethomepage.dev/)