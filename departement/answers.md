# Exercices SQL - Requêtes sur les communes françaises (avec solutions)

## Question 1
**Obtenir la liste des 10 villes les plus peuplées en 2012**

Cette requête trie les villes par population en ordre décroissant et limite les résultats aux 10 premières.

```sql
SELECT *
FROM villes
ORDER BY population_2012 DESC
LIMIT 10;
```

## Question 2
**Obtenir la liste des 50 villes ayant la plus faible superficie**

Cette requête trie les villes par superficie en ordre croissant pour trouver celles avec la plus petite surface.

```sql
SELECT *
FROM villes
ORDER BY surface ASC
LIMIT 50;
```

## Question 3
**Obtenir la liste des départements d'outres-mer, c'est-à-dire ceux dont le numéro de département commencent par "97"**

Les départements d'outre-mer ont tous un code commençant par 97, cette requête utilise LIKE pour filtrer.

```sql
SELECT *
FROM departement
WHERE departement_code LIKE '97%';
```

## Question 4
**Obtenir le nom des 10 villes les plus peuplées en 2012, ainsi que le nom du département associé**

Cette requête utilise une jointure pour associer les villes à leur département.

```sql
SELECT villes.name, departement.departement_nom
FROM villes
JOIN departement ON departement.departement_code = villes.department
ORDER BY villes.population_2012 DESC
LIMIT 10;
```

## Question 5
**Obtenir la liste du nom de chaque département, associé à son code et du nombre de commune au sein de ces département, en triant afin d'obtenir en priorité les départements qui possèdent le plus de communes**

Cette requête utilise GROUP BY pour regrouper les communes par département et compte le nombre de communes dans chaque groupe.

```sql
SELECT departement.departement_nom, villes.department, COUNT(*) AS nbr_items
FROM villes
JOIN departement ON departement.departement_code = villes.department
GROUP BY villes.department, departement.departement_nom
ORDER BY nbr_items DESC;
```

## Question 6
**Obtenir la liste des 10 plus grands départements, en terme de superficie**

Cette requête calcule la superficie totale de chaque département en additionnant la superficie de toutes ses communes.

```sql
SELECT departement.departement_nom, villes.department, SUM(villes.surface) AS dpt_surface
FROM villes
JOIN departement ON departement.departement_code = villes.department
GROUP BY villes.department, departement.departement_nom
ORDER BY dpt_surface DESC
LIMIT 10;
```

## Question 7
**Compter le nombre de villes dont le nom commence par "Saint"**

Cette requête compte les communes dont le nom commence par "Saint" en utilisant le motif LIKE.

```sql
SELECT COUNT(*)
FROM villes
WHERE name LIKE 'Saint%';
```

## Question 8
**Obtenir la liste des villes qui ont un nom existants plusieurs fois, et trier afin d'obtenir en premier celles dont le nom est le plus souvent utilisé par plusieurs communes**

Cette requête identifie les noms de communes qui apparaissent plusieurs fois, les compte et les trie du plus fréquent au moins fréquent.

```sql
SELECT name, COUNT(*) AS nbt_item
FROM villes
GROUP BY name
HAVING COUNT(*) > 1
ORDER BY nbt_item DESC;
```

## Question 9
**Obtenir en une seule requête SQL la liste des villes dont la superficie est supérieur à la superficie moyenne**

Cette requête utilise une sous-requête pour calculer la superficie moyenne, puis sélectionne les villes dont la superficie est supérieure à cette moyenne.

```sql
SELECT *
FROM villes
WHERE surface > (SELECT AVG(surface) FROM villes);
```

## Question 10
**Obtenir la liste des départements qui possèdent plus de 2 millions d'habitants**

Cette requête regroupe les populations par département et filtre ceux qui dépassent 2 millions d'habitants.

```sql
SELECT department, SUM(population_2012) AS population_2012
FROM villes
GROUP BY department
HAVING SUM(population_2012) > 2000000
ORDER BY population_2012 DESC;
```

## Question 11
**Remplacez les tirets par un espace vide, pour toutes les villes commençant par "SAINT-" (dans la colonne qui contient les noms en majuscule)**

Cette requête de mise à jour remplace les tirets par des espaces dans les noms des communes commençant par "SAINT-".

```sql
UPDATE villes
SET name = REPLACE(name, '-', ' ')
WHERE name LIKE 'SAINT-%';
```
