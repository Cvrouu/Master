<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="fr" lang="fr">
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
  </head>
  <body>
    <h1>Rapport sur une création de Vulnérabilité de Sécurité</h1>
    <p>Bouguerba Gihad</p>
    <h1 >Introduction</h1>
    <p>
      <br />
    </p>
    <p>Avant tout cela nous ne savions par quoi choisir. A vrai dire j’avais quelques idées en tête mais pas assez satisfaisant. Des les premiers TP nous avions pense a simplement mettre en place une machine BadStore qui accueillera plusieurs vulnérabilités en fonction de la version choisie. Comme dans la documentation du site web officiel :</p>
    <p>
      <br/>
    </p>
    <p>
      <br />
    </p>
    <p>
      <span>
        <table border="0" cellspacing="0" cellpadding="0">
          <tr>
            <td>
![image](https://github.com/Cvrouu/cvrouu/assets/114159408/a8e52219-042c-4bc8-ab0c-50ab11245d20)
            </td>
          </tr>
        </table>
      </span>
    </p>
    <p>
      <br />
    </p>
    <p>En gros c’est juste une machine possédant des vulnérabilités. Sauf que non, on s’est dit que cela serait trop facile et c’est une SAE il faut qu’il y ait du boulot. Alors on a pensé à une vulnérabilité très connue dans le monde de la cyber : le Server Side Request Forgery.</p>
    <h1>SSRF C’EST QUOI ?</h1>
    <p>
      <br />
    </p>
    <p>
      <span>
        <table border="0" cellspacing="0" cellpadding="0">
          <tr>
            <td>
![image](https://github.com/Cvrouu/cvrouu/assets/114159408/0915429a-3ee0-4a84-9e20-ae11ced4b29f)
            </td>
          </tr>
        </table>
      </span>
    </p>
    <p>Alors d’après nos amis de chez Portswigger : La falsification des requêtes côté serveur est une vulnérabilité de sécurité web qui permet à un attaquant d&#39;amener l&#39;application côté serveur à envoyer des requêtes à un endroit non désiré.</p>
    <p>Dans une attaque SSRF typique, l&#39;attaquant peut amener le serveur à établir une connexion avec des services internes à l&#39;infrastructure de l&#39;organisation. Dans d&#39;autres cas, il peut être en mesure de forcer le serveur à se connecter à des systèmes externes arbitraires. Cela pourrait entraîner la fuite de données sensibles, telles que les identifiants d&#39;autorisation.</p>
    <p>
      <br/>
    </p>
    <p>On a alors fabrique une maquette de sorte a ce qu’elle puisse faire en sorte de mettre</p>
    <p>en place cette vulnérabilité :</p>
    <p>
      <br/>
    </p>
    <p>
      <span>
        <table border="0" cellspacing="0" cellpadding="0">
          <tr>
            <td>
![image](https://github.com/Cvrouu/cvrouu/assets/114159408/ddef200f-f19a-4cf3-aa2f-d30832fa9658)
            </td>
          </tr>
        </table>
      </span>
    </p>
    <p>
      <br />
    </p>
    <p>À la suite d’une analyse approfondie de la sécurité du système, on a identifié une vulnérabilité potentielle liée à une attaque de déni de service. Il est important de noter que cette faille diffère quelque peu des attaques SSRF conventionnelles. Mais ca reste une SSRF.</p>
    <h1>Description de la Vulnérabilité</h1>
    <p>
      <br/>
    </p>
    <p>L&#39;attaque en question se base sur la manipulation du champ Host dans les requêtes HTTP. L&#39;attaquant cherche à exploiter cette faille en se faisant passer pour un PC interne, afin d&#39;accéder à un serveur WEB situé dans une DMZ située en bas à droite de la maquette. Ce serveur autorise l&#39;accès uniquement depuis une adresse IP spécifique, celle-ci étant celle du serveurWEB200.</p>
    <p>
      <br />
    </p>
    <h1>Déroulement de l&#39;Attaque</h1>
    <p>
      <br/>
    </p>
    <p>Etape 1 : On génère une requête falsifiée à partir de son propre PC, simulant une appartenance au sous-réseau de l&#39;entreprise.</p>
    <p>
      <br/>
    </p>
    <p>Etape 2 : En substituant le champ Host de la requête avec une adresse IP à deviner, on tente de contourner les restrictions d&#39;accès du serveur WEB dans la DMZ.</p>
    <p>
      <br />
    </p>
    <p>Etape 3 : Le serveur WEB dans la DMZ, pensant traiter une requête légitime du PC interne, répond avec un code HTTP 408.</p>
    <p>
      <br/>
    </p>
    <h1>Signification du Code HTTP 408</h1>
    <p>
      <br />
    </p>
    <p>Le code de statut de réponse HTTP 408 Request Timeout indique que le serveur souhaiterait clôturer cette connexion inutilisée. Pour certains serveurs, ce code est parfois envoyé sur une connexion inactive sans qu&#39;il y ait nécessairement eu de requête de la part du client.</p>
    <p>
      <br />
    </p>
    <p>Un serveur doit envoyer l&#39;en-tête Connection avec la valeur close en réponse, puisque 408 implique que le serveur a décidé de fermer la connexion plutôt que de continuer à attendre. C’est comme avec Scapy en TP en fait mais en moins chiant...</p>
    <p>On observe donc comme prévu un http 408 du côté serveur :</p>
    <p>
      <span>
        <table border="0" cellspacing="0" cellpadding="0">
          <tr>
            <td>
![image](https://github.com/Cvrouu/cvrouu/assets/114159408/573c2f59-a5ec-456c-aa04-d0d53c55e647)
            </td>
          </tr>
        </table>
      </span>
    </p>
    <p>
      <br />
    </p>
    <h1>Conséquences Potentielles</h1>
    <p>
      <br/>
    </p>
    <p>La manipulation réussie du champ Host peut entraîner une réponse HTTP 408 du serveur distant dans la DMZ. En répétant cette attaque, il est possible de provoquer un déni de service et potentiellement de faire crasher le serveur distant.</p>
    <p>
      <br/>
    </p>
    <h1>Solution Recommandée</h1>
    <p>
      <br/>
    </p>
    <p>Filtrage Strict des Requêtes: On recommande de mettre en place des filtres stricts pour les requêtes entrantes, en particulier pour le champ Host, afin de limiter la possibilité de manipulation.</p>
    <p>
      <br />
    </p>
    <p>Authentification Renforcée: Il est essentiel d&#39;implémenter des mécanismes d&#39;authentification robustes pour garantir que seuls les utilisateurs légitimes ont accès aux ressources sensibles.</p>
    <p>
      <br />
    </p>
    <p>Surveillance Continue : La mise en place d&#39;un système de surveillance peut détecter et réagir rapidement à des activités anormales, comme des tentatives répétées d&#39;attaques.</p>
    <p>
      <br />
    </p>
    <h1 style="padding-left: 141pt;text-indent: 0pt;text-align: center;">Conclusion</h1>
    <p style="padding-top: 1pt;padding-left: 5pt;text-indent: 0pt;line-height: 107%;text-align: left;">Dans les cas habituels les attaques SSRF exploitent souvent les relations de confiance pour intensifier une attaque à partir de l&#39;application vulnérable et effectuer des actions non autorisées. Ces relations de confiance peuvent exister par rapport au serveur ou par rapport à d&#39;autres systèmes dorsaux au sein de la même organisation. elle présente un risque de déni de service significatif. La mise en œuvre de mesures préventives solides est essentielle pour atténuer cette menace potentielle. Tout cela pour dire que notre ami peut devenir en un instant notre pire ennemi.</p>
  </body>
</html>
