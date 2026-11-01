# Projet 5 — AtlasCloud
## Discours oral court — Soutenance (11 diapositives)

Ce texte accompagne la version **Projet5_AtlasCloud_Soutenance_MAJ.pptx**. Les phrases sont volontairement courtes, simples et naturelles.

### Diapositive 1 — Introduction

« Bonjour à tous. Aujourd’hui, nous allons vous présenter notre projet de refonte de la sécurité périmétrique d’AtlasCloud. L’objectif est de mieux protéger le réseau, les applications Web et les accès distants. »

### Diapositive 2 — Contexte et objectifs

« Le besoin d’AtlasCloud est de sécuriser les flux réseau, de protéger les applications Web et de contrôler les accès distants, tout en centralisant les événements de sécurité. »

### Diapositive 3 — Architecture technique

« Notre architecture repose sur pfSense et Suricata pour le réseau, un WAF Nginx et ModSecurity pour le Web, NetBird pour les accès Zero Trust et Wazuh pour la supervision. »

### Diapositive 4 — pfSense et Suricata

« Nous avons configuré pfSense pour contrôler les flux réseau. Avec Suricata, nous avons déclenché des alertes ICMP, puis activé le mode IPS pour vérifier le blocage du trafic. »

### Diapositive 5 — Protection WAF

« Devant Juice Shop, une requête normale passe en HTTP 200. En revanche, une injection SQL est détectée par la règle CRS 942100 et bloquée avec une réponse HTTP 403. »

### Diapositive 6 — Contrôle d’accès NetBird

« Avec NetBird, nous avons autorisé l’accès au port 8080 à un groupe défini. En revanche, l’accès SSH via NetBird est refusé. Nous avons également vérifié que la désactivation de la politique retire l’accès. »

### Diapositive 7 — Déploiement Wazuh

« Nous avons déployé Wazuh pour centraliser les alertes. Sur Ubuntu, nous avons installé un agent qui communique avec le serveur Wazuh sur notre réseau interne SOC-NET. »

### Diapositive 8 — Chaîne de détection SQLi

« Lorsqu’une injection SQL est bloquée par ModSecurity, l’événement est enregistré dans les journaux Docker. L’agent Ubuntu transmet ces journaux à Wazuh, où notre règle 100501 génère une alerte de niveau 10. »

### Diapositive 9 — Résultats et limites

« Nos tests valident le filtrage réseau, la détection et le blocage des attaques, ainsi que le contrôle des accès. Cependant, il s’agit d’un PoC : toutes les sources ne sont pas encore centralisées dans Wazuh. »

### Diapositive 10 — Conclusion

« Pour conclure, nous avons mis en place plusieurs couches de sécurité complémentaires. Ce projet nous a permis de valider des scénarios concrets de protection et de supervision, avec des améliorations possibles pour un déploiement en production. »

### Diapositive 11 — Preuve finale : Threat Hunting

« Sur cette dernière capture, nous retrouvons deux alertes SQLi associées à notre agent p5-client-01. Elles correspondent à la règle 100501, de niveau 10. Cela confirme que les événements du WAF remontent bien dans Wazuh. »

---

## Conseils rapides pour l’oral

- **Ordre conseillé :** présenter la diapositive 11 juste après la 8, puis terminer par le bilan (9) et la conclusion (10). Si tu conserves le PowerPoint tel quel, suis simplement l’ordre 1 à 11 ci-dessus.
- **Mots-clés à retenir :** pfSense, Suricata, HTTP 200/403, NetBird port 8080, Wazuh règle 100501.
- **Plan B de démonstration :** montrer les captures enregistrées plutôt que redémarrer toutes les VM sur le PC de 16 Gio.
