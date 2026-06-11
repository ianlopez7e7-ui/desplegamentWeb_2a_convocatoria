# Task Manager RA6

## Arquitectura
És una web PHP que s'executa amb Apache i es connecta a una base de dades MySQL.

- Servei `web`: imatge `php:8.2-apache`, fitxers PHP a `src/`, exposa el port `8080` al host.
- Servei `db`: MySQL 8.0, crea la base `taskmanager` i inicialitza el directori `db/`.
- Xarxa Docker interna perquè `web` arribi a `db` a través de `DB_HOST=db`.

## Requisits previs

- Docker i Docker Compose instal·lats.
- GitHub repository públic amb `.github/workflows/deploy.yml`.
- Secrets configurats per al desplegament SSH.

## Instal·lació i posada en marxa

1. Clona el repositori:
   ```
   git clone https://github.com/ianlopez7e7-ui/desplegamentWeb_2a_convocatoria.git
   cd desplegamentWeb_2a_convocatoria
   ```
2. Executa el servei localment amb Docker:
   ```
   docker compose up -d
   ```
3. Obre el navegador a `http://localhost:8080`.

## Verificació

### Provar que funciona

- `curl -I http://localhost:8080`
- `wget --spider http://localhost:8080` ha de comprovar que l'URL existeix.
- `ss -tlnp | grep 8080` ha de mostrar Apache escoltant a `0.0.0.0:8080`.
- `docker compose logs web --tail 20` ha de mostrar que Apache està configurat i serveix peticions.

### Desplegament GitHub Actions

El workflow `.github/workflows/deploy.yml` s'executa en push a `main` en un runner `ubuntu-latest`.
S'utilitza `appleboy/ssh-action@v0.1.7` per connectar-se al servidor remot amb secrets.

Secrets requerits:
- `EC2_HOST`
- `EC2_USER`
- `EC2_SSH_KEY`

L'script fa un `git pull` a `/var/www/html` i reinicia Apache.