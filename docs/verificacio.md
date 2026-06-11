# Verificació del servidor i proves realitzades

## 1. curl
- Comanda:
  ```
  curl -I http://localhost:8080
  ```
- Resultat esperat:
  - Resposta HTTP 200 OK i capçalera del servidor.
- Resultat obtingut:
  - `HTTP/1.1 200 OK`
  - `Server: Apache/2.4.67 (Debian)`
  - `X-Powered-By: PHP/8.2.31`

## 2. wget
- Comanda:
  ```
  wget --spider http://localhost:8080
  ```
- Resultat esperat:
  - Comprovació que l'URL és accessible i es pot connectar.
- Resultat obtingut:
  - `Remote file exists and could contain actual data.`

## 3. ss
- Comanda:
  ```
  ss -tlnp | grep -E ':80|:8080|:22'
  ```
- Resultat esperat:
  - Veure el servei Apache escoltant al port exposat `8080`.
- Resultat obtingut:
  - `LISTEN 0      4096         0.0.0.0:8080       0.0.0.0:*`
  - `LISTEN 0      4096            [::]:8080          [::]:*`

## 4. logs del container Apache
- Comanda:
  ```
  docker compose logs web --tail 20
  ```
- Resultat esperat:
  - Apache s'executa i atén peticions HTTP 200.
- Resultat obtingut:
  - `AH00558: apache2: Could not reliably determine the server's fully qualified domain name...`
  - `Apache/2.4.67 (Debian) PHP/8.2.31 configured -- resuming normal operations`
  - `HEAD / HTTP/1.1 200 154 - "curl/8.5.0"`
  - `HEAD / HTTP/1.1 200 210 - "Wget/1.21.4"`
