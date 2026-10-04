# FakeMuteDeafenLab

Un plugin Equicord/Vencord pour apparaître muet ou sourd aux autres tout en gardant ton micro et ton audio actifs en local.

> ⚠️ Les plugins tiers ne sont **PAS** supportés par les développeurs d'Equicord/Vencord. Utilisation à tes risques.

## Fonctionnalités

- 🎙️ Bouton compact près des contrôles vocaux en bas à gauche de Discord
- 🔇 **Fake Mute** : tu apparais muet aux autres, ton micro reste actif en local
- 🔕 **Fake Deafen** : tu apparais sourd aux autres, ton micro et ton audio restent actifs en local
- ♻️ Restaure automatiquement le media RTC local tant qu'un état fake est actif
- 🔄 Re-applique la restauration après une reconnexion vocale ou un changement de salon
- ⏹️ Appuyer sur un bouton natif Discord pendant qu'un fake est actif l'annule et applique le vrai état
- ⚙️ Réglages : délai de restauration, restauration automatique, logs de debug
- 🛠️ Helpers DevTools pour déboguer après une mise à jour de Discord

## Installation

Il te faut Equicord compilé depuis les sources (les userplugins ne fonctionnent que comme ça).

**Windows (PowerShell) :**
```powershell
cd C:\chemin\vers\Equicord\src\userplugins
git clone [https://github.com/hoho087/vencord-FakeMuteDeafenLab](https://github.com/epinayx/FakeDeafen-Equicord.git) fakeMuteDeafenLab
cd ..\..
pnpm build
```

Ensuite, quitte complètement Discord / Equibop, rouvre-le et active **FakeMuteDeafenLab** dans **Paramètres → Equicord → Plugins**.

## Utilisation

1. Rejoins un salon vocal.
2. Clique sur le bouton **FakeMuteDeafenLab** dans la zone des contrôles vocaux en bas à gauche.
3. Active **Fake Mute** ou **Fake Deafen** depuis le menu :
   - **Fake Mute** : tu apparais muet aux autres — ton micro continue de fonctionner en local.
   - **Fake Deafen** : tu apparais sourd aux autres — ton micro et ton audio continuent de fonctionner en local.
4. Pour annuler un état fake, décoche-le depuis le menu ou appuie sur le bouton natif Discord correspondant.

Tu peux aussi contrôler le plugin depuis **Paramètres → Plugins → FakeMuteDeafenLab**.

## Réglages

| Réglage | Description | Défaut |
|---|---|---|
| `showPanelButtons` | Affiche ou masque le bouton en bas à gauche | `true` |
| `autoRestoreLocalMedia` | Ré-applique périodiquement la restauration locale tant qu'un fake est actif | `true` |
| `restoreDelayMs` | Délai (ms) avant de restaurer le media local après un changement d'état vocal | `450` |
| `debugLogs` | Affiche des logs concis dans les DevTools | `false` |

## Helpers DevTools

```js
FakeMuteDeafenLabStatus()           // inspecter l'état actuel de la connexion RTC
FakeMuteDeafenLabSetMute(true)      // activer le fake mute depuis la console
FakeMuteDeafenLabSetDeafen(true)    // activer le fake deafen depuis la console
FakeMuteDeafenLabRestoreLocal()     // forcer la restauration du micro/audio local
```

## Fonctionnement

Discord maintient deux couches séparées : l'état vocal visible (`selfMute` / `selfDeaf`) diffusé aux autres utilisateurs, et la connexion RTC locale qui contrôle ton vrai micro et ta sortie audio.

Ce plugin patche la connexion RTC locale pour intercepter les appels `setSelfMute` et `setSelfDeaf` tant qu'un fake est actif, empêchant Discord de vraiment couper ton media local. L'état visible est modifié via les actions Flux natives de Discord pour que les autres voient le faux statut.

Quand tu appuies sur un bouton natif Discord pendant qu'un fake est actif, le plugin détecte l'intention et annule le fake pour que le vrai état s'applique proprement.

## Mise à jour

```powershell
cd C:\chemin\vers\Equicord\src\userplugins\fakeMuteDeafenLab
git pull
cd ..\..\..
pnpm build
```

## Dépannage

Si le plugin cesse de fonctionner après une mise à jour de Discord, les zones les plus susceptibles d'être en cause sont :

- La détection du module `RTCConnection`
- L'objet interne `_connection`
- Les méthodes media locales : `setSelfMute`, `setSelfDeaf`, `setAudioEnabled` ou leurs équivalents renommés
- Les changements dans le comportement de la connexion media ou des actions Flux de Discord

Active `debugLogs` et ouvre la console (Ctrl+Shift+I) pour voir les lignes `[FakeMuteDeafenLab]` et identifier le problème.

## Support

Telegram : [@epinay](https://t.me/epinay)

## Licence

MIT © 2026 Epinay 
