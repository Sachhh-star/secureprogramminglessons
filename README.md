# Educatieve Webapplicatie voor Webbeveiliging

Deze repository bevat de broncode voor een educatieve webapplicatie die is ontworpen om studenten te leren over webbeveiligingskwetsbaarheden en beste praktijken voor veilig programmeren. De applicatie simuleert een bankwebsite met verschillende opzettelijke beveiligingsfouten, wat een praktische leerervaring biedt voor onderwerpen zoals SQL-injectie, cross-site scripting (XSS), datavalidatie, cryptografie, gebroken toegangscontrole en meer. Studenten zullen deze kwetsbaarheden onderzoeken, misbruiken en beveiligen als onderdeel van hun cursuswerk.


## Installatie van Docker:

1. Zorg ervoor dat Docker is geïnstalleerd op je systeem.
2. Download of clone de Applicatie vanuit GitHub.
3. Navigeer naar de Projectmap:
4. Open een terminal of command prompt.
5. Navigeer naar de map waar je de applicatie hebt opgeslagen (bijvoorbeeld `cd pad_naar_project`).
6. Start Docker Compose:
7. Voer het commando `docker-compose up` uit. Dit bouwt en start de containers die zijn gedefinieerd in je `docker-compose.yml` bestand.

## Toegang tot de Applicatie:

1. Open een webbrowser en ga naar [http://localhost:8000](http://localhost:8000) om de applicatie te bekijken.
2. Om phpMyAdmin te gebruiken, ga naar [http://localhost:8080](http://localhost:8080).

## Wat hebben we verbeterd aan deze opdracht? (7 Beveiligingsfases)

### 1. SQL-injecties (`index.php`)
* **Oplossing:** We hebben de inlogpagina beveiligd door gebruik te maken van **Prepared Statements** met PDO (`$pdo->prepare()`) in plaats van directe gebruikersinvoer in de SQL-query string te plaatsen. Dit voorkomt dat aanvallers kwaadaardige SQL-code kunnen uitvoeren.

### 2. Broken Access Control (`transacties.php`)
* **Oplossing:** We hebben een autorisatiecontrole toegevoegd op basis van de sessie. Gebruikers kunnen nu alleen nog hun eigen transacties inzien (of alle transacties als ze een Admin-account hebben). Ongeautoriseerde pogingen worden omgeleid naar `dashboard.php`.

### 3. Gevoelige data die zichtbaar is / Sensitive Data Exposure (`transacties.php` / database)
* **Oplossing:** Gevoelige gebruikersgegevens (zoals wachtwoorden) worden niet langer in leesbare platte tekst in de database opgeslagen. Daarnaast zijn de parameters in de URL beveiligd via autorisatiecontroles, zodat transactie-ID's van andere gebruikers niet zomaar ingezien kunnen worden.

### 4. Data validatie / Controle op invoer (`dashboard.php`)
* **Oplossing:** We hebben invoervalidatie toegevoegd bij het overmaken van geld. Er wordt gecontroleerd of het bedrag numeriek is en groter is dan 0 (`$bedrag <= 0`), waardoor het niet langer mogelijk is om negatieve bedragen over te maken en andermans saldo te stelen.

### 5. Cryptografie / Versleuteling (`register.php`)
* **Oplossing:** Nieuwe gebruikerswachtwoorden worden nu opgeslagen als een sterke eenrichtingshash met behulp van `password_hash($password, PASSWORD_DEFAULT)`. Dit zorgt tevens voor automatische en veilige salting.

### 6. Cross-site scripting / XSS (`transacties.php` & `includes/header.php`)
* **Oplossing:** Gebruikersinvoer en gegevens uit de database (zoals de transactie-omschrijving en gebruikersnamen) worden nu veilig ontsmet met `htmlspecialchars()` voordat ze in de HTML-code worden weergegeven. Hierdoor kunnen kwaadaardige JavaScript-tags (zoals `<script>`) niet meer in de browser van de gebruiker worden uitgevoerd.

### 7. Problemen met inloggen en wachtwoorden / Identification & Authentication (`index.php`)
* **Oplossing:** We hebben een veilige wachtwoordverificatie met `password_verify()` geïmplementeerd. Daarnaast hebben we **transparante wachtwoordmigratie** toegevoegd in `index.php`. Zodra een bestaande gebruiker (met een plaintext wachtwoord uit `userTable.php`) succesvol inlogt, wordt zijn wachtwoord automatisch geüpgraded naar een veilige hash in de database.

