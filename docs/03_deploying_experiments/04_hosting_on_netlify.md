# Hosting on Netlify 

1. Registration
    -  Go to [https://www.netlify.com/](https://www.netlify.com/) and sign up using GitHub
2. Deployment
    - Using git:
      - Click on the `New site from Git`-Button
      - choose GitHub and authorize the netlify-app on GitHub
      - configure which repositories to give access to
      - (back on netlify) select the repository to deploy
      - enter the build command `npm run build` and the publish directory `dist`
      - click on `Deploy site` 
    - Manual: Go to [https://app.netlify.com/](https://app.netlify.com/) and drag and drop your finished experiment folder (including node_modules) to the drag&drop area
3. Configuration
    - Change the domain name:
        - Click on the deployed site you want to configure, click on `Domain setting`, click on `Edit site name` and change to the name of choice.
