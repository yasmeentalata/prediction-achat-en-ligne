# prediction-achat-en-ligne

Projet personnel d’apprentissage du machine learning : prédire si une session de navigation sur un site marchand
aboutit à un achat.

## Données

Dataset : Online Shoppers Purchasing Intention, UCI
Auteurs : C. Sakar et Y. Kastro (2018)
Source : https://doi.org/10.24432/C5F88Q
Licence des données : CC BY 4.0

- 12 330 sessions.
- 17 variables explicatives disponibles et une cible : Revenue
- Environ 15,5 % des sessions aboutissent à un achat
- Chargement automatique des données depuis UCI

Cette première version utilise six variables : Administrative, Informational, ProductRelated, BounceRates, ExitRates et SpecialDay.

## Méthode

- Exploration des données et vérification des valeurs manquantes
- Comparaison avec une règle prédisant toujours « pas d’achat »
- Standardisation et régression logistique avec class_weight="balanced", regroupées dans une pipeline
- Séparation stratifiée : 60 % apprentissage, 20 % validation et 20 % test
- Comparaison des seuils 0,3, 0,5 et 0,7 sur la validation

## Choix du seuil

Pour illustrer une décision fondée sur le coût des erreurs, j’utilise deux hypothèses fictives :

- Faux positif : coût de 2 €.
- Faux négatif : coût de 10 €.

Le seuil 0,5 minimise le coût sur la validation parmi les trois seuils comparés.

## Résultats sur le test

| Indicateur | Résultat |
| Achats correctement repérés | 269 sur 382 |
| Rappel des achats | 70,4 % |
| Précision des achats | 21,7 % |
| Faux positifs | 973 |
| Faux négatifs | 113 |
| Coût simulé du modèle | 3 076 € |
| Coût de la règle « toujours pas d’achat » | 3 820 € |

La réduction du coût simulé est de 19,5 % par rapport à cette règle simple.

## Limites

- Le modèle produit encore beaucoup de fausses alertes.
- Les coûts sont fictifs : aucune économie réelle en entreprise n’est démontrée.
- Le test a été consulté lors des premiers essais : les résultats restent exploratoires.
- Les scores du modèle pondéré ne sont pas nécessairement des probabilités d’achat bien calibrées.
- Une utilisation en temps réel nécessiterait de vérifier que chaque variable est disponible au moment de la prédiction.

## Exécuter le projet

Ouvrir prediction_achat.ipynb dans Google Colab et exécuter les cellules dans l’ordre.

Une connexion Internet est nécessaire pour télécharger les données. Aucun import manuel du CSV n’est requis.

## Outils

Python, pandas, scikit-learn, Matplotlib, Google Colab et GitHub.
