# Felsökningsrapport - överlappande bokningar

## Felet och hur jag återskapade felet
Felet var att två bokningar kunde överlappas i samma rum och datum. Det som fanns redan var att Aspen var bokat den 17 oktober 2026 kl. 09:00-12:00. Genom att återskapa felet så gjorde jag en till bokning med samma rum, tid och datum men kl. 08:00-10:00.

## Förväntat och faktiskt resultat
Det som jag inte förväntade mig var att programmet skapade bokningen fast överlappning. jag förväntade mig ett felmeddelande och bokningen skulle inte ha sparats. bokningen krockade mellan 09:00 & 10:00

## Orsaken i koden
Rotorsaken till felet var att Validate i BookingService.cs där hasOverlap beräknas som har användts för att avgöra om en bokning överlappar en annan, ursprungskoden bara undersökte den nya starttiden. när jag bokade 08:00-10:00 så fanns 09:00-12:00 och kontrollen blev falsk, och i med att 08:00 ligger före 09:00 och det är därför bokningen godkändes fast tiderna överlappade mellan 09:00 och 10:00.

## Min rättning
Det jag ändrade var tidsvillkoret som beräknar hasOverlap. Nya kontrollen kollar om den nya bokningen har börjat före om en boknings endtime slutar efter dens starttime. Jag använde < > för att fortfarande tillåta en bokning att börja precis när en annan bokning har slutat
såhär blev det: 
booking.StartTime < existing.EndTime &&
booking.EndTime > existing.StartTime

## Kontroll och automatiserade tester
Det jag gjorde manuellt var att testa att skapa överlappande bokningar. Efter jag rättade felen så nekades bokningen och jag fick ett felmeddelande. Jag körde även testerna och alla blev gröna. Testet Create_WhenBookingOverlapsExistingBooking_ReturnsFailure kontrollerar ju flera typer av överlappningar och hjälper till och hittar när det uppstår ett fel. Testet Create_WhenBookingStartsWhenExistingBookingEnds_ReturnsSuccess kollar att en bokning fortfarande får tillstånd att börja precis när en annan slutar, detta test hjälper för att om ett fel uppstår så kommer den att upptäcka det.

## Alternativ lösning och motivering
Ett annat sätt att lösa problemet är att kontrollera när en bokning inte överlappar,Bokningarna överlappa inte om en ny bokning slutar senast när en gammal börjar, eller börjra tidigast när den gamla slutar. Jag bestämde mig att granska överlappningen genast då villkoret blir kort och passar betydelsen av hasOverlap
