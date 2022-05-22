# github-heroku-workflow
Add a github workflow that will auto update your Heroku app upon pushing changes in your git repo's main branch.

## Folow these step by steps
1. Push your app to its dedicated github repository.
2. Create an app in Heroku.
3. Copy this script to this directory (server/app.js) from your app's root directory. This will serve the public folder of your react app. Go inside (./server) directory.
  ```js
  const path = require('path');
const express = require('express');
const port = process.env.PORT || 3000;
const app = express();

const publicPath = path.join(__dirname, '..', 'build');
app.use(express.static(publicPath));

app.get('*', (req, res) => {
  res.sendFile(path.join(publicPath, 'index.html'));
}); 

app.listen(port, () => {
  console.log(`Server is up on port ${port}!`);
});
  ```
4. Initiate npm by `npm init --y` then install express by `npm install express` and make a `.gitignore` with `node_modules` inside. Hit save. 
5. Create a Procfile file in the root directory and put `web: node ./server/app.js` and hit save.
6. Open new terminal and type `heroku login` then open another terminal.
7. Add and commit all changes to your local repo, and add a remote repo for heroku by typing in `heroku git:remote -a [herokuappname]`.
8. After adding a remote repo, push your app to heroku by `git push heroku [gitmainbranch]`.
9. Now create this directory from your app's root directory (.github/workflows/main.yml)
10. Copy this script inside `main.yml`.
```yml
name: Deploy

on:
  push:
    branches:
      - [gitmainbranch]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: akhileshns/heroku-deploy@v3.12.12 # This is the action
        with:
          heroku_api_key: ${{secrets.HEROKU_API_KEY}}
          heroku_app_name: "[herokuappname]" #Must be unique in Heroku
          heroku_email: "[emailloggedinheroku]"
```
11. In your Heroku, open account settings and copy the api key.
12. In your github repo, go to settings and add a new secret named `HEROKU_API_KEY` with a value of the api key copied from your heroku account.
13. Save all files, add them to git, commit changes and push it to your github repository's main branch.
14. Now try to create changes in your app, run build and then push it to your github repository's main branch. You should see an action pending or running upon pushing your changes.
