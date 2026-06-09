# Guide de Contribution

## 🚀 Comment Contribuer

Merci de votre intérêt pour contribuer à Pharmacy Management System! Voici comment faire:

### 1. Fork le Repository

```bash
git clone https://github.com/Olamide10-colab/pharmacy-management.git
cd pharmacy-management
```

### 2. Créer une Branche

```bash
git checkout -b feature/votre-feature
# ou
git checkout -b fix/votre-fix
```

### 3. Faire vos Modifications

- Écrivez du code propre et lisible
- Suivez les conventions de nommage
- Ajoutez des tests pour les nouvelles fonctionnalités
- Mettez à jour la documentation si nécessaire

### 4. Commit avec un Message Clair

```bash
git commit -m "feat: ajout de la fonctionnalité X"
# ou
git commit -m "fix: correction du bug Y"
```

**Format de commit:**
- `feat:` Nouvelle fonctionnalité
- `fix:` Correction de bug
- `docs:` Changements de documentation
- `style:` Formatage, manquant de point-virgule, etc.
- `refactor:` Refactorisation du code
- `test:` Ajout ou modification de tests

### 5. Push et Pull Request

```bash
git push origin feature/votre-feature
```

Créez une Pull Request avec une description claire de vos modifications.

## 📋 Standards de Code

### JavaScript/TypeScript

- Utilisez ESLint et Prettier
- Indentation: 2 espaces
- Utiliser const/let, pas var
- Utiliser les fonctions fléchées

### Nommage

- Variables/functions: `camelCase`
- Classes/Components: `PascalCase`
- Constantes: `UPPER_SNAKE_CASE`

### Tests

- Écrivez des tests unitaires
- Coverage minimum: 80%
- Utilisez Jest et React Testing Library

## 🔄 Workflow

1. Créer une issue pour discuter des modifications majeures
2. Forker et créer une branche
3. Faire les modifications et tests
4. Créer une Pull Request
5. Attendre la review et les approvals
6. Merge dans develop

## 📝 Checklist avant PR

- [ ] Code testé localement
- [ ] Tests ajoutés/mis à jour
- [ ] Linting passé (`npm run lint`)
- [ ] Prettier formaté (`npm run format`)
- [ ] Types TypeScript vérifiés
- [ ] Documentation mise à jour
- [ ] Pas de console.log en production
- [ ] Pas de credentials en dur

## ❓ Questions?

Créez une issue avec le label `question` ou contactez les mainteneurs.

Merci de votre contribution! 🙏
