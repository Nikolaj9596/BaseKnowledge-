
1. Get repository focalboard

```bash
npm update
npm install cross-env

# Download the focalboard source and install all relevant dependencies
cd /usr/src
git clone https://github.com/mattermost/focalboard.git focalboard
cd focalboard/import
npm install
cd webapp
npm install

# convert the exported trello.json file to an archive.boardarchive file
cd trello
npx ts-node importTrello.ts -i /path/to/trello.json -o /path/to/archive.boardarchive
```

