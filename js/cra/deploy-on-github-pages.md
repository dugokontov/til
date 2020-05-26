## Deploying a CRA to GitHub Pages

1. Install npm package [gh-pages](https://github.com/tschaub/gh-pages) as dev dependency
```sh
yarn add --dev gh-pages
```
2. Add `homepage` property in `package.json`:
```json
"homepage": "http://{username}.github.io/{project-name}"
```
3. Add two more scripts in `package.json`:
```json
"scripts": {
  "predeploy": "npm run build",
  "deploy": "gh-pages -d build"
}
```
4. In console run
```sh
yarn run deploy
```
This command will commit content of the `build` folder to the branch `gh-pages`. Go to github settings and configure repository to enable github pages and set github pages to serve from `gh-pages` branch.
