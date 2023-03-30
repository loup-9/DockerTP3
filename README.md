TP 3 Docker - Build avec multi-stage

1.	Créez un nouveau repository git

https://github.com/loup-9/DockerTP3.git

2.	Initialisez un projet React vierge
npx create-react-app my-app
>>
Need to install the following packages:
  create-react-app@5.0.1
Ok to proceed? (y) y
npm WARN deprecated tar@2.2.2: This version of tar is no longer supported, and will not receive security updates. Please upgrade asap.

Creating a new React app in D:\cours\MASTER2\pipeline\DockerTP3\my-app.

Installing packages. This might take a couple of minutes.
Installing react, react-dom, and react-scripts with cra-template...


added 1423 packages in 3m

233 packages are looking for funding
  run `npm fund` for details

Installing template dependencies using npm...

added 62 packages, and changed 1 package in 10s

233 packages are looking for funding
  run `npm fund` for details
Removing template package using npm...


removed 1 package, and audited 1485 packages in 3s

233 packages are looking for funding
  run `npm fund` for details

6 high severity vulnerabilities

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.

Success! Created my-app at D:\cours\MASTER2\pipeline\DockerTP3\my-app
Inside that directory, you can run several commands:

  npm start
    Starts the development server.

  npm run build
    Bundles the app into static files for production.

  npm test
    Starts the test runner.

  npm run eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd my-app
  npm start

Happy hacking!


3.	Vérifiez que l'application fonctionne correctement en local
cd my-app
npm start

 

 

4.	Créez un fichier Dockerfile à la racine du projet. Celui-ci doit se diviser en deux grandes parties :

a.	Étape 1 : Construction de l'application React FROM node:18-alpine as build 


FROM node:18-alpine as build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

B Étape 2 : Serveur web pour l'application FROM nginx:alpine


FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

5.	Instanciez l’image créé précédemment afin d’observer le même résultat que lors de la question 3

docker build -t my-app-image .
docker run -p 80:80 my-app-image
