# api-testing-project
# Testovací projekt API – Postman

## Popis projektu
Tento projekt slouží k testování REST API pomocí Postmanu.  
Cílem je ověřovat funkčnost endpointů a kontrolovat datové typy v response.  

Testy jsou zaměřeny na **Petstore API**, což je veřejné testovací API pro mazlíčky.

**Odkaz na Swagger dokumentaci:**  
[Petstore Swagger](https://petstore.swagger.io/)

---

## Testovaný endpoint

### GET /pet/{petId}

- Účel: Získat informace o konkrétním mazlíčkovi podle jeho ID
- Test: Kontrola datových typů hlavních atributů v response

#### Postman Test (příklad)

```javascript
var response = pm.response.json();

// Test typů hlavních atributů
pm.test("id je číslo", function() {
    pm.expect(typeof response.id).to.eql("number");
});

pm.test("name je string", function() {
    pm.expect(typeof response.name).to.eql("string");
});

pm.test("status je string", function() {
    pm.expect(typeof response.status).to.eql("string");
});

// Kategorie, pokud existuje
if (response.category) {
    pm.test("category.id je číslo", function() {
        pm.expect(typeof response.category.id).to.eql("number");
    });
    pm.test("category.name je string", function() {
        pm.expect(typeof response.category.name).to.eql("string");
    });
}

// photoUrls
if (response.photoUrls) {
    pm.test("photoUrls je pole", function() {
        pm.expect(Array.isArray(response.photoUrls)).to.eql(true);
    });
}

// tags
if (response.tags) {
    pm.test("tags je pole", function() {
        pm.expect(Array.isArray(response.tags)).to.eql(true);
    });
}
