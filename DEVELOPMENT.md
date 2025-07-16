# Development

For now, until we get our GitHub actions workflow in place, you should manually generate your `index.json` and `index_hash.txt`.

To do this, you should install `Node.js` newer than 10.x. You can use `nvm` for this, see: https://github.com/nvm-sh/nvm?tab=readme-ov-file#installing-and-updating

```
nvm use 22
```

## Verification

There is a verification you can run to ensure your presets are valid. Run `node indexer/check.js` to see. Expected output should be something like

```
oats87@dev rotorflight-presets % node indexer/check.js
OK
```

## Generating index.json

To generate `index.json`, run `node indexer/indexer.js`

Output should be something like
```
oats87@dev rotorflight-presets % node indexer/indexer.js
OK
index.json created
index_hash.txt created
```
and your `index.json` and `index_hash.txt` files should be changed. These should be committed.

## Testing your Presets

Once you have done this, you can add your preset repo as a source in the configurator. This will allow your presets to show up provided you have generated your `index.json` and `index_hash.txt`

## Opening a PR

The best practice for opening a PR is to open a PR with at least two commits -- your final commit should contain just the `index.json` and `index_hash.txt` changes, as this allows for easier cherry-picking of presets/changes across repos.