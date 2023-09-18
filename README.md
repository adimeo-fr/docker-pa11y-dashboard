# Docker pa11y-dashboard
A dockerised version of the [pa11y-dashboard project on GitHub](https://github.com/pa11y/pa11y-dashboard).

## How-to

Create a `.env` file (or modify the docker-compose file) to contain `MONGO_PASSWORD=RANDOM_STRING_OF_CHARACTERS`

### Run locally/example usage
```yml
services:
  pa11y-dashboard:
    build: Denperidge/docker-pa11y-dashboard
    ports:
      - 4000:4000
    environment:
      NODE_ENV: production
      WEBSERVICE_DATABASE: mongodb://root:${MONGO_PASSWORD}@database:27017/pa11y-webservice?authSource=admin
    depends_on:
      - database
    security_opt:
      - seccomp=chrome.json
  
  database:
    image: mongo:4.4
    restart: unless-stopped
    ports:
      - 27017:27017
    volumes:
      - ./db:/data/db
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: ${MONGO_PASSWORD}
```


### Building locally
Run the following commands:
```bash
git clone --recursive-submodules https://github.com/Denperidge-Redpencil/docker-pa11y-dashboard.git
docker compose up
```

## Discussions
### seccomp / chrome.json
To allow puppeteer inside of Docker to use sandboxes, the seccomp configuration illustrated [in this answer on Stackoverflow](https://stackoverflow.com/a/62383642) was implemented. It uses chrome.json, a file from [jessfraz's dotfiles repository on GitHub](https://github.com/jessfraz/dotfiles/blob/master/etc/docker/seccomp/chrome.json)

## License
pa11y-dashboard is licensed under the GNU General Public License v3.0. You can [view the pa11y-dashboard repository on GitHub].

As such, docker-pa11y-dashboard is also licensed under the GNU General Public License v3. See the [LICENSE in this project for more information](LICENSE).