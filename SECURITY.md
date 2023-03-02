"name": "@security-alert/share",
  "version": "1.10.9",
    "description": "security alert shared lib",
      "keywords": [
          "cli",
              "console",
                  "github",
                      "issue",
                          "security",
                              "tool"
                                ],
                                  "homepage": "https://github.com/security-alert/security-alert",
                                    "bugs": {
                                        "url": "https://github.com/security-alert/security-alert/issues"
                                          },
                                            "repository": {
                                                "type": "git",
                                                    "url": "https://github.com/security-alert/security-alert.git"
                                                      },
                                                        "license": "MIT",
                                                          "author": "azu",
                                                            "files": [
                                                                "bin/",
                                                                    "lib/",
                                                                        "src/"
                                                                          ],
                                                                            "main": "lib/index.js",
                                                                              "types": "lib/index.d.ts",
                                                                                "directories": {
                                                                                    "lib": "lib",
                                                                                        "test": "test"
                                                                                          },
                                                                                            "scripts": {
                                                                                                "build": "cross-env NODE_ENV=production tsc -p .",
                                                                                                    "clean": "rimraf lib/",
                                                                                                        "prepublish": "npm run --if-present build",
                                                                                                            "test": "mocha \"test/**/*.ts\"",
                                                                                                                "watch": "tsc -p . --watch"
                                                                                                                  },
                                                                                                                    "devDependencies": {
                                                                                                                        "@types/lodash": "^4.14.191",
                                                                                                                            "@types/meow": "^5.0.0",
                                                                                                                                "@types/mocha": "^10.0.1",
                                                                                                                                    "@types/nock": "^11.1.0",
                                                                                                                                        "@types/node": "^18.11.18",
                                                                                                                                            "cross-env": "^7.0.2",
                                                                                                                                                "mocha": "^10.2.0",
                                                                                                                                                    "nock": "^13.3.0",
                                                                                                                                                        "rimraf": "^4.0.4",
                                                                                                                                                            "ts-node": "^10.9.1",
                                                                                                                                                                "ts-node-test-register": "^10.0.0",
                                                                                                                                                                    "typescript": "^4.9.4"
                                                                                                                                                                      },
                                                                                                                                                                        "dependencies": {
                                                                                                                                                                            "@npm/types": "^1.0.2",
                                                                                                                                                                                "@octokit/graphql": "^4.5.3",
                                                                                                                                                                                    "@octokit/rest": "^18.0.3",
                                                                                                                                                                                        "@octokit/types": "^6.34.0",
                                                                                                                                                                                            "@yarnpkg/lockfile": "^1.0.0",
                                                                                                                                                                                                "meow": "^7.0.1"
                                                                                                                                                                                                  },
                                                                                                                                                                                                    "gitHead": "e39f24b9b4729ae26ab590906555d67270ae566d",
                                                                                                                                                                                                      "publishConfig": {
                                                                                                                                                                                                          "access": "public""name": "@security-alert/share",
  "version": "1.10.9",
  "description": "security alert shared lib",
  "keywords": [
    "cli",
    "console",
    "github",
    "issue",
    "security",
    "tool"
  ],
  "homepage": "https://github.com/security-alert/security-alert",
  "bugs": {
    "url": "https://github.com/security-alert/security-alert/issues"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/security-alert/security-alert.git"
  },
  "license": "MIT",
  "author": "azu",
  "files": [
    "bin/",
    "lib/",
    "src/"
  ],
  "main": "lib/index.js",
  "types": "lib/index.d.ts",
  "directories": {
    "lib": "lib",
    "test": "test"
  },
  "scripts": {
    "build": "cross-env NODE_ENV=production tsc -p .",
    "clean": "rimraf lib/",
    "prepublish": "npm run --if-present build",
    "test": "mocha \"test/**/*.ts\"",
    "watch": "tsc -p . --watch"
  },
  "devDependencies": {
    "@types/lodash": "^4.14.191",
    "@types/meow": "^5.0.0",
    "@types/mocha": "^10.0.1",
    "@types/nock": "^11.1.0",
    "@types/node": "^18.11.18",
    "cross-env": "^7.0.2",
    "mocha": "^10.2.0",
    "nock": "^13.3.0",
    "rimraf": "^4.0.4",
    "ts-node": "^10.9.1",
    "ts-node-test-register": "^10.0.0",
    "typescript": "^4.9.4"
  },
  "dependencies": {
    "@npm/types": "^1.0.2",
    "@octokit/graphql": "^4.5.3",
    "@octokit/rest": "^18.0.3",
    "@octokit/types": "^6.34.0",
    "@yarnpkg/lockfile": "^1.0.0",
    "meow": "^7.0.1"
  },
  "gitHead": "e39f24b9b4729ae26ab590906555d67270ae566d",
  "publishConfig": {
    "access": "public",')><!-- BEGIN MICROSOFT SECURITY.MD V0.0.8 BLOCK -->

## Security

Microsoft takes the security of our software products and services seriously, which includes all source code repositories managed through our GitHub organizations, which include [Microsoft](https://github.com/microsoft), [Azure](https://github.com/Azure), [DotNet](https://github.com/dotnet), [AspNet](https://github.com/aspnet), [Xamarin](https://github.com/xamarin), and [our GitHub organizations](https://opensource.microsoft.com/).

If you believe you have found a security vulnerability in any Microsoft-owned repository that meets [Microsoft's definition of a security vulnerability](https://aka.ms/opensource/security/definition), please report it to us as described below.

## Reporting Security Issues

**Please do not report security vulnerabilities through public GitHub issues.**

Instead, please report them to the Microsoft Security Response Center (MSRC) at [https://msrc.microsoft.com/create-report](https://aka.ms/opensource/security/create-report).

If you prefer to submit without logging in, send email to [secure@microsoft.com](mailto:secure@microsoft.com).  If possible, encrypt your message with our PGP key; please download it from the [Microsoft Security Response Center PGP Key page](https://aka.ms/opensource/security/pgpkey).

You should receive a response within 24 hours. If for some reason you do not, please follow up via email to ensure we received your original message. Additional information can be found at [microsoft.com/msrc](https://aka.ms/opensource/security/msrc). 

Please include the requested information listed below (as much as you can provide) to help us better understand the nature and scope of the possible issue:

  * Type of issue (e.g. buffer overflow, SQL injection, cross-site scripting, etc.)
  * Full paths of source file(s) related to the manifestation of the issue
  * The location of the affected source code (tag/branch/commit or direct URL)
  * Any special configuration required to reproduce the issue
  * Step-by-step instructions to reproduce the issue
  * Proof-of-concept or exploit code (if possible)
  * Impact of the issue, including how an attacker might exploit the issue

This information will help us triage your report more quickly.

If you are reporting for a bug bounty, more complete reports can contribute to a higher bounty award. Please visit our [Microsoft Bug Bounty Program](https://aka.ms/opensource/security/bounty) page for more details about our active programs.

## Preferred Languages

We prefer all communications to be in English.

## Policy

Microsoft follows the principle of [Coordinated Vulnerability Disclosure](https://aka.ms/opensource/security/cvd).

<!-- END MICROSOFT SECURITY.MD BLOCK -->
