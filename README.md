# qa-automatization

Aquí tienes un resumen detallado de todo lo que necesitas saber sobre automatización de pruebas según el path de formación que estás siguiendo, organizado por módulos:

---

## 🧭 **Módulo 0: XPath, Selectores HTML y CSS Selector**

### Objetivo:

Aprender a interactuar con la GUI de una web a través de selectores.

### Conceptos clave:

* **XPath**: Lenguaje para navegar por elementos y atributos en un documento XML/HTML. Ej:

  * `//input[@name='username']`
  * `//div[@class='menu']//a[text()='Home']`

* **CSS Selectors**: Selectores más rápidos y legibles en muchos casos.

  * `input[name="username"]`
  * `.menu a`
  * `#main-content > div:nth-child(2)`

### Buenas prácticas:

* Preferir **CSS Selectors** sobre XPath por rendimiento.
* Mantener selectores robustos y estables (evitar índices si es posible).

---

## 🐍 **Módulo 1: Python Behave + Selenium**

### Objetivo:

Automatizar pruebas funcionales con Selenium WebDriver y BDD usando Behave.

### Herramientas:

* **Selenium WebDriver**: Controla navegadores (Chrome, Firefox, etc.).
* **Behave**: Framework BDD con sintaxis Gherkin (`Given`, `When`, `Then`).

### Elementos clave:

* **Features**: `.feature` con escenarios Gherkin.
* **Steps**: Funciones Python que implementan cada paso Gherkin.
* **`environment.py`**: Setup/teardown del entorno de test.
* **Page Object Model (POM)**: Patrones que encapsulan las páginas web como clases.

### Ejemplo básico:

```gherkin
Feature: Login
  Scenario: User logs in with valid credentials
    Given I am on the login page
    When I enter valid credentials
    Then I should be redirected to the dashboard
```

```python
@given('I am on the login page')
def step_impl(context):
    context.browser.get('https://example.com/login')

@when('I enter valid credentials')
def step_impl(context):
    context.browser.find_element(...).send_keys('user')
    context.browser.find_element(...).send_keys('pass')

@then('I should be redirected to the dashboard')
def step_impl(context):
    assert 'dashboard' in context.browser.current_url
```

### Buenas prácticas:

* Separar lógica de test y de UI.
* Agrupar pasos comunes en módulos reusables.

---

## 🤖 **Módulo 2: Robot Framework**

### Objetivo:

Automatización de pruebas con sintaxis legible y de alto nivel.

### Características:

* Framework extensible y orientado a palabras clave (**keyword-driven**).
* Ideal para testers sin mucho conocimiento de programación.

### Archivos:

* `.robot`: Archivos de pruebas.
* `resource.robot`: Recurso reutilizable con funciones.
* `variables.robot`: Definición de variables.

### Librerías:

* `SeleniumLibrary` (UI)
* `Browser` (basada en Playwright)

### Ejemplo:

```robot
*** Settings ***
Library    SeleniumLibrary

*** Variables ***
${URL}    https://example.com

*** Test Cases ***
Login Test
    Open Browser    ${URL}    chrome
    Input Text    id=username    user
    Input Text    id=password    pass
    Click Button    id=submit
    Page Should Contain Element    id=dashboard
    Close Browser
```

---

## 🎭 **Módulo 3: Playwright (Python)**

### Objetivo:

Automatización moderna, rápida y paralelizable de interfaces web.

### Características:

* Soporte para múltiples navegadores (Chromium, Firefox, WebKit).
* Fixtures, screenshots, vídeos.
* Paralelización de pruebas nativamente.

### Ventajas vs Selenium:

* Más rápido, moderno y confiable.
* Mejor manejo de eventos asíncronos.

### Ejemplo básico:

```python
from playwright.sync_api import sync_playwright

def test_example():
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=True)
        page = browser.new_page()
        page.goto("https://example.com")
        page.fill('input[name="username"]', 'user')
        page.fill('input[name="password"]', 'pass')
        page.click('button[type="submit"]')
        assert page.url.endswith("/dashboard")
        browser.close()
```

---

## 🔌 **Módulo 4: API Automation**

### Objetivo:

Automatizar pruebas de APIs RESTful.

### Opciones:

#### A. Con Behave:

* Usar librerías como `requests`.
* Verificar status codes, headers, JSON, etc.

```python
import requests

@given('I call the GET endpoint')
def step_impl(context):
    context.response = requests.get('https://api.example.com/users')

@then('I should receive status 200')
def step_impl(context):
    assert context.response.status_code == 200
```

#### B. Con Robot Framework:

* Usar `RequestsLibrary`.

```robot
*** Settings ***
Library    RequestsLibrary

*** Test Cases ***
GET User
    Create Session    api    https://api.example.com
    ${response}=    GET    api    /users
    Should Be Equal As Numbers    ${response.status_code}    200
```

---

## 🧩 **Módulo 5: Complementos (Integraciones y Reporting)**

### Jenkins:

* CI/CD: ejecutar tests automáticamente en cada commit.
* Puedes integrar con Behave, Robot y Playwright.
* Plugins: JUnit, Allure, HTML Publisher.

### Docker:

* Entornos consistentes.
* Crear contenedores con todo preinstalado: navegador, Python, dependencias.

### Reporting:

#### Allure:

* Reportes visuales y detallados para Behave y Robot Framework.
* Integración mediante decoradores o plugins.

```bash
behave -f allure_behave.formatter:AllureFormatter -o allure-results
allure serve allure-results
```

#### Playwright HTML Report:

```bash
pytest --html=report.html
```

---

## 📌 Recomendaciones generales

1. **Estructura de proyecto clara**:

   ```
   tests/
     features/
     steps/
     pages/
     environment.py
   ```

2. **Versionado en Git** + integración continua con Jenkins.

3. **Separación de responsabilidades**:

   * Lógica de UI en POM.
   * Lógica de test en steps.

4. **Buena cobertura**:

   * UI
   * API
   * Casos edge y de error

5. **Gestión de datos**:

   * Mock de datos o uso de entornos dedicados.

6. **Control de flujos asíncronos y esperas**:

   * Selenium y Playwright permiten manejar esperas explícitas e implícitas.

---

¿Quieres que prepare algún proyecto de ejemplo completo con Behave, Robot o Playwright para que puedas practicar o probar tú mismo?
