Founder skill pour Hermes Agent

=====================

Version patchée de [Jakeschincariol/founder-skill](https://github.com/Jakeschincariol/founder-skill), adaptée pour [Hermes Agent](https://github.com/NousResearch/hermes-agent) (self-hosted). Licence MIT d'origine conservée.

## Ce qui a changé

- Frontmatter : `argument-hint` supprimé, `version`/`license` ajoutés.
- `${CLAUDE_SKILL_DIR}` remplacé par le chemin d'installation `~/.hermes/skills/<skill>/`.
- `$ARGUMENTS` remplacé par la formulation Hermes (skills activées par contexte).
- Agent tool / Task de Claude Code remplacé par `delegate_task` de Hermes dans `founder-board` et `founder-consumer` (réécrits) ; les 9 autres skills sont patchés avec une note Hermes.
- Notes de permissions Claude Code supprimées.

## Installation

```bash
git clone https://github.com/TopheC/founder-skill-hermes.git
cp -r founder-skill-hermes/skills/founder-* ~/.hermes/skills/
```

Python 3.8+ requis pour les scripts (`panel.py`, `unit_economics.py`, `van_westendorp.py`, `compile.py`), rien à installer via pip.

## Les 11 skills

- `founder-board` : conseil d'administration (lenses Offers / Monopoly / Product)
- `founder-competitors` : cartographie des concurrents
- `founder-consumer` : panel de 100 acheteurs simulés
- `founder-pricing` : stratégie de prix (Van Westendorp)
- `founder-offer` : architecture de l'offre
- `founder-cfo` : économie unitaire, trésorerie
- `founder-marketing` : campagne de lancement
- `founder-brand` : nom et marque
- `founder-ops` : opérations, fournisseurs, risques
- `founder-launch` : plan de lancement
- `founder-plan` : compilation du pack

## Ordre du pack

board -> competitors -> consumer -> pricing -> offer -> cfo -> marketing -> brand -> ops -> launch -> plan

Chaque skill peut aussi tourner seule ; chacune lit ce qui existe déjà dans `founder/`.

## Notes Hermes

- `delegate_task` lance jusqu'à 10 sous-agents concurrents par défaut (`delegation.max_children`).
- En session one-shot (`hermes chat -q`), le plafond est `delegation.oneshot_max_children` (2 par défaut) : lancer board et panel depuis une session interactive, ou relever le plafond.
- Avant un run à 100 acheteurs : ~10 appels modèle simultanés par vague, vérifier la capacité du backend self-hosted, sinon `--quick` (20 acheteurs).

## Crédits

Projet original : [Jakeschincariol/founder-skill](https://github.com/Jakeschincariol/founder-skill) (MIT). Le fichier LICENSE est celui de l'original, conservé tel quel. Les dossiers `.claude-plugin` et `tests` de l'original (spécifiques à Claude Code) ne sont pas repris.
