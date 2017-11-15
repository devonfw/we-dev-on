# CobiGen: Now with Swagger / OpenApi 3.0 support

CobiGen is a tool that is constantly evolving. Until now, CobiGen was based on a "code first" generation, that means that every generation needed a previous Java or XML file as input to the generation to start.
On this last version, a new plug-in for Swagger 3.0 is added, enabling the "contract first" generation using a Swagger file that follows the OpenApi 3.0 standard.
 
## What is Swagger?

Swagger is a framework for describing your API using a common language that everyone can understand. That way you can define entities, the relationships that these entities must have, services with defined http requests types, etc... .

In conclusion:

- It's understandable for developers and non-developers. Product managers, partners, and even potential clients can have input into the design of your API, because they can see it clearly mapped out in this friendly UI.
- It's human readable and machine readable. This means that not only can this be shared with your team internally, but the same documentation can be used to automate API-dependent processes.
- It's easily adjustable. This makes it great for testing and debugging API problems.

Swagger Example - [Pet Store](http://petstore.swagger.io/#/)

## How does it works?

Just put your Swagger definition file into the core folder of your OASP4J project and use CobiGen from it. You can generate all the layers of the OASP4J back-end (dataacces, logic and service) in just one single generation, including the defined custom operations. Also, at the same generation, you can generate an Angular4+ front-end.

![alt text](img/cbgn-swgr.png "CobiGen Wizard")

## Writing the Swagger File

To be compatible with CobiGen and OASP4J, the Swagger file must follow some specific configurations. This configurations also allows to avoid redundant definitions as SearchCriterias and PaginatedList objects used at the services definitions.
Just adding the _tags_ property at the end of the service definitions with the items _SearchCriteria_ and/or _paginated_ put into CobiGen knowledge that an standard OASP4J SearchCriteria and/or PaginateListTo object must be generated. That way, the Swagger file will be easier to write and even more understandable.

```src
  /sampledatamanagement/v1/sampledata/customSearch/:
    post:
      summary: 'Delegates to {@link Sampledatamanagement#findSampleDataEtos}.'
      description: Return SampleData that fits the filter.
      operationId: findCustomSampleDataEtos
      responses:
        '200':
          description: List of SampleData's.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/SampleData'
      requestBody:
        $ref: '#/components/requestBodies/SampleData'
      tags:
        - searchCriteria
        - paginated
```

Same criteria is followed for the relationships. Adding the extended properties to the schema definitions allows the definitions of unidirectional and bidirectional relationships:

- x-onetoone
- x-manytoone
- x-onetomany
- x-manytomany

```src
SampleData:
      x-component: sampledatamanagement
      type: object
      properties:
        name:
          type: string
          maxLength: 100
          minLength: 0
          description: Name string.
        surname:
          type: string
          description: Surname string.
        age:
          type: integer
          format: int32
          description: Age integer.
        mail:
          type: string
          description: Mail string.
      x-onetomany: MoreData
MoreData:
      x-component: moredatamanagement
      type: object
      properties:
        example:
          type: string
      x-manytoone: SampleData
```

Is important to underline that, the Swagger file must follow the OpenAPI 3.0 specifications.
