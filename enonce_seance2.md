# Séance 2 : travaux pratiques

**Durée** : environ 1 h 30 de pratique, en alternance avec les diapos.
**Fil rouge** : le dépôt `recettes/` de la séance 1, que tu enverras sur GitHub en séance 3.
**Matériel** : un terminal (**Git Bash** sous Windows) et nano comme éditeur.

## Sommaire

| Partie | Sujet | Durée |
|---|---|---|
| 1 | Branches : `branch`, `switch` | 18 min |
| 2 | `merge` : fast-forward et fusion à trois points | 20 min |
| 3 | Conflits | 28 min |
| 4 | `stash` et `tag` | 10 min |
| 5 | `rebase` | 12 min |

**Point de départ** : `cd recettes`, puis `git status` doit afficher un dépôt propre sur la branche `main`.

Conseil : tape `git log --oneline --graph --all` très souvent. C'est la meilleure façon de voir ce que tu fais.

---

## Partie 1 : branches (18 min)

1. Liste les branches avec `git branch`. Combien y en a-t-il ?
2. Crée la branche `ajout-pizza` et place-toi dessus en une seule commande.
3. Crée `pizza.md` (titre et quelques ingrédients), ajoute-le et commite : `Ajoute la recette de la pizza`.
4. Reviens sur `main` : `git switch main`. Regarde ton dossier : que s'est-il passé pour `pizza.md` ?
5. Affiche `git branch -v`, puis `git log --oneline --graph --all`. Sur quel commit pointe chaque branche ?
6. Retourne sur `ajout-pizza` : `pizza.md` est-il revenu ? Reviens ensuite sur `main`.

### Critères de réussite

- [ ] `git branch` liste `main` et `ajout-pizza`.
- [ ] Sur `main`, `pizza.md` n'existe pas ; sur `ajout-pizza`, il existe.
- [ ] Tu sais dire quelle branche est la branche courante.
- [ ] Tu sais lire `git log --oneline --graph --all`.

---

## Partie 2 : `merge` (20 min)

### 2.1 Fast-forward

1. Tu es sur `main`, qui n'a pas bougé depuis la création de `ajout-pizza`. Fusionne la branche.
2. Regarde le graphe : y a-t-il un commit de fusion ? Pourquoi ?
3. Supprime la branche `ajout-pizza`.

### 2.2 Fusion à trois points

1. Crée la branche `ajout-soupe`, ajoute `soupe.md`, commite.
2. Reviens sur `main` et ajoute une section `## Sommaire` à la fin du `README.md` (liste des recettes). Commite.
3. Fusionne `ajout-soupe` dans `main` avec `--no-edit`.
4. Regarde le graphe : que vois-tu cette fois ? Supprime la branche.

### Critères de réussite

- [ ] Après 2.1, `git log --oneline --graph --all` est une ligne droite.
- [ ] Après 2.2, le graphe montre une fusion avec deux parents.
- [ ] `pizza.md` et `soupe.md` existent sur `main`.
- [ ] Tu sais expliquer la différence entre fast-forward et fusion à trois points.

---

## Partie 3 : conflits (28 min)

### 3.1 Provoquer et résoudre un conflit

1. Crée la branche `variante-6-oeufs`. Dans `crepes.md`, remplace `3 oeufs` par `6 oeufs`. Commite.
2. Reviens sur `main`. Remplace **la même ligne** par `4 oeufs`. Commite.
3. Fusionne `variante-6-oeufs` dans `main`. Que dit Git ?
4. Tape `git status`, puis ouvre `crepes.md` : repère les trois marqueurs `<<<<<<<`, `=======`, `>>>>>>>`.
5. Résous le conflit : garde `5 oeufs` et supprime **tous** les marqueurs.
6. `git add crepes.md`, puis `git commit` (le message proposé par Git convient : enregistre et quitte nano avec `Ctrl+O`, `Entrée`, `Ctrl+X`).
7. Supprime la branche fusionnée.

### 3.2 Abandonner une fusion

1. Crée la branche `autre-titre` : change la première ligne de `README.md` en `# Mes recettes préférées`. Commite.
2. Sur `main`, change cette même ligne en `# Recettes de famille`. Commite.
3. Lance la fusion de `autre-titre` : conflit. Plutôt que de le résoudre, **abandonne** la fusion.
4. Vérifie avec `git status` que tout est revenu comme avant, puis supprime la branche `autre-titre` (elle n'est pas fusionnée : quelle option faut-il ?).

### Critères de réussite

- [ ] `crepes.md` contient `5 oeufs` et aucun marqueur de conflit.
- [ ] `git log --oneline --graph --all` montre la fusion de `variante-6-oeufs`.
- [ ] Après 3.2, `git status` est propre et `README.md` commence par `# Recettes de famille`.
- [ ] Tu sais expliquer ce que signifie la partie entre `<<<<<<< HEAD` et `=======`.

### Défi bonus

Provoque un conflit à trois fichiers en même temps (une ligne de chaque recette) et résous-les en un seul commit de fusion.

---

## Partie 4 : `stash` et `tag` (10 min)

### 4.1 `stash`

1. Sur `main`, ajoute `- une pincée de sel` à la fin de `crepes.md`, **sans commiter**.
2. Une urgence arrive : mets ce travail de côté avec `git stash`. Vérifie que `git status` est propre.
3. Crée la branche `correctif-salade`, ajoute `- huile d'olive` à `salade.md`, commite, puis fusionne dans `main` et supprime la branche.
4. Récupère ton travail de côté avec `git stash pop`, puis commite-le : `Ajoute le sel aux crêpes`.

### 4.2 `tag`

1. Marque l'état actuel avec un tag annoté `v1.0` et le message `Première version du livre de recettes`.
2. Liste les tags, puis affiche `git show v1.0 --stat`.

### Critères de réussite

- [ ] `git stash list` est vide à la fin.
- [ ] `crepes.md` contient `une pincée de sel`, commité sur `main`.
- [ ] `git tag` affiche `v1.0`.
- [ ] Tu sais dire en quoi un tag diffère d'une branche.

---

## Partie 5 : `rebase` (12 min)

1. Crée la branche `ajout-pates`, ajoute `pates.md`, commite. Ajoute `- parmesan` à `pates.md` et commite de nouveau (2 commits sur la branche).
2. Reviens sur `main`, complète la liste du `README.md` (`- pizza`, `- soupe`) et commite.
3. Affiche le graphe : tu dois voir un « Y ».
4. Retourne sur `ajout-pates` et rejoue-la sur `main` avec `git rebase main`.
5. Affiche de nouveau le graphe : en quoi est-il différent ?
6. Fusionne `ajout-pates` dans `main`. Y a-t-il un commit de fusion ? Supprime la branche.

### Critères de réussite

- [ ] Avant le rebase, le graphe montre deux lignes qui divergent.
- [ ] Après le rebase, l'historique de `ajout-pates` est linéaire, au-dessus de `main`.
- [ ] La fusion finale est un fast-forward.
- [ ] Tu sais expliquer pourquoi on ne rebase pas une branche partagée.

### Défi bonus

Crée la branche `ajout-risotto` avec 2 commits, puis fusionne-la dans `main` en **un seul commit** avec `git merge --squash`. Quelle option faut-il pour supprimer la branche ensuite ?

---

## Livrable de fin de séance

Ton dépôt `recettes/` sur `main`, propre, avec le tag `v1.0` et plusieurs recettes (`pizza.md`, `soupe.md`, `pates.md`...). **Ne le supprime pas** : on l'envoie sur GitHub en séance 3.

---

# CORRIGÉS

Le script complet, testé (il rejoue d'abord la séance 1), est dans [`corriges/corrige_seance2.sh`](corriges/corrige_seance2.sh). Pour le tester dans un bac à sable : `bash corriges/test_corrige.sh`. Les extraits ci-dessous sont les mêmes commandes, par exercice.

## Corrigé de la partie 1

```bash
git branch                       # une seule : main
git switch -c ajout-pizza
printf '# Pizza\n\n- pâte\n- tomates\n- mozzarella\n' > pizza.md
git add pizza.md
git commit -m "Ajoute la recette de la pizza"
git switch main                  # pizza.md disparaît du dossier
git branch -v
git log --oneline --graph --all
git switch ajout-pizza           # pizza.md réapparaît
git switch main
```

- Au retour sur `main`, `pizza.md` a disparu du dossier : il n'existe pas dans le dernier commit de `main`. Il est toujours dans l'historique de `ajout-pizza`.
- `main` pointe sur le dernier commit de la séance 1 ; `ajout-pizza` pointe un commit plus loin.

## Corrigé de la partie 2

```bash
# 2.1
git merge ajout-pizza
git log --oneline --graph --all
git branch -d ajout-pizza

# 2.2
git switch -c ajout-soupe
printf '# Soupe\n\n- carottes\n- poireaux\n' > soupe.md
git add soupe.md && git commit -m "Ajoute la recette de la soupe"
git switch main
printf '\n## Sommaire\n\n- crêpes\n- salade\n' >> README.md
git add README.md && git commit -m "Ajoute le sommaire au README"
git merge ajout-soupe --no-edit
git log --oneline --graph --all
git branch -d ajout-soupe
```

- 2.1 : pas de commit de fusion, car `main` n'avait pas avancé : Git a juste déplacé `main` (fast-forward).
- 2.2 : `main` avait avancé, Git crée un commit de fusion à deux parents, visible dans le graphe.

## Corrigé de la partie 3

```bash
# 3.1
git switch -c variante-6-oeufs
sed 's/3 oeufs/6 oeufs/' crepes.md > crepes.tmp && mv crepes.tmp crepes.md
git commit -am "Passe les crêpes à 6 oeufs"
git switch main
sed 's/3 oeufs/4 oeufs/' crepes.md > crepes.tmp && mv crepes.tmp crepes.md
git commit -am "Passe les crêpes à 4 oeufs"
git merge variante-6-oeufs        # CONFLIT dans crepes.md
git status
```

Dans `crepes.md`, avant résolution :

```text
<<<<<<< HEAD
- 4 oeufs
=======
- 6 oeufs
>>>>>>> variante-6-oeufs
```

Après résolution (les 5 lignes du bloc remplacées par une seule) :

```text
- 5 oeufs
```

```bash
git add crepes.md
git commit -m "Fusionne variante-6-oeufs : 5 oeufs pour les crêpes"
git branch -d variante-6-oeufs

# 3.2
git switch -c autre-titre
# changer la 1re ligne de README.md en « # Mes recettes préférées »
git commit -am "Change le titre en Mes recettes préférées"
git switch main
# changer la 1re ligne de README.md en « # Recettes de famille »
git commit -am "Change le titre en Recettes de famille"
git merge autre-titre             # CONFLIT
git merge --abort
git status                        # propre
git branch -D autre-titre         # -D : la branche n'a pas été fusionnée
```

- La partie entre `<<<<<<< HEAD` et `=======` est la version de **la branche sur laquelle tu es** (`main`).
- `sed -i` n'est pas portable (macOS) : le corrigé passe par un fichier temporaire. Dans l'exercice, tu peux simplement éditer avec nano.

### Corrigé du défi bonus

Même démarche trois fois : trois branches qui modifient chacune une ligne différente de trois fichiers, fusionnées dans une même branche cible, puis `git add` des trois fichiers et un seul `git commit` pour finir la fusion.

## Corrigé de la partie 4

```bash
echo "- une pincée de sel" >> crepes.md
git stash
git status                        # propre
git stash list
git switch -c correctif-salade
echo "- huile d'olive" >> salade.md
git commit -am "Ajoute l'huile d'olive à la salade"
git switch main
git merge correctif-salade --no-edit
git branch -d correctif-salade
git stash pop
git commit -am "Ajoute le sel aux crêpes"

git tag -a v1.0 -m "Première version du livre de recettes"
git tag
git show v1.0 --stat
```

- Un tag est un nom **fixe** attaché à un commit, alors qu'une branche **avance** avec chaque nouveau commit.

## Corrigé de la partie 5

```bash
git switch -c ajout-pates
printf '# Pâtes\n\n- pâtes\n- sauce tomate\n' > pates.md
git add pates.md && git commit -m "Ajoute la recette des pâtes"
echo "- parmesan" >> pates.md
git commit -am "Ajoute le parmesan aux pâtes"

git switch main
printf '\n- pizza\n- soupe\n' >> README.md
git commit -am "Complète le sommaire"
git log --oneline --graph --all   # un « Y »

git switch ajout-pates
git rebase main
git log --oneline --graph --all   # ligne droite

git switch main
git merge ajout-pates             # fast-forward
git branch -d ajout-pates
```

- Après le rebase, les commits de `ajout-pates` ont de **nouveaux hash** : ils ont été rejoués au-dessus de `main`.
- On ne rebase pas une branche partagée : les collègues ont les anciens commits, et leur historique ne correspond plus au tien.

### Corrigé du défi bonus

```bash
git switch -c ajout-risotto
printf '# Risotto\n\n- riz\n' > risotto.md
git add risotto.md && git commit -m "Ajoute la recette du risotto"
echo "- bouillon" >> risotto.md
git commit -am "Ajoute le bouillon au risotto"
git switch main
git merge --squash ajout-risotto
git commit -m "Ajoute la recette du risotto"
git branch -D ajout-risotto       # -D obligatoire : Git ne considère pas la branche comme fusionnée
```
