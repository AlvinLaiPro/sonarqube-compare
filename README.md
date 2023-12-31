# sonarqube-compare
With sonarqube-compare, you can compare your new branch against the target branch's coverage and issues in detail.

**Note: `sonarqube-compare` doesn't generate test coverage reports; it uses the sonarqube server API to obtain them. So it needs to wait until your branches have the coverage reports in the sonarqube server.**

## Installation

sonarqube-compare runs on Node.js and is available as a NPM package.

```bash
npm install sonarqube-compare
```
or 
```
npm install sonarqube-compare -g
```

## Usage

To generate the comparison of your code coverage between two branchs, you need to provide these parameters for the `sonarqubeCompare` function: `sourceBranch`, `targetBranch`, `component`, `token`, `host` and `metrics`. The `metrics` parameter is optional, it would be set to `['uncovered_lines', 'uncovered_conditions']` if you don't provide it.

```js
const {sonarqubeCompare} = require('sonarqube-compare');

sonarqubeCompare({
    sourceBranch: 'develop',
    targetBranch: 'master',
    metrics: ['uncovered_lines', 'uncovered_conditions'],
    component: 'your_project_key',
    token: 'token',
    host: 'https://sonarqube.example.com',
}).then(s => console.log(s));
```
Command line support is also available. For command line use, we recommend installing it globally.

```bash
sonarqube-compare -s develop -t master -c your_project_key -k token -h https://sonarqube.example.com -m uncovered_lines uncovered_conditions
```
## Output
```
╔═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════╗
║                                                              Test Coverage Report                                                               ║
╟──────────────────────┬───────┬────────────────────────────────────────────────────┬────────────┬─────────────────┬──────────────────────────────╢
║         Key          │ Line  │ Code                                               │  New code  │ Uncovered Lines │ Uncovered Conditions (Fully) ║
╟──────────────────────┼───────┼────────────────────────────────────────────────────┼────────────┼─────────────────┼──────────────────────────────╢
║                      │  362  │ if (NotCoveredThisConditionFully) {                │   false    │                 │             true             ║
║                      ├───────┼────────────────────────────────────────────────────┼────────────┼─────────────────┼──────────────────────────────╢
║ your_project_key:src │  366  │ if (NotCoveredThisLine == true) {                  │   false    │      true       │                              ║
║ /common/fileName.ts  ├───────┼────────────────────────────────────────────────────┼────────────┼─────────────────┼──────────────────────────────╢
║                      │  371  │ return defaultMessage;                             │   false    │      true       │                              ║
╚══════════════════════╧═══════╧════════════════════════════════════════════════════╧════════════╧═════════════════╧══════════════════════════════╝
```
