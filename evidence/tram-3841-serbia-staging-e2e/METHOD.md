# TRAM-3841 Serbia Transak Staging E2E — method & result

## Verdict
**FAIL** — staging purchase did **not** complete. No `provider_order_id`.

Farthest progress: Serbia → USD BuildQuote → Transak Staging checkout → cold auth email + OTP success → Transak **MiCA region block** ("not onboarding new users in your region").

## Environment
- Device: Dev1 `20F80808-12DA-45F2-8FE8-7B9D152CABBF` (iOS)
- metamask-mobile + Metro `METAMASK_ENVIRONMENT=dev` → `https://on-ramp.dev-api.cx.metamask.io`
- Region: CDP `eval-async` `Engine.context.RampsController.setUserRegion('rs')`
- Controller `userRegion.country.currency` = **USD** (Serbia / RS)
- Flow: Homepage Buy → ETH → BuildQuote → $30 Debit → Continue → Transak Staging WebView
- Staging email: `amitabh.aggarwal@consensys.net`

## What worked
1. Unlock fixture wallet on Dev1
2. `setUserRegion('rs')` → currency **USD**
3. BuildQuote showed **`$100`** (USD) + **Powered by Transak (Staging)**
4. Entered amount / Debit or Credit → Continue
5. Checkout params: `currency=USD`, `providerCode=transak-staging`, `providerName=Transak (Staging)`
6. Cold auth: emailed OTP; Gmail OTP `443116` entered successfully

## Blocker
After OTP validation, Transak showed wallet-select overlay + modal:

> Thanks for your interest in Transak! Heads up, we're not onboarding new users in your region just yet as we are in the final stages of obtaining our MiCA license…

Screenshot: `screenshots/04b-after-otp-load.png`

This is a **Transak regional policy / staging onboarding** block for Serbia (country `rs`), not a MetaMask BuildQuote currency failure. Soft-default USD on `/countries` + BuildQuote `$` is confirmed; end-to-end buy cannot finish while Transak rejects the region.

## Artifacts
- Screenshots under `screenshots/` (BuildQuote USD, auth, OTP, MiCA Oops)
- GIF highlights: `recordings/serbia-staging-buy-highlights.gif`
- Raw sim recording incomplete (simctl left unfinalized `.sb` without moov atom) — use GIF + stills

## provider_order_id
none
