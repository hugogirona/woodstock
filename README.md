
## Cahier des charges — WoodStock (application de gestion de stock pour un négoce de bois)

## 1. Contexte de l’application
WoodStock est une application web CRUD conçue pour un négoce de bois. Le but est de gérer de manière claire et fiable le stock, les achats et les ventes, tout en réduisant les erreurs de saisie liées aux quantités, unités et documents.

L’application propose **une seule interface**. Selon le rôle de l’utilisateur, certains menus, boutons et actions sont affichés ou non, et certaines opérations sont autorisées ou bloquées.

---

## 2. Gestion des accès (une interface, rôles et permissions)
### Rôles
* **Admin (bureau)** : paramétrage et supervision (référentiels, ventes, achats, validations, dashboard, exports).
* **Employé (parc)** : saisie opérationnelle et consultation (réceptions, livraisons, stock, historique), avec des droits limités.

### Principe
* Le rôle détermine :
    * ce qui est **visible** (menus, écrans),
    * ce qui est **modifiable**,
    * ce qui est **validable** (actions sensibles).

Exemples :
* Un Employé peut encoder une réception/livraison, mais la **suppression** ou certaines **validations** peuvent être réservées à l’Admin.
* L’Admin voit le dashboard complet et les exports ; l’Employé voit une version simplifiée (stock, tâches du jour, ses opérations récentes).

---

## 3. Personas et parcours utilisateurs

### 🧑‍🔧 Karim — Responsable commercial (Admin)
**Rôle :** Admin

#### 🧩 Parcours utilisateur 1 — Créer une commande client
Karim reçoit une demande d’un client pour des chevrons et des panneaux trois plis. Il se connecte à WoodStock, ouvre la rubrique “Commandes”, puis crée une nouvelle commande. Il sélectionne le client, ajoute les produits, indique les quantités et choisit l’unité de saisie. Le système propose un prix de vente par défaut, qu’il peut ajuster. Il enregistre la commande puis la confirme.

#### 🧩 Parcours utilisateur 2 — Générer un bon de livraison (commande → Bon de livraison)
Lorsque la commande doit sortir du parc, Karim ouvre la commande confirmée et clique sur “Créer un bon de livraison”. Il peut livrer la commande entièrement ou partiellement. À la validation du bon de livraison, WoodStock enregistre la sortie stock automatiquement et conserve un historique traçable (date, utilisateur, référence du document).

#### 🧩 Parcours utilisateur 3 — Suivre les alertes de stock bas
En fin de journée, Karim consulte le dashboard. Il voit immédiatement les produits sous le seuil minimum et peut ouvrir la liste pour comprendre quels articles doivent être surveillés. Il utilise ensuite l’historique des mouvements pour identifier si la baisse vient d’une livraison récente ou d’une tendance habituelle.

---

### 🧑‍🔧 Yassine — Magasinier (Employé)
**Rôle :** Employé

#### 🧩 Parcours utilisateur 1 — Sortie stock via bon de livraison
Yassine se connecte depuis une tablette sur le parc. Il ouvre la section “Bons de livraison”, retrouve le document à préparer, vérifie les quantités réellement sorties, puis valide selon les autorisations définies (ou soumet pour validation si l’Admin doit valider). Le stock est mis à jour automatiquement et l’opération apparaît dans l’historique.

#### 🧩 Parcours utilisateur 2 — Entrée stock via bon de réception
Un camion arrive avec du lamellé-collé. Yassine ouvre la rubrique “Réceptions”, crée un bon de réception, sélectionne le fournisseur, puis encode les lignes. Si le produit est configuré avec une conversion pièce → \(m^3\), WoodStock convertit automatiquement vers l’unité de base. La réception validée augmente le stock et met à jour l’historique.

#### 🧩 Parcours utilisateur 3 — Contrôle rapide avant chargement
Avant de charger un camion, Yassine recherche un produit sur l’écran “Stock actuel” et vérifie immédiatement la quantité disponible. Il évite ainsi de sortir une quantité incorrecte ou de préparer une livraison impossible.

---

## 4. Fonctionnalités (MVP)

### 4.1 Référentiels (CRUD)
* Produits : référence, désignation, catégorie, emplacement
* Unité de base par produit
* Seuil minimum (alertes stock bas)
* Prix : dernier prix d’achat + prix de vente par défaut
* Catégories
* Unités
* Clients
* Fournisseurs
* Conversions par produit (uniquement si nécessaire)

### 4.2 Ventes (commande → bon de livraison)
* Création et gestion des commandes clients (lignes : produit, quantité, unité, prix, remise)
* Création d’un bon de livraison depuis une commande (total ou partiel)
* Validation du bon de livraison = sortie stock + traçabilité

### 4.3 Achats (bon de réception → entrée stock)
* Création et gestion des bons de réception (lignes : produit, quantité, unité, prix d’achat)
* Validation du bon de réception = entrée stock + traçabilité
* Mise à jour du “dernier prix d’achat” du produit (si inclus dans le MVP)

### 4.4 Stock et suivi
* Stock actuel calculé (entrées − sorties)
* Historique des mouvements filtrable (date, produit, type)
* Liste des produits sous seuil minimum

### 4.5 Dashboard
* Produits sous seuil
* Ventes récentes (ex. 30 derniers jours)
* Dernières opérations (Bon de livraison, réceptions, mouvements)
* Actions rapides

