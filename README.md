# alarme-releases

Manifest e APKs do app **Alarme** (código-fonte em [ricocorreia1/alarme_app](https://github.com/ricocorreia1/alarme_app)).

O próprio app consulta `manifest.json` na branch `main` a cada 15 minutos; quando o `versionCode` é maior que o instalado, oferece a atualização.

## Publicar uma nova versão

1. Buildar `flutter build apk --release` no repo do app.
2. Criar uma release aqui com tag `v<versionName>` e anexar o `app-release.apk`.
3. Atualizar `manifest.json` na `main` com o novo `versionCode`, `versionName`, `url` e `notes`.

A URL do APK segue sempre o padrão `releases/download/v<versionName>/app-release.apk`.
