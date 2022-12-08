# Github guide

Une organisation <a href="https://github.com/UTBM-FISA-TutoredProject" target="_blank">Github</a> a été créée pour le
projet tutoré, si vous ne faites pas partie de l'organisation Github et que vous souhaitez y participer, contactez le
référent Github (<a href="mailto:kilian.goetz@yahoo.fr">Contact</a>).

## Équipes et projets

À l'image de la précédente <a href="https://utbm-fisa-tutoredproject.github.io/Documentation/#/organisation">section</a>
le github possède **4 équipes** et une **5<sup>ème</sup>** qui est une équipe globale. Chacune des équipes possède son
propre projet, qui détiennent des "vues" qui sont soit des kanbans, soit des tableaux.

***Qu'est-ce qu'un Kanban ?*** Un kanban est un tableau graphique composé d'un nombre n de colonnes représentant chacun
un status. Vous pouvez créer dans ces colonnes des cartes contenant chacune une idée ou un travail à effectuer. Ces
différentes cartes peuvent agrémenter de champs spécifiques tels que : une personne assignée, une priorité, une
deadline, un sprint, etc.

Les Kanbans sont définis de la même manière dans toutes les équipes :

- :x: **Bug :** Colonne contenant les bugs identifiés.
- :bulb: **ideas :** Colonne contenant les idées potentielles de nouvelles fonctionnalités.
- :clipboard: **Todo :** Colonne contenant les fonctionnalités à implémenter.
- :broken_heart: **Review rejected :** Colonne contenant les revues de code rejeté par le relecteur.
- :clock1: **In Progress :** Colonne contenant les fonctionnalités en cours d'implémentation.
- :eyes: **To review :** Colonne contenant les fonctionnalités terminées nécessitant une revue du code par un autre
  membre de l'équipe avant un merge.
- :eyes: :clock1: **Reviewing :** Colonne contenant les fonctionnalités terminées en cours d'examination par un
  relecteur. :exclamation: <span style="color:red">Si le relecteur n'accepte pas le code la carte sera renvoyer dans la
  colonne ":broken_heart: Review rejected"</span> :exclamation:
- :heavy_check_mark: **Done pending on merge :** Colonne contenant les fonctionnalités terminées et vérifier prête à
  être merge.
- :file_folder: **Archived :** Colonne contenant les fonctionnalités terminées, vérifier et merge au projet. :
  exclamation: <span style="color:red">Attention, une fois entièrement terminées il ne faut pas supprimer les cartes,
  mais les laissez dans cette colonne. Cela permet d'avoir un aperçu clair de ce qui a été fait</span> :exclamation:

***Vue en tableau ?*** Les tableaux de projets sont de simple tableaux groupés et trier selon des filtres. Par
exemple un tableau **"By Priority"** dans lequel le travail est rangé par priorité. Les vues en tableau sont bien pour
visualiser de façon efficace un grand nombre d'idées et d'identifier plus facilement celles-ci.

<video controls style="width:80%">
  <source src="./_media/GithubProjects.webm" type="video/mp4">
</video>

## Workflow

![Workflow](./_media/GithubWorkflow.png)