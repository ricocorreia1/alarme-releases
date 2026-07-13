# alarme-releases

Manifest e APKs do app **Alarme** (código-fonte em [ricocorreia1/alarme_app](https://github.com/ricocorreia1/alarme_app)).

O próprio app consulta `manifest.json` na branch `main` a cada 15 minutos (com cache-bust) e ao voltar do background. Quando o `versionCode` publicado aqui é maior que o instalado, aparece um prompt **"Atualização disponível"** pro usuário. Aceito → baixa o APK dessa release → instala via `PackageInstaller.Session` nativo (sem pedir "abrir com...").

## Estrutura

- `manifest.json` — apontador da última versão. Formato:
  ```json
  {
    "versionCode": 22,
    "versionName": "0.0.22",
    "url": "https://github.com/ricocorreia1/alarme-releases/releases/download/v0.0.22/app-release.apk",
    "notes": "Uma linha resumindo a mudança."
  }
  ```
- `releases/` — cada versão é uma GitHub Release com tag `v<versionName>` e o `app-release.apk` como asset.

## Publicar uma nova versão

```bash
# 1. Bumpar version em pubspec.yaml no repo alarme_app (0.0.X → 0.0.X+1) e:
flutter build apk --release --obfuscate --split-debug-info=build/symbols

# 2. Aqui neste repo, editar manifest.json (versionCode, versionName, url, notes).
git commit -am "vX.Y.Z: <resumo>"
git push

# 3. Criar release e anexar APK (o path do APK vem do repo alarme_app):
gh release create v0.0.X ../alarme_app/build/app/outputs/flutter-apk/app-release.apk \
  --repo ricocorreia1/alarme-releases \
  --title "v0.0.X" \
  --notes "<resumo>"
```

O padrão de URL do APK é sempre `releases/download/v<versionName>/app-release.apk` — assim o `manifest.json` fica previsível e o app não precisa parsear a API do GitHub.

## Padrão portado do DroidTV

Mesmo esquema usado no meu app [DroidTV](https://github.com/ricocorreia1/droidtv-releases): manifest em raw.githubusercontent, install via `PackageInstaller.Session` em Kotlin (evita a caixa "abrir com" instável em algumas ROMs), receiver dedicado tratando `STATUS_PENDING_USER_ACTION`.
