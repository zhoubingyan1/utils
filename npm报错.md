https://github.com/nuxt/nuxt/issues/6823
```
rm -rf node_modules/ package-lock.json;
npm cache clean --force;
npm i ; npm audit fix; npm update;
```