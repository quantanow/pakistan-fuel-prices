# Publishing this dataset

Both platforms need the maintainer's own account, so these steps are manual.
The cards themselves are generated from the data by
`scripts/dataset_cards.py` — re-run the mirror build to refresh them rather
than editing them by hand, or the coverage numbers will drift from the 310
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

```bash
pip install kaggle                 # credentials at ~/.kaggle/kaggle.json
mkdir -p kaggle-upload && cd kaggle-upload
cp ../data/prices.csv .
sed 's/YOUR_KAGGLE_USERNAME/<you>/' ../dataset-cards/kaggle-dataset-metadata.json \
  > dataset-metadata.json
kaggle datasets create -p . --dir-mode zip
```

Then paste `dataset-cards/kaggle-description.md` into the dataset description
in the web UI — the CLI does not upload long descriptions.

For later refreshes:

```bash
kaggle datasets version -p . -m "Refresh through <date>"
```

## Both

Link back to https://github.com/quantanow/pakistan-fuel-prices and https://oilprices.pk/fuel-price-api-pakistan in the description. The inbound link
is the reason for publishing here; a dataset with no route back to the source
earns nothing.
