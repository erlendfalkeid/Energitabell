# Energitabell
include shared-gdrive(
"dcic-2021",
"1wyQZj_L0qqV9Ekgr9au6RX2iqt2Ga8Ep")
include gdrive-sheets
include data-source
ssid = "1RYN0i4Zx_UETVuYacgaGfnFcv4l9zd9toQTTdkQkj7g"
tabell=
load-table:komponent, energi
source: load-spreadsheet(ssid).sheet-by-name("kWh", true)
sanitize energi using string-sanitizer
end
fun energi-to-number(str :: String) -> Number:
cases(Option) string-to-number(str):
    |some(a) => a
    |none => 20
end
where:
energi-to-number("") is 20
energi-to-number("48")is 48
end
tabell
tabellMedNummer = transform-column(tabell, "energi", energi-to-number)
tabellMedNummer
distance-travelled-per-day = 13
distance-per-unit-of-fuel= 8
energy-per-unit-of-fuel=10
energy-per-day = ( distance-travelled-per-day /
distance-per-unit-of-fuel) *
energy-per-unit-of-fuel
sum(tabellMedNummer, "energi")
bar-chart(tabellMedNummer, "komponent", "energi")
