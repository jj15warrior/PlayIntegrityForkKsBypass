## Custom Fork v9
- Improve migrate script handling of some formatting edge cases
- Add migrate script manual option to override all values using the fingerprint values
- Add an opt-in scripts-only-mode for Android <10 ROMs
- Add autopif script to allow extracting the latest Xiaomi.eu custom.pif.json values
- Add killgms script to allow manual DroidGuard process killing

## Custom Fork v8
- Rename VERBOSE_LOGS to verboseLogs to better differentiate Advanced Settings from Build Fields or System Properties
- Improve replace list to also allow file paths replacing/hiding systemlessly
- Improve migration script and add optional verboseLogs entry to output
- Improve replace list to automatically comment out any overlay APK config.xml entries systemlessly
- Update default/example app replace list for more ROM spoof injection methods
- Fix retaining disabled ROM apps through module updates in some scenarios

_[Previous changelogs](https://github.com/osm0sis/PlayIntegrityFork/releases)_
