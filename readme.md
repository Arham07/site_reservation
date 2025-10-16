# 🧖‍♀️ Espace Bien-Être — Système de Réservation

Ce projet est l'interface client pour un système de réservation de services de bien-être. Il permet aux utilisateurs de consulter les créneaux disponibles pour différents services (Massage AMMA Assis, Réalité Virtuelle, Luminothérapie, Do In) sur une base hebdomadaire (Mardi et Mercredi) ou journalière, et d'effectuer des réservations. Un mode administrateur sécurisé est également disponible pour la gestion des réservations et la configuration.

## 🛠️ Technologies Utilisées

* **Frontend :** HTML, CSS (styles intégrés dans le `<style>` pour une application légère), JavaScript (Vanilla JS).
* **Backend / Communication :** API REST (pour la configuration et les données de réservation), **Socket.IO** (pour les mises à jour en temps réel des réservations).

## 💡 Fonctionnalités

### **Mode Utilisateur**

* **Vue Semaine :** Affiche les créneaux disponibles pour le Mardi et le Mercredi de la semaine sélectionnée.
* **Vue Tableau (Jour) :** Fournit une vue détaillée par créneau et par service pour un jour spécifique.
* **Réservation :** Permet d'ajouter une réservation en entrant un identifiant de type `prenom.nom` (validé comme une adresse email interne `@cpn-laxou.com`).
* **Thèmes et Palettes :** Choix entre un mode jour/nuit (dark/light mode) et plusieurs palettes de couleurs (Teal, Sauge, Sable chaud, Ardoise).
* **Navigation :** Permet de passer aux semaines précédente et suivante, ou de sélectionner une date directement via un calendrier.

---

### **Mode Administrateur (Admin)**

Accessible via un PIN sécurisé (`1234` par défaut dans le code).

* **Gestion des Réservations :** Possibilité de vider un créneau (`Vider`) ou de retirer une seule réservation (`✕` sur le nom).
* **Configuration :** Bouton pour accéder à la page de configuration (`config.html`).
* **Réinitialisation :** Bouton dangereux pour effacer **toutes** les réservations.
* **Export CSV :** Permet d'exporter toutes les données de réservation affichées.

---

## ⚙️ Configuration (Code Client)

Les paramètres suivants sont définis directement dans le script et dépendent du backend :

| Variable | Description | Valeur par Défaut |
| :--- | :--- | :--- |
| `API_BASE` | URL de base de l'API backend. | `""` (relatif) |
| `ADMIN_SECRET_HEADER_VALUE` | Clé secrète utilisée dans le header `X-Admin-Secret` pour les actions admin. | `"INFO0035"` |
| `ADMIN_PIN` | Code PIN pour activer le mode Admin côté client (doit correspondre à la validation serveur si implémentée). | `"1234"` |

### **Services et Capacités**

Les services, leurs clés, labels et capacités par défaut sont définis dans la fonction `applyConfig` et peuvent être mis à jour par le backend via l'API `/api/config`.

* **Services :** `amma` (Massage AMMA Assis), `rv` (Réalité Virtuelle), `lumo` (Luminothérapie), `doin` (Do In).
* **`doin`** est configuré par défaut comme un service **grisé/non réservable** (`grey: true`).

## 🚀 Démarrage

Ce fichier `index.html` est l'interface utilisateur. Pour que l'application soit fonctionnelle, un **serveur backend** doit être en cours d'exécution et accessible via `API_BASE`, et doit implémenter les endpoints suivants :

* `GET /api/config` : Charger la configuration (services, capacités, jours, heures, périodes de fermeture).
* `GET /api/week?from=...&to=...` : Récupérer les réservations pour la semaine.
* `POST /api/slot/{id}/add` : Ajouter une réservation à un créneau.
* `DELETE /api/slot/{id}` : Vider un créneau (Admin).
* `POST /api/slot/{id}/remove` : Retirer une réservation par index (Admin).
* `DELETE /api/reset` : Réinitialiser toutes les données de réservation (Admin).

Le serveur doit également exposer l'interface **Socket.IO** pour les mises à jour en temps réel.
