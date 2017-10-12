# Packagist

## Adding a module to Packagist

- **Create new public repository on Github and commit module files into root of repository**
  - ex: https://github.com/origindesign/origin_test
  - see example composer.josn
- **Submit module to Packagist**
  - https://packagist.org/packages/submit
  - ex: https://packagist.org/packages/origindesign/origin_test
- **Link Github repositry to Packagist for auto updates when you push**
  - Add Packagist as new service
    - Github repo > Settings > Integrations and Services
  - Link with API key and username (origindesign) from packagist
    - Packagist > Account name > Profile > Show API token
    
## Creating a new version of module
- https://packagist.org/about
 - https://git-scm.com/book/en/v2/Git-Basics-Tagging


**Create a branch for new version**
```
git checkout -b v1.1
```
**Make changes, update module version, test, commit**
```
git add -A
git commit -m "Created version 1.1"
```

**Merge branch into master**
```
git checkout master
git merge v1.1
```

**Delete working branch**
```
git branch -d v1.1
```
**Create a tag for the new version and push**
```
git tag -a v1.1 -m "Version 1.1"
git push origin v1.1
```

**Require module to your project**
```
composer require origindesign/origin_test
```
