# fdroidclient-apk-next

🇺🇸 [English (US)](README.en-US.md) ｜ 🇨🇳 [简体中文](README.zh-CN.md) ｜ 🇫🇷 **Français** ｜ 🇪🇸 [Español](README.es-ES.md) ｜ 🇯🇵 [日本語](README.ja-JP.md) ｜ 🇷🇺 [Русский](README.ru-RU.md) ｜ 🇰🇷 [한국어](README.ko-KR.md)

F-Droid a cessé de publier ses APK clients sur GitHub. Ce dépôt permet de garder une porte ouverte.

Je suis collégien en Chine et je veux être honnête : mes compétences techniques sont encore limitées. Je ne peux pas modifier le code de F-Droid ni ajouter de fonctionnalités, et bien des choses dans l'univers du logiciel libre me dépassent encore. Mais j'aime profondément ce que représente l'open source : l'idée que les logiciels peuvent être libres et transparents, et que des inconnus peuvent s'entraider et les créer ensemble, simplement parce que c'est la bonne chose à faire. Cette idée m'a donné envie de contribuer à mon tour, même modestement.

Voici le rôle que je peux jouer : F-Droid est hébergé sur GitLab et ne publie plus ses nouvelles versions sur GitHub. Lorsque le site officiel ou GitLab est inaccessible — ce qui arrive souvent là où je vis — il devient impossible d'obtenir la dernière version du client. Sans pouvoir installer la boutique, on ne peut pas découvrir les milliers d'applications libres qu'elle propose. Cela me semble injuste, surtout pour les personnes comme moi : les élèves, les jeunes curieux et tous ceux qui commencent tout juste à apprendre. Ce dépôt est donc ma réponse. J'y mets en miroir chaque nouvelle version officielle du client Android F-Droid, telle que F-Droid la compile et la signe — sans aucune modification ni reconditionnement, jamais. Voyez-le comme une porte supplémentaire : un autre endroit où trouver le client quand la porte principale est inaccessible.

## Contenu de chaque version

À chaque nouvelle version officielle du client, je téléverse **deux fichiers essentiels, sans exception** — ainsi qu'une clé officielle, conservée avec eux pour celles et ceux qui souhaitent pousser la vérification plus loin :

- `F-Droid_<version>.apk` — le client officiel, inchangé
- `F-Droid_<version>.apk.sha256.txt` — un fichier texte contenant le **hachage du fichier** APK correspondant
- `f-droid.org-signing-key.gpg` — la clé publique GPG officielle, pour les vérifications avancées de la chaîne de publication (voir ci-dessous)

Le fichier `.sha256.txt` est un simple fichier texte. Ouvrez-le sur n'importe quel téléphone : vous y verrez une ligne contenant le SHA-256 de cette version. Je garde volontairement l'extension `.txt` — pas d'astuce consistant à supprimer l'extension — afin que le fichier s'ouvre sur tous les appareils et reste lisible pour les débutants.

## À propos du fichier `.sha256.txt` (à lire attentivement)

**Le hachage contenu dans le fichier `.txt` est celui du fichier APK lui-même. Ce n'est PAS l'empreinte du certificat de signature. Ce sont deux choses complètement différentes.**

- Chaque version de l'APK est différente : **le hachage de ce fichier change donc à chaque version.** C'est normal : nouvelle version, nouveau hachage.
- Il prouve uniquement que le téléchargement n'a pas été corrompu en cours de route. **Il ne prouve pas qui a signé le paquet.**
- Pour confirmer qu'il s'agit bien du paquet officiel de F-Droid, vérifiez séparément le certificat (voir la section suivante).

Vérification rapide sur la plupart des ordinateurs :

```sh
sha256sum -c F-Droid_*.apk.sha256.txt
```

Vous n'avez pas `sha256sum` sur votre téléphone ? Ouvrez simplement le fichier `.txt` et comparez le hachage qu'il contient à celui affiché sur la page de publication. S'ils correspondent, le téléchargement est intact.

## Vérifiez aussi la signature (ne sautez pas cette étape)

Il y a trois niveaux de vérification. Pour une installation courante, les deux premiers suffisent. Le troisième est une clé de secours pour les personnes qui veulent tout vérifier dans le détail.

| | Hachage du fichier (le `.txt`) | Empreinte du certificat (**l'essentiel sur Android**) | Clé publique GPG (chaîne de publication, avancé) |
|---|---|---|---|
| Ce que cela prouve | Que le téléchargement n'est pas corrompu | Que le paquet provient bien de F-Droid | Que la chaîne de publication officielle n'a pas été altérée |
| Est-ce que cela change ? | **À chaque version** | **Jamais** (valide jusqu'en 2037) | **L'empreinte reste identique** (valide jusqu'en 2037 ; le renouvellement conserve la même clé) |
| Comment vérifier | `sha256sum -c`, ou en comparant visuellement | `keytool -printcert -jarfile` | `gpg --verify` (nécessite le fichier `.asc`) |
| Priorité au quotidien | Pratique | ⭐ **L'essentiel — à vérifier à chaque installation** | Facultatif, sur ordinateur |

L'**empreinte du certificat Android** à rechercher est **toujours** celle-ci — c'est elle qu'Android vérifie lors de l'installation et des mises à jour :

```
43238d512c1e5eb2d6569f4a3afbf5523418b82e0a3ed1552770abb9a9c9ccab
```

(F-Droid indique que ce certificat de signature à long terme est valide jusqu'en 2037.)

- **Le hachage de l'APK ne correspond pas ?** C'est normal : chaque version a le sien.
- **Le certificat ne correspond pas ?** Arrêtez-vous. **N'installez pas l'application**, même si le nom du fichier semble correct ou si le hachage paraît « joli ».
- Sous Linux / macOS, exécutez cette commande :

  ```sh
  keytool -printcert -jarfile F-Droid_*.apk
  ```

  Elle doit afficher la même valeur de certificat. Android refusera également toute mise à jour ultérieure dont la signature ne correspond pas : bien réussir la **toute première installation** permet donc de garder les suivantes sûres.

### Pour aller plus loin : signature GPG des publications (facultatif, mais utile à connaître)

Le fichier `f-droid.org-signing-key.gpg` fourni dans ce dépôt contient la clé publique officielle. C'est **une clé différente du certificat de signature Android** ci-dessus : ce sont deux clés distinctes, à ne pas confondre.

- **Certificat Android** (colonne du milieu ci-dessus) : celui qu'Android vérifie à l'installation. Il empêche l'usurpation d'identité. Jusqu'en 2037, son empreinte reste `43238d…ccab`.
- **Clé publique GPG** (cette section) : elle sert aux vérifications sur ordinateur. Elle permet de vérifier le fichier de signature officiel `.asc` placé à côté de l'APK et de confirmer que le paquet provient de la chaîne de publication de F-Droid.

**Comment utiliser le fichier `.gpg` :**

1. Importez la clé une fois :

   ```sh
   gpg --import f-droid.org-signing-key.gpg
   ```

   L'empreinte de la clé principale officielle est indiquée ici — comparez-la lors de l'importation :

   ```
   37D2 C987 89D8 3119 4839  4E3E 41E7 044E 1DBA 2E89
   ```

   Même empreinte = même clé, simplement renouvelée.

2. Vérifiez la signature de la publication (il vous faut le fichier `.asc` officiel fourni avec l'APK) :

   ```sh
   gpg --verify F-Droid_<version>.apk.asc F-Droid_<version>.apk
   ```

   Le message `Good signature from "F-Droid ..."` signifie que la chaîne de publication est intacte.

**⏳ À savoir concernant l'expiration en 2037 :**

Cette clé GPG est valide jusqu'en **2037**. Après cette date, la vérification peut afficher `key expired`. C'est une **expiration normale, pas une altération** : l'équipe officielle renouvelle la clé, et une empreinte inchangée signifie qu'il s'agit toujours de la même clé.

> Pour installer l'application sur un téléphone au quotidien, l'**empreinte du certificat Android** ci-dessus et un fichier `.sha256.txt` correspondant suffisent largement — c'est déjà très solide. GPG est une précaution supplémentaire. La clé est déjà ici ; vous pourrez l'utiliser quand vous en aurez besoin.

## Je vais continuer

Ce dépôt n'est pas un téléversement ponctuel, et je ne vais pas disparaître après la première version. Je continuerai aussi longtemps que possible — malgré les réseaux instables et les journées d'école chargées. Je ne peux pas promettre d'être toujours rapide, mais je peux promettre d'être toujours là. Une nouvelle version officielle sort ? Vous trouverez ici l'APK, son fichier `.sha256.txt` et la clé `.gpg` — les mêmes fichiers, les mêmes règles.

## Une précision en toute honnêteté

**Je ne suis ni affilié à F-Droid ni soutenu par le projet.** Je suis simplement un jeune qui a eu la chance de découvrir l'open source et qui souhaite le faire découvrir à d'autres. Je suis encore jeune et mes capacités sont limitées, mais ce miroir représente tout le soin que je peux y mettre. S'il aide ne serait-ce qu'une personne à découvrir le logiciel libre — un jeune qui se met à poser des questions, quelqu'un qui apprend quelque chose de nouveau — alors chaque synchronisation bloquée et chaque attente tard dans la nuit en valait la peine. Mon souhait est simple : aider quelques personnes de plus à franchir cette porte. Si vous êtes arrivé jusqu'ici, peut-être transmettrez-vous cette aide à votre tour, à votre façon. **C'est ainsi que nous grandissons tous ensemble.** 🤝