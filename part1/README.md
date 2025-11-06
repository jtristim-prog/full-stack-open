# full-stack-open

node-ympäristö löytyy dockerista "node" -nimisenä imagena

Se saatiin näin:
https://nodejs.org/en/download
# Docker has specific installation instructions for each operating system.
# Please refer to the official documentation at https://docker.com/get-started/

# Pull the Node.js Docker image:
docker pull node:24-alpine

# Create a Node.js container and start a Shell session:
docker run -it --rm --entrypoint sh node:24-alpine

# Verify the Node.js version:
node -v # Should print "v24.11.0".

# Verify npm version:
npm -v # Should print "11.6.1".

===========================
PILKOTTUNA:
# Create a Node.js container and start a Shell session:
docker run -it --rm --entrypoint sh node:24-alpine

=>

docker create --entrypoint sh node:24-alpine
docker start -a -i <container_id>
docker exec -it <container_id> sh
docker rm <container_id>

===========================
PORTTI JULKAISTUNA:

docker run -it --rm --entrypoint sh node:24-alpine
=>
docker run -it --rm -p 5173:5173 --entrypoint sh node:24-alpine

VS CODE VOLUME (by ChatGPT):
docker run -it --rm \
  -p 5173:5173 \
  -v $(pwd):/app \
  -w /app \
  node:24-alpine \
  sh

Explanation:
-v $(pwd):/app mounts your current host directory ($(pwd)) into /app inside the container.
-w /app sets the working directory.
Now anything created inside /app in the container will be written to your host folder.

===========================
kurssin react-pohjan luonti:

npm create vite@latest
=> 
/ # npm create vite@latest
Need to install the following packages:
create-vite@8.0.2
Ok to proceed? (y)
=> paina enteriä

Tämän jälkeen:
-vastaillaa kysymyksiin kuten täällä ohjeistettu:
https://fullstackopen.com/en/part1/introduction_to_react

Lopuksi tulee tämmöinen output:
...
◇  Scaffolding project in /part1...
│
└  Done. Now run:

  cd part1
  npm install
  npm run dev

npm notice
npm notice New patch version of npm available! 11.6.1 -> 11.6.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.6.2
npm notice To update run: npm install -g npm@11.6.2
npm notice
/ #

cd part1
npm install

=>
added 199 packages, and audited 200 packages in 15s

32 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities

npm run dev

=>
 VITE v7.2.1  ready in 424 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help



TODO:
-portit auki
docker run -it --rm -p 5173:5173 --entrypoint sh node:24-alpine
OK

-yhteys selaimella reactiin
npm run dev -- --host 0.0.0.0 --port 5173
OK

-asenna vs code docker compose?
cd C:\Users\JuhaRistimäki\OneDrive - Remion Oy\Documents\siviiliasiat\opiskelu\full-stack-open\part1\my-react-app

1) Initialize the Vite/React app (inside the container)

Open a shell in the dev service:

docker compose run --rm dev sh


Now inside the container shell:

npm create vite@latest .     # note the dot to use /app (your host folder)
npm install
exit

You should now see the generated files on your Windows folder.

2) 
From your host terminal (PowerShell, Git Bash, etc.):

docker compose up vite


Open http://localhost:5173
 in your browser 🎉

If you prefer to keep it all in the one-off style:

docker compose run --rm --service-ports dev npm run dev -- --host


(--service-ports publishes ports defined in the service; -- --host is passed to Vite.)

-reactin muuttaminen vs codella
-volume, jotta muutokset säilyvät
-git containeriin