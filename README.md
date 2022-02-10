# Fink website

To deploy locally

```
git clone https://github.com/astrolabsoftware/fink-website.git
cd fink-website

# add submodule
git submodule init
git submodule update

# launch it
docker run --rm -it  -p 1313:1313 -v $(pwd):/src klakegg/hugo:0.81.0 \
  server \
  -t vanilla-bootstrap-hugo-theme \
```

To deploy

```
docker run --rm -it  -p 1313:1313 -v $(pwd):/src klakegg/hugo:0.81.0 \
  -t vanilla-bootstrap-hugo-theme \
  --baseUrl="https://fink-broker.org" \
  --buildDrafts \
  --cleanDestinationDir
```
