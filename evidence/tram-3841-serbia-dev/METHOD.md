# TRAM-3841 Serbia DEV simulator evidence — method

## DEV API
- Mobile `.js.env` / Metro: `METAMASK_ENVIRONMENT=dev`
- Maps via `getRampsEnvironment()` → `RampsEnvironment.Development` → `https://on-ramp.dev-api.cx.metamask.io`
- Confirmed DEV curl: `/v2/regions/rs/providers` returns **6** providers

## Serbia region
- Set with CDP `eval-async`: `Engine.context.RampsController.setUserRegion('rs')`
- Same controller API as Settings country picker (`useRampsUserRegion` / `setUserRegion`)
- Resulting state: `regionCode=rs`, country Serbia, currency RSD, iso RS

## Buy flow
- Unlock fixture wallet → RampTokenSelection → ETH → RampAmountInput (BuildQuote)
- Payment pill present; payment sheet + provider picker show non-empty options
- Controller `providers.data` length **6** (staging provider ids)

## Device
- Dev1 UDID `20F80808-12DA-45F2-8FE8-7B9D152CABBF` (iOS 26.2)
- metamask-mobile `main` + mm-harness 0.25.1 / CDP bridge
