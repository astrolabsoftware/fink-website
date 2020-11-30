# Fink website

To deploy locally

```
docker run --rm -it  -p 1313:1313 -v $(pwd):/src klakegg/hugo:0.78.2 \
  server \
  -t vanilla-bootstrap-hugo-theme \
```

To deploy

```
docker run --rm -it  -p 1313:1313 -v $(pwd):/src klakegg/hugo:0.78.2 \
  -t vanilla-bootstrap-hugo-theme \
  --baseUrl="https://fink-broker.org" \
  --buildDrafts \
  --cleanDestinationDir
```
