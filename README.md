# Prédiction d’achat en e-commerce

Projet personnel de machine learning visant à prédire si une session de navigation sur un site e-commerce aboutira à un achat.

📓 [Consulter le notebook et ses résultats](prediction_achat.ipynb)

## Données

Jeu de données : **Online Shoppers Purchasing Intention**, UCI.
- 12 330 sessions de navigation.
- 17 variables explicatives disponibles et une cible : `Revenue`.
- Environ 15,5 % de sessions avec achat.
- Aucune valeur manquante.

Six variables sont utilisées pour cette première étude :
`Administrative`, `Informational`, `ProductRelated`,
`BounceRates`, `ExitRates` et `SpecialDay`.

Source : C. O. Sakar et Y. Kastro (2018).
https://doi.org/10.24432/C5F88Q
Licence des données : CC BY 4.0.

## Méthode

- Modèle de référence prédisant systématiquement « pas d’achat ».
- Régression logistique avec standardisation, avec et sans pondération des classes.
- Découpage stratifié : 60 % apprentissage, 20 % validation, 20 % test.
- Comparaison de trois seuils sur la validation : 0,3, 0,5 et 0,7.
- Simulation de coûts : 2 € par faux positif et 10 € par faux négatif.
- Analyse des coefficients et de la corrélation entre variables.
- Comparaison avec un arbre de décision de profondeur maximale 3.

Les premiers essais ont utilisé le découpage initial 80 % entraînement / 20 % test. L’ensemble d’entraînement a ensuite été partagé pour constituer la validation.

## Comparaison sur la validation

Résultats au seuil de 0,5 :

| Modèle | Précision | Rappel | Coût simulé |
|---|---:|---:|---:|
| Régression logistique pondérée | 23,0 % | 79,5 % | 2 814 € |
| Arbre de décision pondéré | 23,6 % | 74,0 % | 2 818 € |

Les coûts sont presque identiques. La régression logistique détecte davantage d’achats, mais produit plus de fausses alertes.

Le retrait de `BounceRates`, fortement corrélée à `ExitRates` (0,91 sur l’apprentissage), modifie peu les résultats de validation.

## Résultats sur le test exploratoire

Modèle retenu : **régression logistique pondérée, seuil de 0,5**.

| Indicateur | Résultat |
|---|---:|
| Achats détectés | 269 sur 382 |
| Rappel | 70,4 % |
| Précision | 21,7 % |
| Faux positifs | 973 |
| Faux négatifs | 113 |
| Coût simulé du modèle | 3 076 € |
| Coût simulé de la règle « toujours pas d’achat » | 3 820 € |

Avec ces hypothèses, le coût simulé diminue de **19,5 %** par rapport à la règle « toujours pas d’achat ». Il ne s’agit pas d’économies réellement mesurées.

## Limites

- La précision reste faible et les fausses alertes nombreuses.
- Les coûts d’erreur sont fictifs.
- Le test a été consulté lors des premiers essais : l’évaluation reste exploratoire.
- Les résultats reposent sur six variables et un seul découpage.
- Les scores du modèle pondéré ne sont pas nécessairement des probabilités bien calibrées.
- Une utilisation en temps réel nécessiterait de vérifier la disponibilité des variables au moment de prédire.

## Exécuter le projet

Ouvrir `prediction_achat.ipynb` dans Google Colab, puis exécuter les cellules dans l’ordre. Les données sont téléchargées automatiquement depuis UCI ; une connexion Internet est nécessaire.

Bibliothèques utilisées : **pandas, scikit-learn et Matplotlib**.

## Pistes d’amélioration

- Évaluer la stabilité des résultats par validation croisée.
- Étudier d’autres variables disponibles au moment de la prédiction.
- Définir les coûts d’erreur à partir d’un besoin métier réel.
