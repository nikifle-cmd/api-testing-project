# api-testing-project # Testovací projekt API – Postman 

## Popis projektu Tento projekt slouží k testování REST API pomocí Postmanu. Cílem je ověřovat funkčnost endpointů a kontrolovat datové typy v response. Testy jsou zaměřeny na **Petstore API**, což je veřejné testovací API pro mazlíčky. 

**Odkaz na Swagger dokumentaci:** [Petstore Swagger](https://petstore.swagger.io/) --- 

## Testovací scénář – životní cyklus mazlíčka

1. Vytvoření mazlíčka (POST)
2. Ověření vytvoření (GET)
3. Aktualizace údajů (PUT)
4. Ověření změny (GET)
5. Smazání mazlíčka (DELETE)
# Tento scénář simuluje reálné použití API a ověřuje konzistenci dat v průběhu operací.

## Testovaný endpoint 

### GET /pet/{petId} - Účel: Získat informace o konkrétním mazlíčkovi podle jeho ID - Test: Kontrola datových typů hlavních atributů v response 

#### Postman Test (příklad)
javascript
var response = pm.response.json();

pm.test("id je číslo", function() {
    pm.expect(typeof response.id).to.eql("number");
});

pm.test("name je string", function() {
    pm.expect(typeof response.name).to.eql("string");
});

pm.test("status je string", function() {
    pm.expect(typeof response.status).to.eql("string");
});

if (response.category) {
    pm.test("category.id je číslo", function() {
        pm.expect(typeof response.category.id).to.eql("number");
    });
    pm.test("category.name je string", function() {
        pm.expect(typeof response.category.name).to.eql("string");
    });
}

if (response.photoUrls) {
    pm.test("photoUrls je pole", function() {
        pm.expect(Array.isArray(response.photoUrls)).to.eql(true);
    });
}

if (response.tags) {
    pm.test("tags je pole", function() {
        pm.expect(Array.isArray(response.tags)).to.eql(true);
    });
}


### GET /pet/findByStatus?status=sold

- Účel: Získat informace všech vyprodaných mazlíků
- Test: Kontrola datových typů hlavních atributů v response

var response = pm.response.json();

pm.test("Response je pole", function() {
    pm.expect(Array.isArray(response)).to.eql(true);
});

response.forEach(function(pet) {
    pm.test("Pet status je sold", function() {
        pm.expect(pet.status).to.eql("sold");
    });
});

### GET /pet/findByStatus?status=sold&name="doggggie"

- Účel: Ověřit přítomnost konkrétního mazlíčka ve výpisu
- Test: Negativní test, API nepodporuje parametr name. Filtrace probíhá lokálně.

var pets = pm.response.json();

var filtered = pets.filter(pet => pet.name === "doggggie");

pm.test("Existuje mazlíček mezi sold", function() {
    pm.expect(filtered.length).to.be.above(0);
});
Poznámka: Endpoint /pet/findByStatus nepodporuje filtraci podle jména. Test slouží k ověření přítomnosti konkrétního mazlíčka ve vyprodaných.

### DELETE /pet/11

- Účel: Smazat konkrétního mazlíčka podle ID
- Test: Kontrola, že mazlíček byl úspěšně smazán (HTTP status 200)

pm.test("Mazlíček byl smazán (status 200)", function() {
    pm.response.to.have.status(200);
});
Poznámka: Při opakovaném spuštění DELETE pro stejné ID může API vrátit chybu, protože mazlíček již neexistuje.
