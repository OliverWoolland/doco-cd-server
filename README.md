# doco-cd-may-be
A quick test of doco-cd for automatically deploying github repos 

While generally `.doco-cd.yml` and the doco-cd deployment compose file would live 
in different repos, they are bundled here to make everything needed obvious.

## Setup

Run these commands to setup doco-cd. 

> [!TIP]
> If you want to quickly try out a development grade setup, skip to `Development setup```.

First populate the environment, using the Github web UI to create tokens, secrets and a webhook.

```bash
cp env.template .env
nano .env
```
Then put the doco-cd service up

```bash
docker compose -f doco-compose.yaml up -d
docker compose -f doco-compose.yaml logs  -f
```

## Development setup 

> [!WARNING]
> Read these commands carefully and understand what they do - they will not be useful or desirable for everyone.
>
> These commands give you a quick start for trying out doco-cd, they are not how you would work in production.

First install the Github cli tool (if you don't have it already). 

A guide for specific distros can be found [here]([url](https://github.com/cli/cli/blob/trunk/docs/install_linux.md), or the following general method applied

```bash
curl -sS https://webi.sh/gh \| sh
```
Next, obtain a Github acccess token, easiest with the `gh` tool.

```bash
gh auth
```
Add the token to the config file

```bash
echo GIT_ACCESS_TOKEN=$(gh auth token) > .env
```
Next, setup a *development* webhook

```bash
gh extension install cli/gh-webhook
WEBHOOK_SECRET=$(openssl rand -hex 32)
echo WEBHOOK_SECRET=$WEBHOOK_SECRET >> .env 
gh webhook forward --repo=OliverWoolland/doco-cd-may-be --events="push" --url="http://localhost:8543 --secret=$WEBHOOK_SECRET
```

You are now ready to try out doco-cd locally.
