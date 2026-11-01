# Soutenance — Projet 5 AtlasCloud

**Version alignée sur `Projet5_AtlasCloud_Soutenance_MAJ.pptx` (11 diapositives)**  
Soutenance prévue : **lundi 2 novembre 2026**. Durée indicative de ce script : **9 à 11 minutes**, hors démonstration. Les phrases sont volontairement simples et prononçables. Les diapositives 8 et 11 parlent du même test : la première expose la chaîne, la dernière apporte la capture finale.

## Diapo 1 — Sécurité périmétrique & supervision SOC
**À dire :** « Bonjour à tous. Nous allons présenter notre projet de refonte de la sécurité périmétrique d’AtlasCloud. Notre objectif est de protéger les communications réseau, l’application Web et les accès distants, mais aussi de centraliser les alertes de sécurité. Pour cela, nous avons construit un laboratoire avec pfSense, Suricata, ModSecurity, NetBird et Wazuh. »

**Transition :** « Je commence par le contexte et les objectifs. »

## Diapo 2 — Contexte et objectifs
**À dire :** « AtlasCloud est une entreprise en croissance. Son infrastructure doit pouvoir évoluer sans conserver des accès trop larges ou des protections dispersées. Nous avons donc retenu une défense en profondeur : filtrer les flux, détecter les tentatives d’attaque, protéger les applications, limiter les accès et remonter les événements dans un outil central. Nous avons vérifié ces objectifs avec des scénarios de test précis. »

**Transition :** « Voici comment nous avons organisé notre laboratoire. »

## Diapo 3 — Architecture technique du laboratoire
**À dire :** « Ici, on voit les différentes briques. pfSense assure le pare-feu, et Suricata surveille le trafic réseau. Sur Ubuntu, nous hébergeons Juice Shop derrière Nginx et ModSecurity. NetBird contrôle l’accès distant au service Web. Enfin, un agent installé sur Ubuntu envoie les événements du WAF au serveur Wazuh. Pour nos tests SOC, Ubuntu et Wazuh communiquent directement sur le réseau interne SOC-NET. »

**Point de transparence :** « À cause des 16 Go de mémoire de notre ordinateur, pfSense était arrêté durant la démonstration Wazuh. Les scénarios ont été validés séparément. »

**Transition :** « Commençons par la sécurité réseau. »

## Diapo 4 — pfSense + Suricata : détection et blocage
**À dire :** « Nous avons configuré pfSense et activé Suricata. Dans un premier temps, nous avons créé une règle ICMP de test. En mode IDS, Suricata génère une alerte, mais laisse passer le trafic. Nous avons ensuite testé le mode IPS : la règle bloque les paquets et le ping échoue. Les captures montrent la différence entre détection et prévention. »

**Transition :** « Ensuite, nous avons protégé notre application Web. »

## Diapo 5 — Protection applicative : WAF
**À dire :** « Notre application de test est OWASP Juice Shop, protégée par Nginx et ModSecurity avec les règles OWASP CRS. Une requête normale renvoie HTTP 200, donc l’application reste accessible. Nous avons ensuite envoyé une injection SQL de test dans le paramètre id. ModSecurity a reconnu la tentative avec la règle 942100. Comme le score d’anomalie dépassait le seuil prévu, la requête a été bloquée en HTTP 403. »

**Transition :** « Mais protéger une application ne suffit pas : il faut aussi contrôler qui peut y accéder. »

## Diapo 6 — Zero Trust : contrôle d’accès NetBird
**À dire :** « Nous avons créé deux groupes NetBird : les employés et les serveurs. Une politique autorise seulement l’accès TCP au port 8080 du WAF. Nous avons confirmé que l’application était accessible, alors que le port SSH 22 n’était pas autorisé par cette politique. Nous avons également désactivé temporairement la règle : l’accès au port 8080 a été coupé. Cela démontre que les autorisations sont révocables. »

**Précision si question :** « Le SSH local de maintenance via la redirection NAT de VirtualBox est distinct de l’accès NetBird. »

**Transition :** « Passons maintenant à la centralisation des événements. »

## Diapo 7 — Centralisation Wazuh : agent actif
**À dire :** « Nous avons déployé Wazuh avec son Manager, son Indexer et son Dashboard. Un agent Wazuh version 4.14.9 a été installé sur notre serveur Ubuntu, à l’adresse 192.168.100.10. La capture montre l’agent 001 au statut actif. Nous l’avons ensuite configuré pour lire le fichier de journaux JSON du conteneur WAF. »

**Transition :** « Voici ce que cela donne lorsqu’une attaque de test est envoyée. »

## Diapo 8 — Chaîne de détection SQLi de bout en bout
**À dire :** « La chaîne fonctionne en six étapes. Nous envoyons une requête SQLi de test. Le WAF détecte la règle CRS 942100, puis bloque la requête en HTTP 403. L’événement est écrit dans les logs Docker. L’agent Ubuntu le collecte et l’envoie à Wazuh. Enfin, notre règle personnalisée 100501 produit une alerte de niveau 10, visible dans Threat Hunting. Nous avons ainsi relié le blocage local à la supervision centralisée. »

**Précision technique :** « La règle de démonstration recherche le motif 942100 dans le journal JSON Docker ; elle reste à renforcer pour un usage en production. »

**Transition :** « Ces tests donnent de bons résultats, mais il reste des limites à présenter. »

## Diapo 9 — Résultats, limites et améliorations
**À dire :** « Nous avons validé le filtrage et les tests Suricata, le blocage SQLi par le WAF, la restriction des accès NetBird et la remontée de l’alerte dans Wazuh. Notre principale limite est matérielle : nous avons travaillé sur un PC de 16 Go avec peu d’espace disque. Autre limite : nous n’avons pas encore démontré la collecte centralisée des journaux pfSense/Suricata et NetBird. Pour la suite, il faudra fiabiliser la collecte, les sauvegardes, la rétention des logs et les règles de détection. »

**Transition :** « Je termine avec notre scénario de démonstration. »

## Diapo 10 — Conclusion & démonstration
**À dire :** « Pour résumer, notre laboratoire montre trois fonctions complémentaires : prévenir, détecter et superviser. Une requête normale passe en HTTP 200 ; une injection SQL est bloquée en HTTP 403 ; puis nous retrouvons l’alerte associée dans Wazuh. Le résultat important n’est pas seulement le blocage, mais le fait de rendre cet événement visible pour une analyse SOC. Merci pour votre attention. »

**Démo courte si le temps le permet :** `curl -I http://127.0.0.1:8080/` pour HTTP 200, puis la commande SQLi du script `scripts/test-waf.sh` pour HTTP 403, et enfin la capture de la règle 100501. Ne pas relancer toutes les VM au risque de saturer la RAM.

## Diapo 11 — Preuve finale : Wazuh Threat Hunting, règle 100501
**À dire :** « Pour finir, voici la preuve directement dans Wazuh. On voit deux alertes provenant de notre agent p5-client-01. Elles déclenchent toutes les deux la règle personnalisée 100501, de niveau 10, qui correspond à une injection SQL détectée par ModSecurity. Cela confirme que notre chaîne de détection fonctionne de bout en bout. »

**Attention à l’enchaînement :** cette diapo arrive après la conclusion dans le fichier actuel. La présenter comme **une preuve finale complémentaire**, ou déplacer cette diapo juste après la diapo 8 si vous modifiez ensuite le PowerPoint.

---

## Questions possibles du jury — réponses courtes

**Pourquoi pfSense plutôt qu’OPNsense ?** « Nous avons retenu pfSense pour les tests de pare-feu et de Suricata. Pour l’architecture d’entreprise, OPNsense reste une alternative à comparer selon les contraintes et le support attendu. »

**Quelle différence entre IDS et IPS ?** « L’IDS alerte ; l’IPS peut bloquer le trafic. Nous avons démontré les deux modes sur un test ICMP contrôlé. »

**Que signifie la règle 100501 ?** « C’est notre règle Wazuh personnalisée. Elle recherche le motif de détection SQLi CRS 942100 dans le journal Docker du WAF, puis crée une alerte de niveau 10. »

**Est-ce prêt pour la production ?** « Non. Le PoC valide des scénarios ciblés. La haute disponibilité, l’intégration IAM, le dimensionnement, la rétention des journaux et la centralisation des trois briques restent à industrialiser. »

**Pourquoi NetBird ne fonctionnait plus à la fin ?** « Notre passerelle pfSense était éteinte pendant les essais Wazuh. Ubuntu n’avait donc plus de sortie Internet normale vers le plan de gestion NetBird. Les tests ZTNA avaient été validés avant ce changement. »

**Pourquoi Wazuh affichait des alertes avant l’installation d’un agent ?** « Le serveur Wazuh surveillait aussi ses événements locaux, notamment les sessions et contrôles de configuration. »

## Captures essentielles le jour J
- Capture Suricata : alerte IDS puis blocage IPS.
- Capture WAF : HTTP 200 et HTTP 403, plus extrait ModSecurity `942100` / `949110`.
- Capture NetBird : TCP 8080 autorisé, SSH 22 refusé, puis révocation.
- Capture Wazuh : agent `p5-client-01` actif et **Threat Hunting, règle 100501**.

**Rappel :** le périmètre du test WAF → Wazuh est réellement démontré ; ne pas affirmer que Suricata et NetBird envoient déjà leurs journaux à Wazuh.
