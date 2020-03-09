 
# After the creation of a new remote Git repository

## Git global setup
```
git config --global user.name "Frédéric LAUDARIN"
git config --global user.email "frederic.laudarin@supergrid-institute.com"
```

## Create a new repository
```
git clone git@gitlab.supergrid.com:flaudarin/opteasoft-wind-alpha-gui.git
cd opteasoft-wind-alpha-gui
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

## Existing folder
```
cd existing_folder
git init
git remote add origin git@gitlab.supergrid.com:flaudarin/opteasoft-wind-alpha-gui.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

## Existing Git repository
```
cd existing_repo
git remote rename origin old-origin
git remote add origin git@gitlab.supergrid.com:flaudarin/opteasoft-wind-alpha-gui.git
git push -u origin --all
git push -u origin --tags
```