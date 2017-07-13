# Drupal

See [Origin Drupal 8 project](https://github.com/origindesign/origin-drupal-8) in order to set drupal with docker and composer.

## Pattern Lab Starter Theme

follow steps on [Pattern Lab Starter] (https://github.com/phase2/pattern-lab-starter)

install patternlab extra depenencies
```
cd webroot/themes/custom/*themename*/pattern-lab
composer require aleksip/plugin-data-transform pattern-lab/plugin-data-inheritance
```

Fix component-libraries path issue on windows

gulconfig.js
change addToDrupalThemeFile to false

*themename*.info.yml
in component-libraries paths replace \ with /
