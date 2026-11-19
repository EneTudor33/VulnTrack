# VulnTrack – Platforma cloud pentru gestionarea si monitorizarea vulnerabilitatilor de securitate

## Descrierea proiectului

**VulnTrack** este o aplicatie web bazata pe cloud, destinata inregistrarii, organizarii si monitorizarii vulnerabilitatilor de securitate identificate in cadrul unor evaluari de securitate autorizate. In loc sa implementeze un scanner complex de vulnerabilitati, proiectul se concentreaza pe un proces practic: utilizatorii pot raporta vulnerabilitati, le pot atribui niveluri de severitate, le pot urmari starea si pot consulta istoricul modificarilor.

Scopul principal este dezvoltarea unei aplicatii functionale si securizate, demonstrand utilizarea principalelor servicii **Microsoft Azure**, automatizarea deploymentului in cloud, autentificarea, autorizarea si procesarea asincrona a mesajelor.

## Functionalitati principale

- **Autentificare:** utilizatorii trebuie sa se autentifice inainte de a accesa aplicatia.
- **Rapoarte de vulnerabilitate:** crearea, vizualizarea, modificarea si stergerea rapoartelor, care includ titlu, descriere, sistem afectat, severitate (Low / Medium / High / Critical) si stare (Open / In Progress / Resolved).
- **Dashboard:** afisarea vulnerabilitatilor raportate si filtrarea lor dupa severitate sau stare.
- **Autorizare:** utilizatorii obisnuiti isi gestioneaza propriile rapoarte, in timp ce administratorii pot vizualiza si gestiona toate rapoartele.
- **Istoricul modificarilor:** inregistrarea modificarilor relevante, inclusiv cine a actualizat o vulnerabilitate si cand.
- **Notificari asincrone:** atunci cand este raportata o vulnerabilitate cu severitate High sau Critical, un mesaj dintr-o coada declanseaza o functie care inregistreaza o notificare pentru administrator.

## Tehnologii propuse si arhitectura Azure

| Componenta | Tehnologie / serviciu Azure | Rol |
|---|---|---|
| Frontend | HTML, CSS, JavaScript (Azure Static Web Apps) | Interfata web si dashboard |
| Backend / compute | Python (Azure Functions) | Endpointuri API, logica aplicatiei si verificarea drepturilor de acces |
| Baza de date | Azure SQL Database | Stocarea rapoartelor, profilurilor utilizatorilor si istoricului modificarilor |
| Autentificare | Microsoft Entra ID prin integrarea de autentificare a aplicatiei web | Autentificarea si identificarea utilizatorilor |
| Coada de mesaje | Azure Queue Storage | Gestionarea evenimentelor asociate vulnerabilitatilor importante |
| Procesare in fundal | Azure Function declansata de mesaje din coada | Procesarea evenimentelor si crearea notificarilor in aplicatie |
| Deployment infrastructura | Azure CLI + Bicep | Crearea si stergerea reproductibila a resurselor cloud |

**Flux simplificat:** Browser -> frontend autentificat -> API Python protejat -> Azure SQL Database. Rapoartele High/Critical genereaza si un mesaj in Azure Queue Storage -> functie declansata de coada -> inregistrarea unei notificari.

## Aspecte de securitate

- Protejarea endpointurilor API si verificarea identitatii si rolului utilizatorului in backend, fara a ne baza exclusiv pe restrictiile din frontend.
- Folosirea HTTPS, validarea datelor introduse si a interogarilor SQL parametrizate pentru prevenirea SQL Injection.
- Neexpunerea credentialelor bazei de date in browser; stocarea secretelor in configuratii Azure securizate si utilizarea identitatilor gestionate acolo unde sunt disponibile.
- Restrictionarea accesului de retea la baza de date si aplicarea principiului privilegiilor minime.
- Testarea faptului ca un utilizator nu poate vizualiza sau modifica rapoartele altui utilizator.

## Plan de implementare si delimitarea proiectului

1. **Prototip local:** implementarea structurii bazei de date, a API-ului Python, a interfetei, a integrarii de autentificare (sau a unei autentificari locale pentru teste) si a operatiilor de baza asupra rapoartelor.
2. **Integrare cloud:** publicarea frontendului, API-ului si bazei de date in Azure; configurarea identitatilor, permisiunilor si a cozii de mesaje.
3. **Automatizare si testare:** realizarea scripturilor pentru deployment si stergerea resurselor; testarea functionalitatilor principale si a regulilor de autorizare.
4. **Livrare finala:** codul sursa, un README scurt, o diagrama de arhitectura si o prezentare de 8–10 slide-uri cu demonstratie functionala.

**Delimitare:** VulnTrack gestioneaza vulnerabilitati introduse de utilizatori. Scanarea automata a porturilor, exploatarea activa si detectarea avansata a vulnerabilitatilor **nu** fac parte din implementarea initiala. Astfel, proiectul ramane realizabil si acopera cerintele principale ale cursului de cloud computing.

**Rezultat asteptat:** o aplicatie de cybersecurity mica, dar completa, care demonstreaza o arhitectura cloud securizata, utilizarea unei baze de date persistente, autentificarea si autorizarea, procesarea prin cozi de mesaje si deploymentul repetabil in Azure.
