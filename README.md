# Angular app in docker container  
hosted from web server nginx
![](https://wuschools.com/wp-content/uploads/2020/01/angulardocker.jpg)

### Step by step how to do :-)

1. Generate new angular app with cli `ng new docker-ng` 
    ```
       ? Would you like to add Angular routing? No
       ? Which stylesheet format would you like to use? CSS
       CREATE docker-ng/README.md (1026 bytes)
       CREATE docker-ng/.editorconfig (246 bytes)
       CREATE docker-ng/.gitignore (631 bytes)
       CREATE docker-ng/angular.json (3617 bytes)
       CREATE docker-ng/package.json (1295 bytes)
       CREATE docker-ng/tsconfig.json (543 bytes)
       CREATE docker-ng/tslint.json (1953 bytes)
       CREATE docker-ng/browserslist (429 bytes)
       CREATE docker-ng/karma.conf.js (1021 bytes)
       CREATE docker-ng/tsconfig.app.json (270 bytes)
       CREATE docker-ng/tsconfig.spec.json (270 bytes)
       CREATE docker-ng/src/favicon.ico (948 bytes)
       CREATE docker-ng/src/index.html (294 bytes)
       CREATE docker-ng/src/main.ts (372 bytes)
       CREATE docker-ng/src/polyfills.ts (2838 bytes)
       CREATE docker-ng/src/styles.css (80 bytes)
       CREATE docker-ng/src/test.ts (642 bytes)
       CREATE docker-ng/src/assets/.gitkeep (0 bytes)
       CREATE docker-ng/src/environments/environment.prod.ts (51 bytes)
       CREATE docker-ng/src/environments/environment.ts (662 bytes)
       CREATE docker-ng/src/app/app.module.ts (314 bytes)
       CREATE docker-ng/src/app/app.component.css (0 bytes)
       CREATE docker-ng/src/app/app.component.html (25498 bytes)
       CREATE docker-ng/src/app/app.component.spec.ts (990 bytes)
       CREATE docker-ng/src/app/app.component.ts (213 bytes)
       CREATE docker-ng/e2e/protractor.conf.js (808 bytes)
       CREATE docker-ng/e2e/tsconfig.json (214 bytes)
       CREATE docker-ng/e2e/src/app.e2e-spec.ts (642 bytes)
       CREATE docker-ng/e2e/src/app.po.ts (262 bytes)
       
       added 1443 packages from 1068 contributors and audited 19053 packages in 85.087s
       
       23 packages are looking for funding
         run `npm fund` for details
       
       found 0 vulnerabilities
       
           Successfully initialized git.
   ```
   
2. Add dockerfile `touch Dockerfile`
    ```
        ### STAGE 1: Build ###
        FROM node:12.7-alpine AS build
        WORKDIR /usr/src/app
        COPY package.json ./
        RUN npm install
        COPY . .
        RUN npm run build
    
        ### STAGE 2: Run ###
        FROM nginx:1.17.1-alpine
        COPY --from=build /usr/src/app/dist/docker-ng /usr/share/nginx/html

    ``` 

3. And now we need build docker image. Run this command `docker build -t docker-ng-image .  `

    ```
       Sending build context to Docker daemon  283.6MB
       Step 1/8 : FROM node:12.7-alpine AS build
       12.7-alpine: Pulling from library/node
       e7c96db7181b: Already exists 
       72484f09da35: Pull complete 
       86bee4bed5f2: Pull complete 
       f9e983f0fe2c: Pull complete 
       Digest: sha256:300e3d2c19067c1aec9d9b2bd3acbd43d53797a5836d70a23e437a5634bcd33a
       Status: Downloaded newer image for node:12.7-alpine
        ---> d97a436daee9
       Step 2/8 : WORKDIR /usr/src/app
        ---> Running in 4e03b4fc2890
       Removing intermediate container 4e03b4fc2890
        ---> 4d8642f1adb4
       Step 3/8 : COPY package.json ./
        ---> 885b5bddfad4
       Step 4/8 : RUN npm install
        ---> Running in 873f0b728b3c
   
        added 1167 packages from 1048 contributors and audited 19053 packages in 180.92s
        found 0 vulnerabilities
        
        Removing intermediate container 873f0b728b3c
         ---> 686f8bcf1dca
        Step 5/8 : COPY . .
         ---> 548588613316
        Step 6/8 : RUN npm run build
         ---> Running in c2b83c833f0b
        
        > docker-ng@0.0.0 build /usr/src/app
        > ng build
        
        Generating ES5 bundles for differential loading...
        ES5 bundle generation complete.
        
        chunk {runtime} runtime-es2015.js, runtime-es2015.js.map (runtime) 6.16 kB [entry] [rendered]
        chunk {runtime} runtime-es5.js, runtime-es5.js.map (runtime) 6.16 kB [entry] [rendered]
        chunk {styles} styles-es2015.js, styles-es2015.js.map (styles) 9.7 kB [initial] [rendered]
        chunk {styles} styles-es5.js, styles-es5.js.map (styles) 10.9 kB [initial] [rendered]
        chunk {main} main-es2015.js, main-es2015.js.map (main) 46.4 kB [initial] [rendered]
        chunk {main} main-es5.js, main-es5.js.map (main) 49.9 kB [initial] [rendered]
        chunk {polyfills} polyfills-es2015.js, polyfills-es2015.js.map (polyfills) 264 kB [initial] [rendered]
        chunk {polyfills-es5} polyfills-es5.js, polyfills-es5.js.map (polyfills-es5) 683 kB [initial] [rendered]
        chunk {vendor} vendor-es2015.js, vendor-es2015.js.map (vendor) 3.48 MB [initial] [rendered]
        chunk {vendor} vendor-es5.js, vendor-es5.js.map (vendor) 4.21 MB [initial] [rendered]
        Date: 2020-01-25T20:50:42.668Z - Hash: f72b634fab003c855b42 - Time: 51255ms
        Removing intermediate container c2b83c833f0b
         ---> dbe3a8dca55e
        Step 7/8 : FROM nginx:1.17.1-alpine
        1.17.1-alpine: Pulling from library/nginx
        e7c96db7181b: Already exists 
        3fb6217217ef: Pull complete 
        Digest: sha256:17bd1698318e9c0f9ba2c5ed49f53d690684dab7fe3e8019b855c352528d57be
        Status: Downloaded newer image for nginx:1.17.1-alpine
         ---> ea1193fd3dde
        Step 8/8 : COPY --from=build /usr/src/app/dist/docker-ng /usr/share/nginx/html
         ---> 75381a8226ae
        Successfully built 75381a8226ae
        Successfully tagged docker-ng-image:latest
   ```
4. We check if the image has been created with command `docker images`

    ```
       REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
       docker-ng-image                        latest              75381a8226ae        4 minutes ago       36.8MB
       nginx                                  1.17.1-alpine       ea1193fd3dde        6 months ago        20.6MB
       node                                   12.7-alpine         d97a436daee9        6 months ago        79.3MB
    ```

5. And last command to run container `docker run --name docker-ng-container -d -p 8080:80 docker-ng-image`
6. We check if the container has been created with command `docker ps`
    ```
       CONTAINER ID        IMAGE                            COMMAND                  CREATED             STATUS              PORTS                    NAMES
       87f736fa52cf        docker-ng-image                  "nginx -g 'daemon of…"   29 seconds ago      Up 28 seconds       0.0.0.0:8080->80/tcp     dicker-ng-container
    ```
   
   Successful - your app is now hosted from docker container. Open your app url http://localhost:8080
