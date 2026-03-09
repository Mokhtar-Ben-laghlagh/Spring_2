# Injection de Dépendances avec Spring — IoC par Annotations

## 📁 Structure du projet

```
src/
├── dao/
│   ├── IDao.java           ← Interface DAO
│   ├── DaoImpl.java        ← Bean Spring "dao"     (retourne 100.0)
│   └── DaoImpl2.java       ← Bean Spring "dao2"    (retourne 150.0)
├── metier/
│   ├── IMetier.java        ← Interface Métier
│   └── MetierImpl.java     ← Bean Spring "metier"  (valeur × 2)
└── presentation/
    └── Presentation2.java  ← Point d'entrée + Configuration Spring

pom.xml                     ← Dépendances Maven (Spring Context)
```

---

## ⚙️ Dépendance Maven (`pom.xml`)

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>6.1.0</version>
</dependency>
```

---

## 🚀 Lancer le projet

### Avec Maven
```bash
mvn compile
mvn exec:java -Dexec.mainClass="presentation.Presentation2"
```

### Avec un IDE (IntelliJ / Eclipse)
Clic droit sur `Presentation2.java` → **Run**

---

## 📝 Résultat attendu

```
Résultat = 200.0
```

> `DaoImpl` retourne **100.0** × 2 = **200.0**

---

## 📚 Annotations utilisées

| Annotation                            | Classe          | Rôle                                       |
|---------------------------------------|-----------------|--------------------------------------------|
| `@Configuration`                      | `Presentation2` | Indique une classe de configuration Spring |
| `@ComponentScan`                      | `Presentation2` | Scanne les packages `dao` et `metier`      |
| `@Component("dao")`                   | `DaoImpl`       | Déclare un bean Spring nommé "dao"         |
| `@Component("dao2")`                  | `DaoImpl2`      | Déclare un bean Spring nommé "dao2"        |
| `@Component("metier")`                | `MetierImpl`    | Déclare un bean Spring nommé "metier"      |
| `@Autowired`                          | `MetierImpl`    | Injection automatique de `IDao` par Spring |

---

## 🔄 Comment ça fonctionne

```
Presentation2 (main)
    ↓
AnnotationConfigApplicationContext  → Spring scanne dao/ et metier/
    ↓
@Component("dao")    → Spring crée un bean DaoImpl
@Component("metier") → Spring crée un bean MetierImpl
    ↓
@Autowired IDao dao  → Spring injecte DaoImpl dans MetierImpl
    ↓
metier.calcul()      → dao.getValue() × 2 = 200.0
```

---

<img width="398" height="213" alt="Capture d&#39;écran 2026-03-09 142308" src="https://github.com/user-attachments/assets/b6615f13-7654-4af8-9527-d018493984ab" />


## 🔀 Changer l'implémentation DAO

Pour utiliser `DaoImpl2` (150.0) à la place de `DaoImpl`, ajouter `@Qualifier` dans `MetierImpl.java` :

```java
@Autowired
@Qualifier("dao2")   // ← spécifie quel bean injecter
private IDao dao;
```

Nouveau résultat : `150.0 × 2 = 300.0`

<img width="448" height="235" alt="Capture d&#39;écran 2026-03-09 142659" src="https://github.com/user-attachments/assets/6a629c64-d946-434a-b974-f5d13978a784" />
