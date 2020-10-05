# JS / Node.js

## Skrypty

### Publish na customowy registry

```sh
#!/bin/bash
scope=$(jq -r .name package.json | grep -oE '^[^/]+')
registry=http://jakis.tam.registry.com

npm config set "${scope}:registry" "${registry}"
mv .npmrc .npmrc-tmp 2>&1 >/dev/null || true
npm publish || true
mv .npmrc-tmp .npmrc 2>&1 >/dev/null || true
npm config delete "${scope}:registry"

```
