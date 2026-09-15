# Publishing this dataset

Both platforms need the maintainer's own account, so these steps are manual.
The cards themselves are generated from the data by
`scripts/dataset_cards.py` — re-run the mirror build to refresh them rather
than editing them by hand, or the coverage numbers will drift from the 346
records actually shipped.

## HuggingFace

```bash
pip install huggingface_hub
hf auth login                      # needs a write token
hf repo create pakistan-fuel-prices --repo-type dataset
git clone https://huggingface.co/datasets/<you>/pakistan-fuel-prices hf-dataset
cd hf-dataset
mkdir -p data
cp ../data/prices.csv data/
cp ../dataset-cards/huggingface-README.md README.md
git add -A && git commit -m "Add Pakistan fuel price dataset" && git push
```

The `configs` block in the card points the dataset viewer at `data/prices.csv`,
so the preview table works with no loading script.

## Kaggle

Published 2026-09-11 at
https://www.kaggle.com/datasets/imransaleem/pakistan-fuel-prices-ogra

```bash
pip install kaggle                 # CLI 2.2.4; see the auth note below
mkdir -p kaggle-upload && cd kaggle-upload
cp ../data/prices.csv .
sed 's/YOUR_KAGGLE_USERNAME/<you>/' ../dataset-cards/kaggle-dataset-metadata.json \
  > dataset-metadata.json
python -c "import json;m=json.load(open('dataset-metadata.json'));m['description']=open('../dataset-cards/kaggle-description.md').read();json.dump(m,open('dataset-metadata.json','w'),indent=2)"
kaggle datasets create -p . -u --dir-mode zip
```

Four things this got wrong before 2026-09-11, each measured against the API:

- **`~/.kaggle/kaggle.json` no longer exists.** CLI 2.2.4 dropped it. Auth is
  `kaggle auth login` (browser OAuth, credentials cached, expires and expects a
  human) or a bare token in `~/.kaggle/access_token` / `KAGGLE_API_TOKEN`. The
  settings UI hands you a token string to copy, not a file to download, so
  waiting for a download that never arrives is the first thing that happens.
- **`-u` is mandatory or the dataset is created PRIVATE.** A private dataset
  earns no inbound link, which is the only reason to publish here at all.
- **The CLI DOES upload long descriptions** — put `description` in
  `dataset-metadata.json` (the `python -c` line above) and it lands in full.
  The old instruction to paste it into the web UI was wrong; verified by
  reading it back with `kaggle datasets metadata`.
- **Keywords are a controlled taxonomy with a hard cap.** Free-form tags are
  dropped with "The following are not valid tags", and too many valid ones fail
  the whole version with "You have exceeded the max category limit". See
  `KAGGLE_CATEGORIES`.

For later refreshes:

```bash
kaggle datasets version -p . -m "Refresh through <date>"
```

Nothing runs that automatically. `sync-data-mirror.sh` pushes the GitHub mirror
on every price commit but does not touch Kaggle, so the dataset goes stale the
day after an upload. Automating it needs the static `access_token` form in the
launchd environment, since cached OAuth credentials expect a human to
re-approve.

## Both

Link back to https://github.com/quantanow/pakistan-fuel-prices and https://oilprices.pk/fuel-price-api-pakistan in the description. The inbound link
is the reason for publishing here; a dataset with no route back to the source
earns nothing.
