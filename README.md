The blog is built using Hugo and has a PaperMod theme.

### First time setup
References: [Hugo quickstart](https://gohugo.io/getting-started/quick-start/) and [PaperMod setup](https://github.com/adityatelange/hugo-PaperMod/wiki/Installation).
1. Install Hugo `brew install hugo`
2. Check Hugo installed version `hugo version`
3. Empty site init using `hugo new site mayankrajoria --format yaml`
4. From inside `mayankrajoria`: 
```
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod

git submodule update --init --recursive  # needed when you reclone your repo (submodules may not get cloned automatically)
```
5. In hugo.yml specify the theme by adding:
```
theme: ["PaperMod"]
```

### Dev
References: [Hugo quickstart](https://gohugo.io/getting-started/quick-start/)
Start dev server

```
cd mayankrajoria
hugo server
```

Build the site
```
hugo
```
