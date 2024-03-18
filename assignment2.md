## Part 1

#### Snyk Vulnerability Report

I was able to use the snyk.sarif file to identify that the package.json file was requiring a package that contained a medium level vulnerability. The sarif file also contained reference to a pre-existing CVE [(CVE-2017-16114)](https://nvd.nist.gov/vuln/detail/CVE-2017-16114)

From there, I was able to find the original [GitHub issue page](https://github.com/markedjs/marked/issues/937) where the issue was outlined and a fix was implimented.

I learned that there have since been 12 major releases of the package so this issue was fixed long ago. However, since there have been so many changes (v0.3.5 - v12.0.1) I made the recommendation to test with the package fully up to date prior to deployment as there may be many other changes that could affect the application.

#### Semgrep Security Report

I was able to use the Semgrep.sarif file to identify that 2 html files contained links to plaintext HTTP sites. This is a security risk as it could allow the network traffic to be sniffed and the data to be intercepted. I recommended that the links be updated to use HTTPS.

## Part 2

### Arbitrary Code Execution via eval() in contributions.js

It was noted that the use of `eval()` in the contributions.js file could allow for arbitrary code execution. This is a security risk as it could allow for an attacker to execute code on the server as the data had not been sanitized.
To resolve this I replaced all uses of `eval()` with `parseInt()` as it is a safer alternative.
A subsequent test was run to ensure that the application still functioned as expected and that no other issues were introduced. This testing was completed in a fork and then a pull request was made to the original repository.

### XSS Vulnerability in ejs Template Engine

It was noted that the ejs template engine was not properly escaping user input. This could allow for an attacker to execute arbitrary code on the client side. Through [CVE-2022-24785](https://nvd.nist.gov/vuln/detail/CVE-2022-24785) It was discovered that moment.js was the vulnerabe package. As this application does not directly use moment.js it is likely required by another package. As a security measure, I have updated all packages to the latest version to ensure that the issue is resolved. I ensured no further security issues were introduced by running a test on a fork of the repository and then making a pull request to the original repository.
