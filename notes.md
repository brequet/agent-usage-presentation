Je veux créer une présentation sur l'utilisation des agents IA pour les developeurs.

Je met des idées en vrac ici, ce n'est pas exactement organisé ! Ton role va etre de m'aider à formatter tout ça.

Je me suis rendu compte qu'on ne s'en sert pas tous de la meme manière, et j'aimerai montrer quelques choses que j'ai appris, mais en même temps, j'aimerai que ce soit interactif. Je vais surtout donner des exemples, et j'aimerai que les gens puissent partager leurs expériences.

Voice quelques idées que j'aimerai mettre en avant:

Petit rappel rapide de ce qu'est un agent

Recherche d'information / Reverse engineering / Explication de features

Il faut que les agents puissent nous aider a comprendre des choses qu'on ne maitrise pas forcement.

On peut utiliser les agents pour faire de la recherche d'information, par exemple pour comprendre comment une feature fonctionne, ou comment un outil est configuré, via le code d'autres repositories.

J'ai aussi trouvé des manières de booster les capacités de recherches des agents: au lieu de les laisser se débrouiller, on peut leur donner des instructions supplémentaires pour leur donner des outils de recherche plus performants!
Par exemple, au lieu de laisser l'extension copilot d'intellij faire des recherches basiques, on peut lui dire qu'il y a ripgrep d'installé, et lui donner des instructions pour l'utiliser. Idem pour d'autres outils.

-> Ca serait bien de trouver des chiffres qui explique pourquoi c'est mieux (reduction du nombre de tokens a process etc)

Il y a des exemples récent et concret que j'ai pu expérimenter:

- We started a new project and I needed to understand how the auth was setup and other features from a recent project (same compagny so we must integrate with the same way of doing things) ; what db they used etc 
- Retro engineered the CLI arguments for a compagny tool we used but i failed to find the doc for

Il faut savoir utiliser les agents intelligemment.

Autre exemple: pour cette présentation, le fond est l'essentiel, la forme moins.
J'aurais pu demander a un agent de me générer une présentation toutes faites sous formes de slides mais ca n'aurait pas été très opti: il aurait surement généré un html avec du style et du js pour recréer lui même un système de slides. Il aurait perdu donc du temps, du contexte et de la précision/focus sur cette tache, et le fond aurait été noyé donc moins pertinent.
A la place, j'ai choisi une librarie (Marp) qui est faite pour généré des slides a partir de markdown: dans ce cas, l'agent se concentre essentiellement sur le fond car il n'a juste qu'a écrire du markdown, et moi je m'occupe de la forme en utilisant Marp pour le rendu final.
=> En choissisant les bons outils, on boost l'efficacité des agents. On leur permet de se concentrer sur ce qu'ils savent faire en déléguant une partie du travail a des outils spécialisés.


Je ne suis pas sur du titre de la présentation, ca c'est bof : "Cas d’usage originaux de l'IA pour les devs (agent copilot, tricks pour améliorer la pertinence, optimisation du contexte, rétroingénierie et autres etc..)" -> TODO chercher mieux, plus concis et plus attrayant


D'autres point que j'aimerais aborder:
- Copilot extension (dans intellij là mais ca marche avec les autres IDE):
  - on peut mettre des instructions global par dev (j'utilise pwsh 7 ou bash etc, rg..)
  - Aussi instruction par projet (dans un fichier .copilot-instructions.md a la racine du projet)
  - -> ptit tuto rapide pour montrer ca ?

SI on réfléchi a une structure de présentation:
- Introduction rapide: 
  - LLM et Agent (-> boucle for sur un agent, permet des taches plus complexes, certaine autonomie etc)
  - Utilité d'un agent: aide a la recherche d'info, reverse engineering, explication de features, generation de tests etc (TDD)
- Github COpilot (ou autre agent) dans IDE (intellij pour mes exemples)
  - Instructions globales / par projet
  - Booster les capacités de recherche (exemple ripgrep) -> TODO: trouver des chiffres/des exemples qui prouve l'efficacité de la méthode => reduction du nombre de tokens, rapidité etc
    - Exemples concrets (retro engineering d'une CLI, comprendre une feature d'un projet velu etc)
    
    
  TIPS POUR LES PROMPTS: find the right balance between specificity and generality => You don't want to spend too much time crafting the prompt (else you are almost doing the job the agent will od), but still its important to give enough context/instructions to get good results.
  
  
UPDATE:
- C'est un milieu qui avance rapidement - juste cette semaine est sortie mgrep (https://github.com/mixedbread-ai/mgrep) qui est un grep optimisé pour la recherche en language naturelle, très opti pour les LLMs. Je n'ai pas eu l'occaz d'essayer (c'est un outil payant mais avec free tier, ca utilise un API donc ce n'est plus très local.. ca c'est bof)
  -> ajouté ca en note de fin?
  
  
  Qu'est-ce qu'un Agent IA ?
  
  - **Défini simplement**: LLM + boucle + outils
  - **La boucle Think-Act-Observe:**
      - **Think** (Thought): LLM décide prochaine action
      - **Act** (Action): Agent appelle les outils avec params
      - **Observe** (Observation): reçoit résultat, continue loop jusqu'à objectif atteint
  - **Points clés à montrer:**
      - Autonomie progressive (pas juste une réponse, mais itération)
      - Peut gérer tâches complexes multi-steps
      - La qualité des instructions système détermine le comportement


 Utilité pour les devs

- Recherche d'infos
- Reverse engineering (CLI args, configs, intégrations)
- Explication de features complexes
- Génération de tests (TDD)
- Onboarding tech stack

***

AGENTS DANS L'IDE  - Copilot (IntelliJ mais générique pour tous les IDE et tout les agents aussi)

- **Instructions globales** (.copilotconfig global)
    - Env: `pwsh 7`, `bash`, `rg`, tooling local
    - Style de code préféré
    - Patterns utilisés dans la team
- **Instructions par projet** (.copilot-instructions.md)
    - Framework spécifique (Spring Boot, patterns ADR)
    - Conventions du projet
    - Endpoints/APIs dispo

**Bénéfice:** Agent adapté au contexte local = moins de hallucinations, plus de pertinence

 BOOSTER LES CAPACITÉS DE L'AGENT (exemple avec la recherche)


 Le problème: Naive Search = Inefficace

- Agent sans instructions de recherche: fait avec ce les moyens du bord
- Utilise surement le ctrl+F basique de l'IDE = résultats limités, query non optimisé.
-> c'est pas mauvais pour autant mais j'ai le sentiment qu'on peut faire mieux

La solution: Donner les bons outils ET instructions

Perso je trouve que ca marche mieux avec rg (ripgrep):
TODO use dire a l'agent qu'il a accès a ripgrep plutot pour faire des recherches:
- lui donner une doc simple (il doit connaitre, c'est un outil connu)
- -> l'outil est dédié a ca, il est plus efficace que la recherche basique
- -> query puissante que l'IA saura utiliser
- -> Moins de tokens consommés, recherche plus rapide, résultats plus pertinents


EXEMPLES CONCRETS VÉCUS
 Onboarding technique (nouveau projet)

- **Situation:** Nouveau projet, besoin d'implem auth + features
- **Même compagnie:** patterns/conventions identiques
- **Questions:** DB utilisée? Auth scheme? Integration points?
- **Avec agent + instructions:**
    - Agent cherche avec `rg "auth" --type java` dans codebase
    - Trouve Spring Security config
    - Cherche DB setup → `rg "DataSource|JdbcTemplate"`
    - Synthétise patterns pour nouveau projet
    
TODO: moyen de faire un exemple de rapport/resultat avec et sans rg/prompts pour voir la différence de pertinence ?

Reverse Engineering CLI Tool

- **Situation:** Outil interne, doc introuvable


 CHOIX DES OUTILS = MULTIPLICATEUR D'EFFICACITÉ


Anti-pattern: "Utiliser un agent pour tout"

- LLM génère slides HTML complet (layout + CSS + JS + slides logic)
- Perte: contexte divisé (design + contenu)
- Résultat: fond noyé dans du HTML boilerplate, pas optimisé

Pattern: "Agent + bon outil spécialisé"

- **Marp:** markdown-based slides
- **Agent:** écrit le contenu/structure (ce qu'il sait faire)
- **Marp:** gère rendering/style (ce qu'il sait faire)
- **Résultat:** Séparation concerns, focus sur fond, agents concentrés

**À mettre en avant:**

- Agent ne perd pas de tokens sur creation from scratch d'un systeme de slides (logique, affichage etc)
- Agent peut réitérer contenu rapidement (markdown = facile à edit)
- Output final de qualité (Marp know slides)


 Appliquer à d'autres contextes

- **CLI tools:** Agent + ripgrep + cat (pas agent qui invente grep)
- **Tests:** Agent + framework existant (pas agent qui code JUnit from scratch)
- **Docs:** Agent + markdown (pas agent qui génère HTML)

**Pattern général:** Agent = Brain, Specialized Tool = Hands



INTERACTIVE SEGMENT**



Idée 1 de structure, a voir si c'est ok:

```
I. INTRO: LLM vs Agent
   - Simple loop: think → act → observe
   - Pourquoi c'est game-changer

II. AGENTS DANS IDE
    - Copilot/Cursor: global + project instructions
    - Quick setup demo

III. BOOSTER RECHERCHE (Technical Heart)
     - Problem: naive search = waste
     - Solution: ripgrep + instructions
     - Chiffres: performance + tokens
     - Exemples concrets (your 2 use cases)

IV. ARCHITECTURE: Agent + Outils
    - Why separation of concerns matters
    - Marp example walkthrough
    - Anti-patterns to avoid

V. INTERACTIVE: Your experiences?
   - Q&A
   - Share workflows

VI. OUTRO: Key takeaway
    - "Agents + right tools = exponential productivity"
```
