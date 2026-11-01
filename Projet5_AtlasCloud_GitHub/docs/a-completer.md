# Projet 5 AtlasCloud — liste de finalisation

État de référence : 1er novembre 2026. La liste distingue ce qui est **validé**, ce qui est **à vérifier dans le dépôt**, et ce qui reste **à réaliser** au regard du sujet officiel `M1-05_Securite_perimetrique.pdf`.

## A. Urgent — avant la soutenance du lundi 2 novembre

- [ ] **Vérifier la dernière version du PowerPoint (11 slides)** et prévoir environ 10 min pour l’oral. La slide 11 est une preuve finale après la conclusion ; sinon, la déplacer juste après la 8.
- [ ] **S’assurer que la capture Threat Hunting `100501` est bien lisible sur la slide 11** ; vérifier dans la présentation finale, pas seulement dans le dossier `evidence/`.
- [ ] **Garder les preuves de secours** : alerte et DROP Suricata ; HTTP 200 et 403 ; CRS 942100 et 949110 ; tests NetBird ; agent Wazuh ; règle 100501.
- [ ] **Répéter le scénario WAF → Wazuh** sans démarrer pfSense en parallèle : vérifier au préalable la mémoire Windows et l’espace disque.
- [ ] **Prendre une capture des logs ModSecurity** contenant `ruleId: 942100`, `ruleId: 949110`, score 8 et HTTP 403 ; preuve textuelle existante dans le chat, mais capture séparée à conserver.
- [ ] **Relire le discours `docs/soutenance.md`** et répartir la parole si soutenance en équipe.
- [ ] **Vérifier que tous les liens et images du README s’affichent sur GitHub** après publication, et retirer tout secret ou identifiant personnel.

## B. Configuration et reproductibilité du dépôt — important

- [ ] **Remplacer `config/docker/compose.yaml` reconstitué** par l’original de la VM : `cd ~/projet5-waf && cat compose.yaml` ou copier le fichier via SCP. La version fournie est une reconstruction, pas une exportation certifiée identique.
- [ ] **Exporter les règles pfSense** (NAT, interfaces, LAN, politiques) sous une forme documentée ; ne jamais publier des secrets issus d’une sauvegarde brute sans nettoyage.
- [ ] **Exporter et documenter les paramètres Suricata** (ET Open, mode IDS/IPS, interface LAN, règle locale SID 1000001, captures associées).
- [ ] **Vérifier les politiques NetBird** réellement utilisées (`P5-Employees`, `P5-Servers`, `P5-Allow-WAF` TCP/8080) et fournir une procédure de réapplication expurgée de clés.
- [ ] **Copier les extraits Wazuh exacts**, après vérification : `local_rules.xml` (100501), bloc `<localfile>` de l’agent, `05-soc-net.network`, délai systemd du Manager.
- [ ] **Fiabiliser la source de logs Docker** : l’agent pointe actuellement vers un ID de conteneur ; prévoir une source stable ou une procédure de mise à jour après recréation.
- [ ] **Figer les versions des images Docker** (éviter `latest`) et conserver un exemple de logs anonymisé, des instructions de démarrage et d’arrêt et les exigences de ressources.
- [ ] **Tester la procédure README sur un environnement propre**, ou indiquer explicitement qu’elle n’a pas été retestée de bout en bout.
- [ ] **Vérifier la licence et les fichiers Git**, puis créer le dépôt distant. Le ZIP livré n’est pas automatiquement publié sur GitHub.

## C. Exigences du sujet officiel — documents à compléter / approfondir

- [ ] **Audit de l’existant AtlasCloud** : SonicWall ancien, VPN SSL legacy, absence de WAF, segmentation cloud insuffisante, télétravailleurs sur VPN direct ; conséquences, priorités et hypothèses.
- [ ] **Analyse de risques étayée** : matrice probabilité/impact, risque résiduel, responsables, mesures compensatoires et priorisation.
- [ ] **Architecture cible pour une entreprise de 250 salariés**, distincte du laboratoire : HA, NGFW, IDS/IPS, WAF, ZTNA, IAM/OIDC, réseau cloud, sauvegardes, journalisation ; justifier pfSense/NetBird face aux alternatives du sujet (OPNsense/Headscale notamment).
- [ ] **Diagrammes C4** contexte, conteneurs et composants finalisés avec flux, zones de confiance, IP/ports, responsabilités et légendes ; les `.mmd` existants sont une base éditable.
- [ ] **Audit trail NIS2 sur trois briques** : WAF → Wazuh **validé** ; Suricata → Wazuh **non démontré** ; ZTNA → Wazuh **non démontré**. Prévoir une preuve distincte pour chaque flux, ou présenter clairement la limite.
- [ ] **Plan de migration en six phases sur six mois** : dépendances, pilote, bascule progressive sans interruption, rollback, critères GO/NO-GO et responsables.
- [ ] **Budget CAPEX + OPEX sur 18 mois < 250 k€** : hypothèses, quantités, coûts licences/support/formation/ressources, provisions et alternatives open source ; chiffrage à valider.
- [ ] **Dossier d’architecture de 50 à 80 pages** demandé par le sujet : le `rapport.md` et les documents actuels sont une base, pas une preuve que le volume et la profondeur requis sont atteints.
- [ ] **Présentation COMEX de 20 slides / 30 min** demandée par le sujet : le PowerPoint actuel de 11 slides est une version courte **technique**, pas le livrable COMEX complet.
- [ ] **Note de cadrage de 2 pages**, à vérifier et compléter avec planning, budget et critères de qualité.
- [ ] **Vidéo de démonstration** prévue par le sujet ; enregistrer ou documenter honnêtement son absence si non réalisée.
- [ ] **Infrastructure as Code** : prévoir Ansible et OpenTofu/Terraform pour une reproductibilité avancée si le temps et le périmètre le permettent ; actuellement non validés.

## D. Corrections documentaires recommandées

- [ ] Harmoniser les dates, heures et fuseaux des captures entre Ubuntu, Wazuh et Windows.
- [ ] Séparer partout **PoC démontré** et **architecture cible recommandée**.
- [ ] Expliquer que le test Suricata IPS porte sur une **règle ICMP locale**, pas sur une validation exhaustive d’ET Open.
- [ ] Expliquer que la règle 100501 recherche la chaîne `942100` dans le champ `log` JSON Docker : la rendre plus discriminante avant toute mise en production.
- [ ] Ajouter un tableau de correspondance « objectif → commande/test → capture → résultat → limites ».
- [ ] Vérifier toutes les commandes copiées, versions, chemins, noms de groupes et références ; retirer mots de passe, IP publiques et jetons.

## Priorisation réaliste

**D’ici demain** : vérifier captures, oral, démonstration et conformité minimale du dépôt. **Après la soutenance ou avec l’équipe si le temps le permet** : audit trail trois briques, C4 complets, chiffrage validé, dossier 50–80 pages et présentation COMEX 20 slides.
