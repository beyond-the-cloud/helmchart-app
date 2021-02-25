# helmchart-app
Repository for Application Dependencies Helm Charts

Install the helm:
`helm install --set dbHost=dbHost --set dbPassword=dbPassword {{ .Release.Name }} ./{{ .Chart.Name }}`

Uninstall the helm:
`helm uninstall {{ .Release.Name }} ./{{ .Chart.Name }}`

Check NOTES.txt for details.