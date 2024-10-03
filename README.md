# OpenMKBHD

Download the mkbhd Panels wallpapers, for free

## How

According to [this
tweet](https://x.com/uwokko/status/1838640935770440031) the backend is a google cloud storage file with
links to images on imgix that are free to download, with no
authentication required

## Requirements

* `curl`
* `jq`
* `grep`

### Instructions

```bash
mkdir -p images; \
echo "Listing all images"; \
curl -s https://storage.googleapis.com/panels-api/data/20240916/media-1a-i-p\~s |
    jq -r '.data | to_entries | map(.value.dhd)[]' |
    grep -v null |
    while read url; \
        do filename=$(basename "$url" | sed 's/\?.*//'); \
        echo "Downloading $filename"; \
        curl -s "$url" -o "./images/${filename%%.*}-image.png"; \
    done; \
echo "Done 🎉"
```
