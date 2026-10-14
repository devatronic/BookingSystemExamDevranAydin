# Felsökningsrapport - överlappande bokningar

## Felet och hur jag återskapade felet
Felet var att två bokningar kunde överlappas i samma rum tid och datum. Det som fanns redan var att Aspen var bokat den 17 oktober 2026 kl. 09:00-12:00. Genom att återskapa felet så gjorde jag en till bokning med samma rum, tid och datum men kl. 08:00-10:00.

## Förväntat och faktiskt resultat
Det som jag inte förväntade mig var att programmet skapade bokningen fast överlappning. jag förväntade mig ett felmeddelande och bokningen skulle inte ha sparats. bokningen krockade mellan 09:00 & 10:00

## Orsaken i koden

## Min rättning

## Kontroll och automatiserade tester

## Alternativ lösning och motivering