# Analyse du Manifeste Android — DIVA (jakhar.aseem.diva)

## Métadonnées du manifeste

```xml
<manifest versionCode="1"
          versionName="1.0"
          package="jakhar.aseem.diva"
          platformBuildVersionCode="23"
          platformBuildVersionName="6.0-2166767">

  <uses-sdk minSdkVersion="15" targetSdkVersion="23" />

  <uses-permission name="android.permission.WRITE_EXTERNAL_STORAGE" />
  <uses-permission name="android.permission.READ_EXTERNAL_STORAGE" />
  <uses-permission name="android.permission.INTERNET" />
  <uses-permission name="android.permission.ACCESS_MEDIA_LOCATION" />

  <application
      theme="@2131296387"
      label="@2131099683"
      icon="@2130903040"
      debuggable="true"
      allowBackup="true"
      supportsRtl="true">

    <!-- Activité principale exportée -->
    <activity name="jakhar.aseem.diva.MainActivity">
      <intent-filter>
        <action name="android.intent.action.MAIN" />
        <category name="android.intent.category.LAUNCHER" />
      </intent-filter>
    </activity>

    <!-- Activités non exportées via intent implicite -->
    <activity label="@2131099687" name="jakhar.aseem.diva.LogActivity" />
    <activity label="@2131099692" name="jakhar.aseem.diva.HardcodeActivity" />
    <activity label="@2131099693" name="jakhar.aseem.diva.InsecureDataStorage1Activity" />
    <!-- ... autres activités -->

    <!-- Content Provider vulnérable -->
    <provider
        name="jakhar.aseem.diva.NotesProvider"
        authorities="jakhar.aseem.diva.provider.notesprovider"
        exported="true" />
        <!-- Pas de readPermission ni writePermission ! -->
  </application>
</manifest>
```

## Observations critiques

| Attribut | Valeur | Risque |
|----------|--------|--------|
| `android:debuggable` | `true` | 🔴 Critique — permet le débogage en production |
| `android:allowBackup` | `true` | 🟡 Moyen — données sauvegardables via ADB |
| `minSdkVersion` | `15` | 🟡 Moyen — Android 4.0 avec de nombreuses CVE |
| `targetSdkVersion` | `23` | 🟡 Moyen — Android 6.0, manque les protections modernes |
| Provider `readPermission` | `null` | 🔴 Critique — lecture libre des données |
| Provider `writePermission` | `null` | 🔴 Critique — écriture libre des données |
| Permissions déclarées | `WRITE/READ_EXTERNAL_STORAGE` | 🟡 Moyen — accès large au stockage |

## Points positifs

- ✅ Aucun service exporté
- ✅ Aucun broadcast receiver exporté
- ✅ La majorité des activités secondaires ne définissent pas d'intent-filter implicite
