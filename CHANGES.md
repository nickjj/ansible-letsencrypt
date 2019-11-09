# Changelog

### v0.3.0

*Released: November 9th 2019*

- Update to using the ACME v2 API since v1 is on its way to being deprecated

### v0.2.3

*Released: November 19th 2017*

- Update Agreement URL in acme-tiny to reflect the latest Let's Encrypt changes

### v0.2.2

*Released: January 23rd 2017*

- Add `sudo` when restarting web service to pick up the changes

### v0.2.1

*Released: December 22nd 2016*

- Fix "Scheme missing" wget v1.16 issue in renew script
- Fix `acme_tiny` path not existing in the renew script

### v0.2.0

*Released: November 2nd 2016*

- Add syntax test (due to not being able to mock acme-tiny easily)
- Remove unused root CA URL
- Use a more friendly cron syntax

### v0.1.0

*Released: October 27th 2016*

- Initial release
