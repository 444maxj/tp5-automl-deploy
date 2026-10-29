# Modele manuel ou modele AutoML ? (TP05)
| Critere | Manuel (rf 500/16) | AutoML (FLAML 90 s) |
|----------------------------|--------------------|---------------------|
| Famille de modele | foret aleatoire (500 arbres, profondeur max. 16) | XGBoost |
| F1 sur le jeu de test | 0.8669 | 0.8582 |
| Temps de recherche machine | 3,0 s (entrainement) | 90,5 s |
| Temps humain (estimation) | environ 30 min (choix et reglages) | environ 10 min (parametrage et validation) |
| Latence mediane (ms) | 173,0 | 22,6 |
| Latence p95 (ms) | 300,3 | 38,9 |
| Taille du fichier (Mo) | 14,02 | 0,30 |
| Explicabilite | moyenne : importance des variables possible, mais 500 arbres difficiles a interpreter | faible a moyenne : ensemble XGBoost complexe, explicable avec des outils comme SHAP |

*Les latences sont mesurees sur les endpoints HTTP actifs (200 requetes, apres 10 requetes d'echauffement). La taille correspond aux fichiers `joblib` locaux, en Mo decimaux. Les temps humains sont des estimations, pas des mesures.*

## Decision (5 lignes) : quel modele mettre en production, et pourquoi ?
1. Je recommande le modele AutoML (XGBoost) si la priorite est une API rapide et legere.
2. Son F1 de 0.8582 reste proche de celui du modele manuel (0.8669), avec un ecart de 0.0087.
3. Sa latence mediane est de 22,6 ms contre 173,0 ms, et son fichier est environ 47 fois plus petit.
4. Le modele manuel conserve le meilleur F1 ; il est preferable si ce gain est essentiel et si 300,3 ms au p95 convient au service.
5. Avant le deploiement final, valider le seuil de latence et le compromis F1 sur les donnees metier recentes.
