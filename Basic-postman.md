For an SQA interview, you'll be expected to know more than just sending a request. The core of API testing in Postman is **automated validation using JavaScript** in the "Tests" tab. Here are the essential tests you should know, complete with code you can practically use.

### 1. The Foundation: Status Code Checks
This is the most basic and critical test. It verifies the API request was processed successfully and the server responded as expected.

*   **Check for a single, successful status code:**
    ```javascript
    pm.test("Status code is 200", function () {
        pm.response.to.have.status(200);
    });
    ```
    

*   **Check if the status code is within a success range (e.g., 2xx):**
    This is great for making your tests more resilient.
    ```javascript
    pm.test("Status code is 2xx", function () {
        pm.expect(pm.response.code).to.be.within(200, 299);
    });
    ```

### 2. Performance: Response Time Checks
Validating performance is a key part of an SQA role. This test ensures the API meets its Service Level Agreement (SLA) for speed.

*   **Check that the response time is under a specific limit (e.g., 200ms):**
    ```javascript
    pm.test("Response time is less than 200ms", function () {
        pm.expect(pm.response.responseTime).to.be.below(200);
    });
    ```

### 3. Core Logic: Validating the Response Body
This is where you prove you can test the actual functionality and data integrity of the API.

*   **Check for the presence of a specific field:**
    ```javascript
    pm.test("Response has an 'id' field", function () {
        var jsonData = pm.response.json();
        pm.expect(jsonData).to.have.property('id');
    });
    ```

*   **Check for the exact value of a field:**
    This verifies the core business logic.
    ```javascript
    pm.test("Product name is correct", function () {
        var jsonData = pm.response.json();
        pm.expect(jsonData.name).to.eql("Emily's Classic Perfume");
    });
    ```

*   **Check if a response is an array and has items:**
    Useful for testing `GET /users` or `GET /products` endpoints.
    ```javascript
    pm.test("Response is an array with at least 1 item", function () {
        var jsonData = pm.response.json();
        pm.expect(jsonData).to.be.an('array');
        pm.expect(jsonData.length).to.be.at.least(1);
    });
    ```

### 4. Data Integrity: JSON Schema Validation
This is a more advanced concept that interviewers love. Instead of checking individual fields, you validate the **entire structure** of the response against a predefined schema.

*   **Validate the response against a JSON schema:**
    ```javascript
    var schema = {
        "type": "object",
        "properties": {
            "id": { "type": "integer" },
            "name": { "type": "string" },
            "price": { "type": "number" }
        },
        "required": ["id", "name", "price"]
    };

    pm.test("Response matches the JSON schema", function () {
        pm.response.to.have.jsonSchema(schema);
    });
    ```
    This ensures that future API changes don't break the expected data format.

### 5. Workflow & State: Setting and Using Variables
This demonstrates you can create **dynamic test workflows** where one request depends on data from a previous one.

*   **Extract a value from the response and save it as an environment variable:**
    ```javascript
    var jsonData = pm.response.json();
    pm.environment.set("product_id", jsonData.products[0].id);
    ```
    You can then use `{{product_id}}` in the URL or body of subsequent requests.

### 6. The "Gotcha": Checking for Error States
Negative testing is just as important as positive testing. You must prove the API handles errors gracefully.

*   **Check for a `404 Not Found` status:**
    ```javascript
    pm.test("Status code is 404 for a non-existent resource", function () {
        pm.response.to.have.status(404);
    });
    ```

*   **Check for an error message in the response body:**
    ```javascript
    pm.test("Response contains an 'error' message", function () {
        var jsonData = pm.response.json();
        pm.expect(jsonData).to.have.property('message');
        pm.expect(jsonData.message).to.include("not found");
    });
    ```

---

### 🚀 Key Concepts to Mention in the Interview

To show you're not just copying code, be prepared to explain these:

1.  **`pm` Object:** It's the global Postman object that gives you access to everything: `pm.response`, `pm.environment`, `pm.test`, etc.
2.  **`pm.expect`:** This is a [Chai.js](https://www.chaijs.com/) style assertion library (BDD) built into Postman.
3.  **Chaining Requests:** Mention how you use variables to pass data (like an auth token or a newly created user ID) from one request to the next.
4.  **Automation:** Explain how you can run these tests automatically using the **Collection Runner** (for a single run) or **Newman** (Postman's command-line tool for CI/CD integration).
5.  **Data-Driven Testing:** Show you know how to run the same collection with different data sets from a CSV or JSON file.