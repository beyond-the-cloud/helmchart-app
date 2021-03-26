# helmchart-app

Repository for Application Dependencies Helm Charts

Install the helm:
`helm install --set mysql.dbHost=dbHost --set mysql.dbPassword=dbPassword {{ .Release.Name }} ./{{ .Chart.Name }}`

Uninstall the helm:
`helm uninstall {{ .Release.Name }}`

Check NOTES.txt for more details.

## Github Release Setup

We are using [release-it](https://github.com/release-it/release-it) to help bootstrap the release process.

Jenkins Job Setup:

```bash
git remote rm origin
git remote add origin https://beyond-the-cloud:${GITHUB_TOKEN}@github.com/beyond-the-cloud/helmchart-app.git
git symbolic-ref HEAD refs/heads/master

export VERSION=$(git describe --abbrev=0 --tags)
echo $VERSION

export a=$(echo "$VERSION" | cut -d. -f1)
export b=$(echo "$VERSION" | cut -d. -f2)
export c=$(echo "$VERSION" | cut -d. -f3)
export NEW_VERSION="${a}.${b}.$(($c + 1))"
echo $NEW_VERSION

sed -i -e "s/^version:.*/version: ${NEW_VERSION}/g" webapp/Chart.yaml

zip -r webapp-v${NEW_VERSION}.zip webapp

git reset --hard HEAD

npx release-it patch --ci --no-git.requireUpstream --no-git.requireCleanWorkingDir --github.release --github.releaseName="webapp-v${NEW_VERSION}" --github.assets=webapp-v${NEW_VERSION}.zip
```
