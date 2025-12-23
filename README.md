# TP18 - Service gRPC avec Spring Boot

Service gRPC pour la gestion de comptes bancaires implémenté avec Spring Boot selon les spécifications du cours MLIAEdu.

## 📋 Description

Ce projet implémente un service gRPC complet permettant de :
- Créer des comptes bancaires (COURANT ou EPARGNE)
- Consulter tous les comptes
- Consulter un compte par ID
- Calculer les statistiques de solde (count, sum, average)

## 🏗️ Architecture

```
grpc2/
├── src/
│   └── main/
│       ├── java/ma/projet/grpc/
│       │   ├── entities/           # Entités JPA
│       │   │   └── Compte.java
│       │   ├── repositories/       # Repositories Spring Data
│       │   │   └── CompteRepository.java
│       │   ├── services/           # Couche service
│       │   │   └── CompteService.java
│       │   ├── controllers/        # Services gRPC
│       │   │   └── CompteServiceImpl.java
│       │   └── Grpc2Application.java  # Main Spring Boot
│       ├── proto/
│       │   └── CompteService.proto # Définition Protobuf
│       └── resources/
│           └── application.properties
└── pom.xml
```

## 🛠️ Technologies Utilisées

- **Spring Boot** 3.1.5
- **gRPC** 1.53.0
- **Protocol Buffers** 3.21.12
- **H2 Database** (en mémoire)
- **Spring Data JPA**
- **Java** 17

## 📦 Dépendances Principales

### Spring Boot
- `spring-boot-starter-data-jpa` - Pour la persistance
- `spring-boot-starter-web` - Pour les fonctionnalités web

### gRPC
- `grpc-netty-shaded` - Transport réseau
- `grpc-protobuf` - Génération des stubs
- `grpc-stub` - Classes de base gRPC
- `grpc-server-spring-boot-starter` - Intégration Spring Boot

### Base de données
- `h2` - Base de données en mémoire

## 🚀 Compilation et Exécution

### Compilation
```bash
# Compiler le projet et générer les stubs gRPC
.\mvnw.cmd clean compile

# Package l'application
.\mvnw.cmd package
```

### Exécution
```bash
# Lancer l'application
.\mvnw.cmd spring-boot:run
```

Le serveur gRPC démarre sur le port **9090**.

## 📡 API gRPC

### Service: CompteService

#### 1. AllComptes
Récupère tous les comptes bancaires.

**Request:** `GetAllComptesRequest` (vide)  
**Response:** `GetAllComptesResponse`
```protobuf
message GetAllComptesResponse {
    repeated Compte comptes = 1;
}
```

#### 2. CompteById
Récupère un compte par son ID.

**Request:** 
```protobuf
message GetCompteByIdRequest {
    string id = 1;
}
```
**Response:** `GetCompteByIdResponse`

#### 3. TotalSolde
Calcule les statistiques des soldes.

**Request:** `GetTotalSoldeRequest` (vide)  
**Response:**
```protobuf
message GetTotalSoldeResponse {
    SoldeStats stats = 1;  // count, sum, average
}
```

#### 4. SaveCompte
Crée un nouveau compte bancaire.

**Request:**
```protobuf
message SaveCompteRequest {
    CompteRequest compte = 1;  // solde, dateCreation, type
}
```
**Response:** `SaveCompteResponse`

## 🧪 Test avec BloomRPC

### Installation
1. Télécharger [BloomRPC](https://github.com/bloomrpc/bloomrpc/releases)
2. Installer l'application

### Configuration
1. Ouvrir BloomRPC
2. Cliquer sur **File > Import Protobuf**
3. Sélectionner `src/main/proto/CompteService.proto`
4. Configurer l'adresse: `localhost:9090`

### Exemples de Requêtes

#### Créer un compte
```json
{
  "compte": {
    "solde": 15000,
    "dateCreation": "2025-12-23",
    "type": "COURANT"
  }
}
```

#### Récupérer tous les comptes
```json
{}
```

#### Récupérer un compte par ID
```json
{
  "id": "abc123-def456"
}
```

#### Obtenir les statistiques
```json
{}
```

## 🗄️ Configuration Base de Données

### H2 (par défaut)
```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
```

### Console H2
Accessible sur: `http://localhost:8080/h2-console`

## 📝 Structure des Données

### Entité Compte
```java
@Entity
public class Compte {
    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private String id;
    private float solde;
    private String dateCreation;
    private String type;  // "COURANT" ou "EPARGNE"
}
```

### Message Protobuf
```protobuf
message Compte {
    string id = 1;
    float solde = 2;
    string dateCreation = 3;
    TypeCompte type = 4;
}

enum TypeCompte {
    COURANT = 0;
    EPARGNE = 1;
}
```

## 🔧 Dépannage

### Maven Wrapper non exécutable
```bash
# Windows PowerShell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

### Port déjà utilisé
Modifier dans `application.properties`:
```properties
grpc.server.port=9091
```

### Erreurs de compilation
```bash
# Nettoyer et recompiler
.\mvnw.cmd clean install -DskipTests
```

## 📚 Ressources

- [Documentation gRPC](https://grpc.io/docs/)
- [Spring Boot gRPC Starter](https://github.com/yidongnan/grpc-spring-boot-starter)
- [Protocol Buffers](https://developers.google.com/protocol-buffers)
- [BloomRPC](https://github.com/bloomrpc/bloomrpc)

## 👥 Auteur

Projet réalisé dans le cadre du **TP18 - Architecture Microservices** sur MLIAEdu.

## 📄 Licence

Ce projet est à des fins éducatives uniquement.
