# Phrase TMS projects App (frontend)

A simple user login app built using Vue.js. This app allows access to secure portion of website only after the user has authenticated. In the secured page the user can get the list of projects from a Phrase TMS account.

## Prerequisites

- Vue.js cli installed. To install cli run this command `npm install -g @vue/cli`. To check if the cli is already installed run the following command `vue --version`.

## Running Project

### Project setup

```
npm install
```

### Compiles and hot-reloads for development

```
npm run serve
```

## Notes

- The server runs on port 8080
- Sends requests to a backend server which listens on port 8081
- The backend server port can be configured in App.vue, using backendServerPort
- The backend server repository can be found here: https://github.com/luis-e-lopez/phrase-backend

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

## Useful Links

- https://cli.vuejs.org/guide/installation.html
