# TRAM-3841 Serbia UB2 — USD allowlist soft-default evidence

## Deploy
- Workflow: https://github.com/consensys-vertical-apps/va-mmcx-onramp-api/actions/runs/31045403313
- Overall run conclusion: **failure** (SonarCloud scan failed) but **`deploy-pr-to-dev / deploy` succeeded**
- Sibling race run: https://github.com/consensys-vertical-apps/va-mmcx-onramp-api/actions/runs/31045389648 (ECR push failed — immutable tag race)
- Image tag rolled to DEV: `v1.77.1-cb1c433d` (workload commit `9ff0c56c`)
- API commit SHA: `c9becf3fca5d4cde3eef8b6ed43be3c9cd9f9446` — *soft-default countries currency via aggregator allowlist*
- What made countries currency USD: aggregator allowlist O(1) soft-default on `/v2/regions/countries` (local → USD → EUR) rather than per-country `buildRegionCache`

## DEV API curl (post-rollout ~20:55 UTC)
| code | currency |
|------|----------|
| RS | USD |
| BA | USD |
| AL | USD |
| UA | USD |
| AM | USD |
| FR | EUR |
| US | USD |

- `/v2/regions/rs/providers?sdk=2.1.6&context=mobile-ios` → **6** providers
- Optional `/v2/quotes` with fiat=rsd via ad-hoc curl still 500 (param shape); UI quotes succeeded with soft-defaulted USD

## Simulator
- Device: Dev1 `20F80808-12DA-45F2-8FE8-7B9D152CABBF` (iOS)
- metamask-mobile + Metro `METAMASK_ENVIRONMENT=dev` → `https://on-ramp.dev-api.cx.metamask.io`
- Region: CDP `eval-async` `Engine.context.RampsController.setUserRegion('rs')`
- Controller `userRegion.country.currency` = **USD** (was RSD pre-deploy)
- BuildQuote OCR: **`$100`** (prior evidence was **`RSD 100`**)
- Providers/quotes sheet: Moonpay / Ramp Network / Transak staging quotes non-empty

## Screenshots
- `screenshots/02-buildquote-serbia-usd.png` — amount input USD (`$100`)
- `screenshots/03-payment-methods.png` — Apple Pay + Debit/Credit
- `screenshots/04b-providers-quotes.png` — non-empty provider quotes
