# Founder skill pour Hermes Agent

> Onze skills qui transforment [Hermes Agent](https://github.com/NousResearch/hermes-agent) en équipe de conseil, pour tester un business **avant** de le lancer : board d'administration, éclaireur concurrentiel, panel d'acheteurs simulés, directeur financier, marketing complet.

Version adaptée pour Hermes (self-hosted) de [Jakeschincariol/founder-skill](https://github.com/Jakeschincariol/founder-skill) — conçu à l'origine pour Claude Code. Licence MIT d'origine conservée.

📖 **Guide complet pour débutants** : consultez le [wiki du dépôt](https://github.com/Tophe-Factory/founder-skill-hermes/wiki) (Description, Installation, Utilisation, FAQ).

---

## Installation

Prérequis : [Hermes Agent](https://hermes-agent.nousresearch.com/docs) fonctionnel et Python 3.8+ (aucune dépendance à installer via pip).

```bash
git clone https://github.com/Tophe-Factory/founder-skill-hermes.git
cp -r founder-skill-hermes/skills/founder-* ~/.hermes/skills/
```

Les skills s'activent ensuite par contexte : il suffit de décrire son idée et de demander de la tester.

## Les onze skills

| Skill | Rôle | Sortie principale |
|---|---|---|
| `founder-board` | Conseil d'administration (lenses Offers / Monopoly / Product) | Vote FUND / FUND IF / PASS dans `founder/board.md` |
| `founder-competitors` | Éclaireur concurrentiel | Carte des concurrents, prix, plaintes clients, faits sourcés |
| `founder-consumer` | Panel de consommateurs | 100 acheteurs simulés (20 en `--quick`) : taux d'achat, objections, prix |
| `founder-pricing` | Stratégie de prix | Fourchette acceptable (Van Westendorp) confrontée à la marge |
| `founder-offer` | Architecture de l'offre | Offre stack : bonuses, garantie, urgence, nom |
| `founder-cfo` | Directeur financier | Marge réelle, seuil de rentabilité, année 1, trésorerie |
| `founder-marketing` | Direction marketing | Positionnement, canaux, campagne 30 jours, coût d'acquisition max |
| `founder-brand` | Identité de marque | Noms candidats, vérifications à mener, ton, promesse |
| `founder-ops` | Opérations | Fournisseurs, routines, licences, registre des risques |
| `founder-launch` | Jour J | Checklist de lancement |
| `founder-plan` | Compilation | Plan complet assemblé depuis tout le dossier `founder/` |

**Ordre recommandé** :

```text
board → competitors → consumer → pricing → offer → cfo → marketing → brand → ops → launch → plan
```

Chaque skill écrit dans `founder/` à la racine du projet et suggère la suivante ; chacune peut aussi tourner seule en lisant ce qui existe déjà.

## Ce qui a changé par rapport à l'original

- `delegate_task` de Hermes remplace le mode sub-agent de Claude Code (`founder-board` et `founder-consumer` réécrits en conséquence)
- Chemins de fichiers systématiquement en `~/.hermes/skills/`
- Retrait des spécificités Claude Code : `argument-hint`, `$ARGUMENTS`
- Scripts Python originaux conservés tels quels : `panel.py`, `unit_economics.py`, `van_westendorp.py`, `compile.py`

## Notes Hermes (self-hosted)

- `delegate_task` lance jusqu'à 10 sous-agents concurrents par défaut (`delegation.max_children`)
- En session one-shot (`hermes chat -q`), le plafond est `delegation.oneshot_max_children` (2 par défaut) : lancer board et panel depuis une session interactive, ou relever le plafond dans `config.yaml`
- Un panel à 100 acheteurs représente 10 vagues d'environ 10 appels modèle simultanés : vérifier la capacité du backend, sinon démarrer avec `--quick` (20 acheteurs)
- Les sous-agents demandent l'approbation de leurs écritures ; répondre dans le chat d'origine

## Limites honnêtes

Les acheteurs simulés sont bons pour révéler des objections et des segments, mauvais pour prédire une conversion réelle : le taux d'achat se lit comme une **borne supérieure**. Confirmer les grandes décisions avec de vrais gens (liste d'attente, pré-vente, landing page avec prix). Ne jamais présenter une citation du panel comme un témoignage client.

## Crédits et licence

Projet original : [Jakeschincariol/founder-skill](https://github.com/Jakeschincariol/founder-skill), licence MIT (fichier `LICENSE`, conservé tel quel). Les dossiers `.claude-plugin` et `tests` de l'original, spécifiques à Claude Code, ne sont pas repris.
