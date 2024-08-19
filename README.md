<h1 align="center">Time Series Forecasting</h1>

<h3 align="center">Regroupement des variations de la consommation d’énergie des appartements</h3>
<h4 align="center">
    <p>
        <a href="#objectif">Objectif</a> •
        <a href="#données">Données</a> •
        <a href="#dépendances">Dépendances</a> •
        <a href="#exemple-de-code">Exemple de code</a> •
        <a href="#conclusion">Conclusion</a>
    </p>
</h4>

### Objectif

L’objectif de ce projet est d’utiliser la classification non supervisée pour résumer les variations de la consommation d’énergie de 100 appartements, observée toutes les 30 minutes durant 91 jours consécutifs. Plus spécifiquement, on cherche ici à obtenir une classification des jours, uniforme pour l’ensemble des appartements. Une telle classification pourra être utile, par exemple, dans le cadre de la surveillance d’un réseau d’électricité.

### Données

Les données utilisées dans ce projet représentent la consommation d'énergie de 100 appartements, mesurée toutes les 30 minutes sur une période de 91 jours consécutifs. Chaque ligne du jeu de données correspond à une mesure de consommation pour un appartement à un moment donné.


- **Source des données**: Les données peuvent être obtenues à partir de [lien vers la source des données].
- **Format des données**: Les données sont au format texte avec les fichiers suivants :

    - [`APPART.txt`]("donnees/APPART.txt"): Contient les identifiants des appartements. Chaque ligne correspond à un appartement.
  
    ```plaintext
    1
    1
    1
    1
    ...
    ```

    - [`JOUR.txt`]("donnees/JOUR.txt"): Contient les identifiants des jours. Chaque ligne correspond à un jour.

    ```plaintext
    1
    2
    3
   ...
    ```

    - [`X.txt`]("donnees/X.txt"): Contient les mesures de consommation d'énergie. Chaque ligne correspond à une mesure de consommation pour un appartement à un moment donné.   

    ```plaintext
    0.123 0.456 0.789 ...
    0.234 0.567 0.890 ...
    ...
    ```


### Dépendances

Pour exécuter ce projet, les dépendances suivantes sont nécessaires :

- `matplotlib`
- `pandas`
- `scikit-learn`
- `jupyter`

Vous pouvez installer ces dépendances en utilisant le fichier `requirements.txt` fourni :

```bash
pip install -r requirements.txt
```

#### Exemple de code
Voici un exemple de code pour exécuter le clustering KMeans et générer une heatmap des résultats :


```Python
from sklearn.cluster import KMeans
import matplotlib.pyplot as plt
import pandas as pd
from matplotlib.colors import ListedColormap

# Chargement des données
X = pd.read_csv('path_to_data/X.txt', sep=" ", header=None)

# Exécution du clustering KMeans
kmeans = KMeans(n_clusters=3, random_state=42, n_init='auto')
kmeans.fit(X)

# Transformation des données pour la visualisation
Y = pd.DataFrame(columns=range(91))
for i in range(100):
    Y.loc[i] = kmeans.labels_[i * 91: (i + 1) * 91]

# Création d'une colormap personnalisée
couleurs = ListedColormap(["#962428", "#fefdc5", "#374b99", "#7db9ab"])

# Génération de la heatmap
plt.figure(figsize=(20, 10))
plt.imshow(Y, aspect='auto', cmap=couleurs)
plt.xlabel('Days')
plt.ylabel('Flats')
plt.title('Heatmap des données journalières')
plt.colorbar()
plt.show()
```


### Conclusion
Ce projet démontre comment utiliser la classification non supervisée pour analyser les variations de la consommation d'énergie dans des appartements. En utilisant l'algorithme KMeans, nous avons pu regrouper les jours en fonction des schémas de consommation d'énergie, ce qui peut être utile pour la surveillance et l'optimisation des réseaux électriques. Les résultats sont visualisés à l'aide d'une heatmap, offrant une représentation claire et intuitive des clusters formés