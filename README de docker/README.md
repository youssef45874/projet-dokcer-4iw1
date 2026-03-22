# README

## 1. Présentation du projet
Application e-commerce découpée en microservices (frontend, auth, product, order), chaque service dans son conteneur, avec une base MongoDB dédiée.

---

## 2. Construction et exécution des conteneurs

### Développement
```sh
docker compose up --build
```
- Accès : http://localhost:8080

### Initialisation des produits (dev et prod)
Au premier lancement, la base produits est vide.  
Il faut exécuter :
```sh
bash ./scripts/init-products.sh
```
Ce script ajoute les produits par défaut.

---

### Production (sur VPS)

#### 1. Construire les images de production
```sh
docker build -t ecommerce/auth-service:prod ./services/auth-service
docker build -t ecommerce/product-service:prod ./services/product-service
docker build -t ecommerce/order-service:prod ./services/order-service
docker build -t ecommerce/frontend:prod ./frontend
```

#### 2. Lancer l’environnement de production
```sh
docker compose -f docker-compose.prod.yml up -d
```

#### 3. Initialiser les produits
```sh
bash ./scripts/init-products.sh
```

#### 4. Arrêter l’environnement de production
```sh
docker compose -f docker-compose.prod.yml down
```

---

## 3. Configurations spécifiques

- Variables d’environnement dans `.env` (dev) et via `secrets` (prod)
- Réseau Docker dédié : `ecommerce-network`
- Volumes pour MongoDB : persistance des données
- Secrets pour JWT en production

---

## 4. Commandes pour tester les services

- Tester l’API Auth :
  ```sh
  curl -X POST http://localhost:3001/api/auth/register -d '{"email":"test@test.com","password":"1234"}' -H "Content-Type: application/json"
  ```
- Tester les produits :
  ```sh
  curl http://localhost:3000/api/products
  ```
- Tester le frontend : ouvrir http://localhost:8080

---

## 5. Bonnes pratiques appliquées

- 1 service = 1 conteneur (microservices)
- Multi-stage build et utilisateur non-root dans les Dockerfiles
- Volumes pour la persistance MongoDB
- Secrets pour les infos sensibles (JWT)
- Healthchecks sur chaque service
- Pas de ports inutiles exposés en production
- Réseau bridge pour l’isolation
