# Task Manager RA6

## Arquitectura
Aquest projecte és una aplicació web PHP amb Apache i una base de dades MySQL.

- Servei `web`: contenedor basat en `php:8.2-apache`, serveix els fitxers de `src/` i exposa el port `8080` a l'entorn host local.
- Servei `db`: MySQL 8.0, crea la base de dades `taskmanager` i inicialitza el directori `db/`.
- Xarxa Docker interna perquè el servei `web` es connecti al servei `db` amb `DB_HOST=db`.

> La web s'ha desplegat localment i s'accedeix a `http://localhost:8080`.

## Requisits previs

- Docker i Docker Compose disponibles en la màquina local.
- Accés al repositori GitHub i permisos per configurar secrets si es vol provar el desplegament SSH.

## Instal·lació i posada en marxa

1. Clona el repositori:
   ```bash
   git clone https://github.com/ianlopez7e7-ui/desplegamentWeb_2a_convocatoria.git
   cd desplegamentWeb_2a_convocatoria
   ```
2. Inicia els serveis Docker:
   ```bash
   docker compose up -d
   ```
3. Obre el navegador a:
   ```text
   http://localhost:8080
   ```

## Verificació

### Proves fetes
- `curl -I http://localhost:8080` ha de retornar `HTTP/1.1 200 OK`.
- `wget --spider http://localhost:8080` ha de confirmar que l'URL existeix.
- `ss -tlnp | grep 8080` ha de mostrar Apache escoltant a `0.0.0.0:8080`.
- `docker compose logs web --tail 20` ha de confirmar que Apache està en funcionament.

### Com comprovar localment
```bash
curl -I http://localhost:8080
wget --spider http://localhost:8080
ss -tlnp | grep 8080
docker compose logs web --tail 20
```

## Pipeline GitHub Actions

El workflow `.github/workflows/deploy.yml` està configurat per executar-se en `push` a la branca `main` sobre un runner `ubuntu-latest`.

Aquest workflow també inclou un pas amb `appleboy/ssh-action@v0.1.7` per fer el desplegament a un servidor remot via SSH (Que no funciona ja que s'efectua el localhost, però esta present tal i com es va fer a la pràctica i per tindre les condicions que caldrien per efectuar-ho tal i com es demana a l'enunciat: " 
    Que s'activi en fer push a la branca main
    Executi en un runner ubuntu-latest
    Es connecti al servidor EC2 via SSH usant els secrets EC2_HOST, EC2_USER i EC2_SSH_KEY
    Desplegi l'aplicació al servidor (com a mínim: connexió SSH i reinici del servei Apache)
"
 Cosa que ho fa, però en localhost, i amb l'estructura per fes-ho per EC2).

Secrets necessaris per al desplegament remot:
- `EC2_HOST`
- `EC2_USER`
- `EC2_SSH_KEY`

> La part de desplegament local ja s'ha verificat a `localhost`, i el workflow SSH està preparat per connectar-se a un servidor remot quan es configuren correctament els secrets.
