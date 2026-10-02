# 🔎 GEARZ LeakFinder Fixtures

A small collection of deliberately dummy, credential-shaped strings for developing and inspecting secret-detection rules in [GEARZ LeakFinder](https://github.com/Gearsoldier/gearz-leakfinder).

**This is test data, not a credential store.** The fixture file is explicitly labeled as dummy data. Never attempt to authenticate with its values, and never replace them with real credentials.

## Repository contents

- [`secrets.txt`](secrets.txt): six labeled sample entries for AWS access/secret keys, GitHub, Google API, Stripe, and Slack token formats
- [`README.md`](README.md): fixture purpose, limitations, and safe handling
- [`.gitignore`](.gitignore): local dependency and operating-system exclusions

There is no application, package manifest, test runner, or expected-results file in this repository. It requires no dependency installation.

## How to use the fixtures

1. Inspect the sample labels and the detector rules you intend to exercise
2. Use the file as local input to your own isolated detector test harness
3. Record which rule matched, which fixture it matched, and which results you expected
4. Keep the test offline and avoid any credential-validation calls or external uploads

Cloning the repository only downloads the test data:

```bash
git clone https://github.com/Gearsoldier/gearz-leakfinder-fixtures.git
cd gearz-leakfinder-fixtures
```

This repository does not provide a command that runs the LeakFinder engine against a local file. Network-adapter testing is a separate workflow and needs explicit target authorization.

## Detection expectations

A credential-like label does not guarantee a match. Detection depends on the exact pattern, value length, context, and the rule pack under test.

The current [LeakFinder default rule pack](https://github.com/Gearsoldier/gearz-leakfinder/blob/main/engine/rules/default.yaml) defines AWS access keys, AWS secret keys, Google API keys, JWTs, and RSA private-key headers. It does not define dedicated GitHub, Stripe, or Slack rules. Conversely, this fixture file does not include dedicated JWT or RSA private-key samples.

The fixture set is therefore incomplete, and no expected hit count is asserted. A detector returning zero findings is not proof that the samples were loaded, the integration worked, or a real asset is free of secrets. Check [LeakFinder's README](https://github.com/Gearsoldier/gearz-leakfinder#readme) and implementation limitations before interpreting an end-to-end run.

## Adding fixtures

- Use unmistakably synthetic values and document the rule each sample is meant to exercise
- Include positive and negative cases so a test can check both misses and false positives
- Keep expected results alongside the test harness, without reproducing real secrets
- Do not copy tokens from logs, production systems, user accounts, or incident reports
- Do not validate sample credentials against provider APIs

Secret scanners may intentionally flag this file. Keep any test-only exclusions narrow; a fixture label is not a reason to disable scanning for unrelated files.

## Safety note

The existing samples are described by their author as inert. Their validity has not been tested against service providers, and doing so is outside the purpose of this repository. If a real credential is accidentally added, treat it as exposed and follow the provider's revocation process.
