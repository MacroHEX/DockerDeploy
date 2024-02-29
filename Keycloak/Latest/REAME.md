# Keycloak

## Crear Red Docker

```bash
docker network create auth-network
```

## Como crear la imagen

```bash
docker build -t quintokeycloak .
```

## Como correr la imagen

```bash
docker compose up -d
```
