# TP — Réparer la fenêtre modale

## Problèmes rencontrés

| # | Problème | Impact utilisateur | Correction | Vérification |
|---|---|---|---|---|
| 1 | « Réserver » était une `<div onClick>` | Inaccessible avec Tab, Entrée et Espace sans effet, aucun rôle exposé : un utilisateur clavier ou de lecteur d'écran ne peut pas réserver | `<button type="button">` natif | Tab atteint « Réserver Après l'aube », Entrée ouvre la fenêtre ; DevTools > Accessibility : rôle `button`, nom « Réserver Après l'aube » |
| 2 | La fenêtre était une `<div>` sans rôle ni nom, le focus restait derrière et le contenu de la page restait dans le parcours Tab | L'utilisateur ne sait pas qu'une fenêtre s'est ouverte ni ce qu'elle concerne, et continue à tabuler dans la page masquée | `<dialog>` ouvert par `showModal()` via `useRef`, `aria-labelledby="booking-title"` relié au `<h2 id="booking-title" tabIndex={-1}>`, focus placé sur le titre dans un `useEffect` | DevTools > Accessibility sur le `dialog` : rôle `dialog`, nom « Réserver Après l'aube » puis « Réserver Orbite 9 » ; `document.activeElement` = le `h2` à l'ouverture ; Tab ne fait tourner que « Fermer la réservation » et « Revenir aux films » |
| 3 | Le « × » était un `<span>` sans nom, pas de fermeture par Échap, et le focus était perdu après la fermeture | Impossible de fermer au clavier ; une fois fermé, l'utilisateur repart du haut de la page et perd sa place | « × » devient `<button aria-label="Fermer la réservation">` ; Échap géré nativement par `<dialog>` ; `onClose={handleClose}` remet le focus sur le bouton déclencheur mémorisé dans un `useRef` | Scénario clavier sur deux films : Échap, « Fermer la réservation » et « Revenir aux films » rendent chacun le focus au bon bouton (« Réserver Après l'aube » ou « Réserver Orbite 9 ») |

## Contraintes

- Aucun `tabIndex` positif : seul `tabIndex={-1}` sur le titre, pour recevoir le focus par script.
- Éléments natifs : `<button>` et `<dialog>`.
- Indicateur de focus conservé : `:focus-visible` inchangé, visible sur le titre du dialogue.
- Zoom 200 % : vérifié avec une fenêtre de 640 px de large, le dialogue reste lisible et ses boutons accessibles.
