# JS / Node.js

## Skrypty

### Publish na customowy registry

```sh
#!/bin/bash
scope=$(jq -r .name package.json | grep -oE '^[^/]+')
registry=http://jakis.tam.registry.com

npm config set "${scope}:registry" "${registry}"
npm publish || true
npm config delete "${scope}:registry"

```
