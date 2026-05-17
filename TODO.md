Što je ispravnije? Ispravnije je imati public folder. Iako će Rješenje 1 (gdje smo natjerali server da gleda u korijen) raditi bez ikakvih problema za tvoj lokalni portfolio, odvajanje koda u public folder je industrijski standard koji se koristi u profesionalnim PHP okvirima (poput Laravel-a ili Symfony-a), ali i u Node.js/React svijetu.
Čemu zapravo služi taj public folder?

Glavni i najvažniji razlog zašto on postoji je sigurnost.

Kada web server (bio to Caddy, Apache ili Nginx) uperiš u neki folder, sve što se nalazi unutar tog foldera postaje javno dostupno cijelom internetu ako netko pogodi točnu putanju u URL-u.

Pogledaj ponovno sliku strukture tvog v2 projekta:

    Imaš mapu config u kojoj se vjerojatno nalaze lozinke za bazu podataka, API ključevi ili sistemske postavke.

    Imaš mapu includes ili templates s PHP datotekama koje služe samo kao dijelovi koda (npr. spajanje na bazu, provjera sesije) i ne bi se trebale izvršavati same za sebe.

Što se događa ako server gleda u korijen (/app)?

Ako netko u preglednik utipka http://localhost:8083/config/config.php, poslužitelj će pokušati otvoriti tu datoteku. Ako ti server u nekom trenutku krivo konfiguriran, umjesto izvršavanja PHP-a, on bi mogao ispisati čisti tekst te datoteke i napadač bi vidio tvoje lozinke.
Kako public folder to rješava?

U profesionalnom setupu, ti napraviš folder public i u njega staviš samo index.php, te javne stvari poput slika, CSS-a i JS-a. Sve ostalo (config, includes, ostatak PHP koda) ostavljaš izvan tog public foldera.

Budući da je web server uperen isključivo u public, napadač koji tipka po URL-u fizički ne može "izaći" iz tog foldera i dohvatiti tvoje konfiguracijske datoteke koje se nalaze razinu iznad. One su potpuno sigurne na serveru, a tvoj index.php ih i dalje može normalno povlačiti u pozadini preko require ili include naredbi.
Kako bi to izgledalo na tvom v2 projektu ako želiš napraviti "po propisu"?

Ako imaš vremena i želiš da ti projekt bude maksimalno "clean" za portfolio:

    Unutar soundwave/v2 napravi novi folder pod nazivom public.

    U njega prebaci: index.php, favicon.ico, te mape css, js, img, fonts.

    Sve ostale mape (config, includes, templates, admin) ostavi vani, u korijenu v2 foldera.

    U docker-compose.yml makneš onu FRANKENPHP_CONFIG varijablu i vratiš volumen na zadano:
    YAML

    volumes:
      - ./soundwave/v2:/app

    (FrankenPHP će sam ući u /app/public i pokrenuti stvar).

Mala napomena: Ako ovo napraviš, morat ćeš u index.php popraviti putanje do includes mapa (npr. umjesto include 'config/config.php' morat ćeš pisati include '../config/config.php' jer je index sada u podfolderu).