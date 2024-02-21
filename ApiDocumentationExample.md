### Documenting Endpoints in an API.
In this app there's one model (`Animal`) and 5 endpoints related to that model. Your goal in creating the documentation for your API is to instruct users as clearly as possible how access and use these endpoints. This should include the URL for each endpoint, it's required and optional parameters and examples of responses the user should expect.

### Base URL: http://localhost:5000

### GET /api/Animals
<table>
    <thead>
      <tr>
        <th>HTTP Verb</th>
        <th>URL</th>
        <th>Expected Behavior</th>
        <th>Response Status</th>
      </tr>
    </thead>
      <tr>
        <td>GET</td>
        <td>/api/Animals</td>
        <td>Returns an array containing all Animal objects in the database.</td>
        <td>200: Ok</td>
      </tr>
</table>

Expected Response:
```json
[
  {
    "animalId": 1,
    "name": "Matilda",
    "species": "Woolly Mammoth",
    "age": 7
  },
  {
    "animalId": 2,
    "name": "Rexie",
    "species": "Dinosaur",
    "age": 10
  }
]
```

### GET /api/Animals *with optional query parameters
<table>
    <thead>
      <tr>
        <th>HTTP Verb</th>
        <th>URL</th>
        <th>Optional URL Parameters</th>
        <th>Expected Behavior</th>
        <th>Response Status</th>
      </tr>
    </thead>
      <tr>
        <td>GET</td>
        <td>/api/Animals?[PARAMETER NAME]=[PARAMETER VALUE]</td>
        <td>name (string) <br> species (string) <br> minimumAge (integer)</td>
        <td>Returns an array containing all Animal objects in the database that match the included parameters, multiple parameters may be included.</td>
        <td>200: Ok</td>
      </tr>
</table>

Example Request URL: `GET /api/Animals?species=Dinosaur&name=Rexie&minimumAge=10`

Expected Response:

```json
[
 {
    "animalId": 2,
    "name": "Rexie",
    "species": "Dinosaur",
    "age": 10
  }
]
```

### GET /api/Animals/{id}
<table>
    <thead>
      <tr>
        <th>HTTP Verb</th>
        <th>URL</th>
        <th>URL Parameter *required</th>
        <th>Expected Behavior</th>
        <th>Response Status</th>
      </tr>
    </thead>
      <tr>
        <td>GET</td>
        <td>/api/Animals/{id}</td>
        <td>id (int)</td>
        <td>Returns a JSON object representing an Animal with an "animalId" property that matches the "id" provided as a URL parameter.</td>
        <td>200: Ok</td>
      </tr>
</table>

Example Request URL: `GET /api/Animals/1`

Expected Response: 

```json
  {
    "animalId": 1,
    "name": "Matilda",
    "species": "Woolly Mammoth",
    "age": 7
  },
```

### POST /api/Animals
<table>
    <thead>
      <tr>
        <th>HTTP Verb</th>
        <th>URL</th>
        <th>Request Body *required</th>
        <th>Expected Behavior</th>
        <th>Response Status</th>
      </tr>
    </thead>
      <tr>
        <td>POST</td>
        <td>/api/Animals</td>
        <td>A JSON object containing key-value pairs for: <br> - name(string), <br> - species(string), <br> - age(int) <br> - animalId(int) may be included but regardless of the value provided, it's value will be set by the database when the record is saved.</td>
        <td>Creates a new Animal object in the database.</td>
        <td>201: Created</td>
      </tr>
</table>

Example Request Body *required:

```json
  {
    "name": "Matilda",
    "species": "Woolly Mammoth",
    "age": 7
  }
```

Expected Response:

```json
  {
    "animalId": 100,
    "name": "Matilda",
    "species": "Woolly Mammoth",
    "age": 7
  }
```

### PUT /api/Animals/{id}
<table>
    <thead>
      <tr>
        <th>HTTP Verb</th>
        <th>URL</th>
        <th>URL Parameter *required</th>
        <th>Request Body *required</th>
        <th>Expected Response</th>
        <th>Response Status</th>
      </tr>
    </thead>
      <tr>
        <td>PUT</td>
        <td>/api/Animals/{id}</td>
        <td>id (int)</td>
        <td>A JSON object containing key-value pairs for: <br> - animalId(int) <br> - name(string), <br> - species(string), <br> - age(int) <br> *Note that the "animalId" must match the "id" provided as a URL parameter.</td>
        <td>No content</td>
        <td>204: No Content</td>
      </tr>
</table>

Example Request Body *required:

```json
  {
    "animalId": 100,
    "name": "Matilda",
    "species": "Woolly Mammoth",
    "age": 8
  }
```

### DELETE /api/Animals/{id}
<table>
    <thead>
      <tr>
        <th>HTTP Verb</th>
        <th>URL</th>
        <th>URL Parameter *required</th>
        <th>Expected Behavior</th>
        <th>Response Status</th>
      </tr>
    </thead>
      <tr>
        <td>DELETE</td>
        <td>/api/Animals/{id}</td>
        <td>id (int)</td>
        <td>Deletes an Animal from the database.</td>
        <td>204: No Content</td>
      </tr>
</table>
