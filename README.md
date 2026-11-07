# Boutique Diayma

## 1. Bugs constatés

### Bug 1 : Calcul du total

Lorsque le même produit est ajouté plusieurs fois au panier, le total des prix ne s'additionne pas correctement.

### Bug 2 : Gestion du stock

Après validation d'une commande, le stock du produit ne diminue pas.

---

## 2. Points d'arrêt et débogage

Les points d'arrêt suivants ont été placés dans les fichiers demandés afin d'observer l'exécution de l'application.

### 2.1. CartSummaryViewComponent

- **Namespace :** `P2FixAnAppDotNetCode.Components`
- **Classe :** `CartSummaryViewComponent`
- **Méthode :** `Invoke()`

Ce composant intervient dans l'affichage du résumé du panier.

### 2.2. ProductController

- **Namespace :** `P2FixAnAppDotNetCode.Controllers`
- **Classe :** `ProductController`
- **Méthode :** `Index()`
- **Ligne 21 :** récupère la liste des produits depuis `ProductService` avec `GetAllProducts()`.
- **Ligne 22 :** transmet la liste des produits à la vue avec `View(products)`.

### 2.3. OrderController

- **Namespace :** `P2FixAnAppDotNetCode.Controllers`
- **Classe :** `OrderController`
- **Constructeur :** `OrderController(ICart pCart, IOrderService service, IStringLocalizer<OrderController> localizer)`
- **Ligne 17 :** affecte `pCart` à l'attribut `_cart`.
- **Ligne 18 :** affecte `service` à l'attribut `_orderService`.
- **Ligne 19 :** affecte `localizer` à l'attribut `_localizer`.

### 2.4. CartController

- **Namespace :** `P2FixAnAppDotNetCode.Controllers`
- **Classe :** `CartController`
- **Constructeur :** `CartController(ICart pCart, IProductService productService)`
- **Ligne 15 :** affecte `pCart` à l'attribut `_cart`.
- **Ligne 16 :** affecte `productService` à l'attribut `_productService`.

### 2.5. Startup

- **Namespace :** `P2FixAnAppDotNetCode`
- **Classe :** `Startup`
- **Constructeur :** `Startup(IConfiguration configuration)`
- **Ligne 20 :** affecte le paramètre `configuration` à la propriété `Configuration`.

---

## 3. Question 8 : Parcours d'affichage des produits

Le débogage a permis de suivre le parcours d'exécution depuis le contrôleur jusqu'au dépôt des produits, puis le retour des données vers la vue.

### 3.1. ProductController

- **Namespace :** `P2FixAnAppDotNetCode.Controllers`
- **Classe :** `ProductController`
- **Méthode :** `Index()`

La méthode `Index()` appelle `GetAllProducts()` pour récupérer la liste des produits depuis le service.

Ensuite, elle transmet cette liste à la vue avec l'instruction `View(products)`.

### 3.2. ProductService

- **Namespace :** `P2FixAnAppDotNetCode.Models.Services`
- **Classe :** `ProductService`
- **Méthode :** `GetAllProducts()`

Cette méthode appelle `ProductRepository.GetAllProducts()` pour récupérer les produits.

Elle convertit ensuite le résultat en liste avec `ToList()`.

### 3.3. ProductRepository

- **Namespace :** `P2FixAnAppDotNetCode.Models.Repositories`
- **Classe :** `ProductRepository`
- **Méthode :** `GetAllProducts()`

Cette méthode :

- Récupère les produits dont le stock est supérieur à 0.
- Trie les produits par ordre alphabétique de leur nom.
- Retourne les produits sous forme de tableau avec `ToArray()`.

### 3.4. Vue

- **Fichier :** `Views/Product/Index.cshtml`

La vue reçoit la liste des produits transmise par `ProductController` et les affiche dans le navigateur.

### 3.5. Résultat observé

Pendant le débogage, la variable `products` contenait **9 produits** au niveau de la méthode `ProductController.Index()`.

---

## 4. Modes de débogage utilisés

Les trois modes de débogage suivants ont été utilisés pour suivre l'exécution du programme.

### Step Into (F11)

Permet d'entrer dans une méthode appelée afin d'observer son fonctionnement.

Il a notamment permis de suivre les appels de `ProductController` vers `ProductService`, puis vers `ProductRepository`.

### Step Over (F10)

Permet d'exécuter l'instruction courante sans entrer dans les méthodes appelées.

Il a été utilisé pour avancer dans l'exécution des instructions.

### Step Out (Shift + F11)

Permet de terminer l'exécution de la méthode courante et de revenir à la méthode appelante.

Il a été utilisé pour revenir de `ProductRepository` vers `ProductService`, puis vers `ProductController`.
