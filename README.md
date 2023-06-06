# UserAPI

UserAPI is a RESTful API built with Django, Django Rest Framework, and Postgres that allows users to create and manage their profiles using token authentication.

## Features

- User authentication using token authentication
- Create user profiles using email address as primary key
- Manage user profiles by changing name and password
- Store user data securely in Postgres database
- Containerized using Docker for easy deployment
- API documentation using Swagger UI

## Requirements

- Docker
- Docker Compose

## Installation and Usage

1. Clone the repository:

```
git clone https://github.com/MastersMasterM/recipe-app-api.git
```

2. Build and run the Docker containers:

```
docker-compose up --build
```

3. Access the API documentation at http://localhost:8000/api/docs/

4. Use Swagger UI to explore and test the API endpoints.

## API Endpoints

```
openapi: 3.0.3
info:
  title: ''
  version: 0.0.0
paths:
  /api/schema/:
    get:
      operationId: schema_retrieve
      description: |-
        OpenApi3 schema for this API. Format can be selected via content negotiation.

        - YAML: application/vnd.oai.openapi
        - JSON: application/vnd.oai.openapi+json
      parameters:
      - in: query
        name: format
        schema:
          type: string
          enum:
          - json
          - yaml
      - in: query
        name: lang
        schema:
          type: string
          enum:
          - af
          - ar
          - ar-dz
          - ast
          - az
          - be
          - bg
          - bn
          - br
          - bs
          - ca
          - cs
          - cy
          - da
          - de
          - dsb
          - el
          - en
          - en-au
          - en-gb
          - eo
          - es
          - es-ar
          - es-co
          - es-mx
          - es-ni
          - es-ve
          - et
          - eu
          - fa
          - fi
          - fr
          - fy
          - ga
          - gd
          - gl
          - he
          - hi
          - hr
          - hsb
          - hu
          - hy
          - ia
          - id
          - ig
          - io
          - is
          - it
          - ja
          - ka
          - kab
          - kk
          - km
          - kn
          - ko
          - ky
          - lb
          - lt
          - lv
          - mk
          - ml
          - mn
          - mr
          - my
          - nb
          - ne
          - nl
          - nn
          - os
          - pa
          - pl
          - pt
          - pt-br
          - ro
          - ru
          - sk
          - sl
          - sq
          - sr
          - sr-latn
          - sv
          - sw
          - ta
          - te
          - tg
          - th
          - tk
          - tr
          - tt
          - udm
          - uk
          - ur
          - uz
          - vi
          - zh-hans
          - zh-hant
      tags:
      - schema
      security:
      - cookieAuth: []
      - basicAuth: []
      - {}
      responses:
        '200':
          content:
            application/vnd.oai.openapi:
              schema:
                type: object
                additionalProperties: {}
            application/yaml:
              schema:
                type: object
                additionalProperties: {}
            application/vnd.oai.openapi+json:
              schema:
                type: object
                additionalProperties: {}
            application/json:
              schema:
                type: object
                additionalProperties: {}
          description: ''
  /api/user/create/:
    post:
      operationId: user_create_create
      description: Create a new user in the system
      tags:
      - user
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/User'
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/User'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/User'
        required: true
      security:
      - cookieAuth: []
      - basicAuth: []
      - {}
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
          description: ''
  /api/user/me/:
    get:
      operationId: user_me_retrieve
      description: Manage the authenticated user
      tags:
      - user
      security:
      - tokenAuth: []
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
          description: ''
    put:
      operationId: user_me_update
      description: Manage the authenticated user
      tags:
      - user
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/User'
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/User'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/User'
        required: true
      security:
      - tokenAuth: []
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
          description: ''
    patch:
      operationId: user_me_partial_update
      description: Manage the authenticated user
      tags:
      - user
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/PatchedUser'
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/PatchedUser'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/PatchedUser'
      security:
      - tokenAuth: []
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
          description: ''
  /api/user/token/:
    post:
      operationId: user_token_create
      description: Create a new auth token for user.
      tags:
      - user
      requestBody:
        content:
          application/x-www-form-urlencoded:
            schema:
              $ref: '#/components/schemas/AuthToken'
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/AuthToken'
          application/json:
            schema:
              $ref: '#/components/schemas/AuthToken'
        required: true
      security:
      - cookieAuth: []
      - basicAuth: []
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/AuthToken'
          description: ''
components:
  schemas:
    AuthToken:
      type: object
      description: Serializer for the user auth token.
      properties:
        email:
          type: string
          format: email
        password:
          type: string
      required:
      - email
      - password
    PatchedUser:
      type: object
      description: Serializer for the user object.
      properties:
        email:
          type: string
          format: email
          maxLength: 225
        password:
          type: string
          writeOnly: true
          maxLength: 128
          minLength: 5
        name:
          type: string
          maxLength: 255
    User:
      type: object
      description: Serializer for the user object.
      properties:
        email:
          type: string
          format: email
          maxLength: 225
        password:
          type: string
          writeOnly: true
          maxLength: 128
          minLength: 5
        name:
          type: string
          maxLength: 255
      required:
      - email
      - name
      - password
  securitySchemes:
    basicAuth:
      type: http
      scheme: basic
    cookieAuth:
      type: apiKey
      in: cookie
      name: Session
    tokenAuth:
      type: apiKey
      in: header
      name: Authorization
      description: Token-based authentication with required prefix "Token"
```

## Authentication

To authenticate with the API, include an `Authorization` header in your requests with a token in the format `Token <token>` where `<token>` is the user's authentication token.

## Testing

To run the automated tests, use the following command:

```
docker-compose run app sh -c "python manage.py test && flake8"
```

## Contributing

Contributions to UserAPI are welcome! To contribute, please fork the repository and submit a pull request.

## License

UserAPI is released under the MIT License. See [LICENSE](LICENSE) for details.