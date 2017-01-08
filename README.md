# Awesomewm Wiki Dump

> Convert the old shuttered awesomewm wiki to markdown

# Background

The old AwesomeWM wiki is gone, taken down with other legacy awesomewm infrastructure in [awesome-www#7](https://github.com/awesomeWM/awesome-www/issues/7).

This archive has an xml dump, and a markdown conversion of that dump. It was introduced in [this comment](https://github.com/awesomeWM/awesome-www/issues/7#issuecomment-244820814).

It was made with the [mediawiki-to-markdown](https://github.com/philipashlock/mediawiki-to-markdown) converter.

# Usage

1. [Install Mediawiki to Markdown](https://github.com/philipashlock/mediawiki-to-markdown#installation). This is two step:
  1. Install packages (pandoc, php, composer).
    * On Debian, I was able to satisfy it's dependencies via `apt-get install pandoc composer php-xml`.
  1. Install the converter's packages: `composer install` from a checkout of it's source code.
1. Run the converter: `php ../mediawiki-to-markdown/convert.php --filename=awesome-wiki-data.xml`
