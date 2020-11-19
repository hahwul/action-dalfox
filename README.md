# Action-dalfox
XSS scanning with Dalfox on Github-action

![Docker build test](https://github.com/hahwul/action-dalfox/workflows/Docker%20build%20test/badge.svg)

## Getting Started
### Usage
```yaml
- name: Dalfox scan
  uses: hahwul/dalfox-action@master
  id: xss-result
  with:
    target: 'https://www.hahwul.com'
    mode: url
    cmd_options: '--follow-redirects'
```
- Modes: `url` `pipe` `file` `sxss`
- Options: https://github.com/hahwul/dalfox#usage

### Output Handling
Send slack/github issue/Submit JIRA, etc.. with found-action option
```yaml
- name: Dalfox scan
  uses: hahwul/dalfox-action@master
  id: xss-result
  with:
    target: 'https://www.hahwul.com'
    mode: url
    cmd_options: '--found-action "curl -i -k"https://hooks.your.system"'
  - run: echo "XSS result - ${{ steps.xss-result.outputs.result }}"
```
- Found-Action: https://github.com/hahwul/dalfox/wiki/Found-action

## Sample
### XSS Scanning
xss.yaml
```yaml
on: [push]

jobs:
  dalfox_scan:
    runs-on: ubuntu-latest
    name: XSS Scanning
    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          ref: master
      - name: Dalfox scan
        uses: hahwul/dalfox-action@master
        id: xss-result
        with:
          target: 'https://xss-game.appspot.com/level1/frame'
          mode: url
          cmd_options: '--follow-redirects'
```
