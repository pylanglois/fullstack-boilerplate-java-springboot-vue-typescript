# Mise en route rapide

```powershell
# 1. Installer Java 21 (OpenJDK)
winget install --id Microsoft.OpenJDK.21 -e

# 2. Installer Node.js LTS (inclut npm)
winget install --id OpenJS.NodeJS.LTS -e

# 3. Installer les dépendances du projet
npm install

# 4. (Optionnel) Construire puis démarrer les services
npm run build
npm run start
```
