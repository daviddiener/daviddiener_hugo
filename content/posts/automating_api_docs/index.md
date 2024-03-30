---
title: Automating API Documentation for RESTful Backends
date: 2024-03-30
tags:
  - webdev
  - gamedev
cover:
    image: "cover.png"
---

To keep up with [Odyssey's](https://odyssey.daviddiener.de/) evolving landscape I looked up efficient ways of automating API documentation. This blog post delves into various strategies for automating API documentation for RESTful backends. This post explores three solutions for automating API documentation for RESTful backends, all adhering to the **single source of truth principle** as well as using the OpenAPI specification as a middleware for the documentation of the endpoints. We'll delve into the specifics of each solution, its advantages and disadvantages, and how it integrates with your development workflow.

## OpenApi Specification

OpenAPI Specification (OAS) offers a machine-readable format (YAML or JSON) that automation tools can easily parse. This allows them to extract crucial API details and generate comprehensive documentation, reducing manual work.

> The OpenAPI Specification defines a standard, programming language-agnostic interface description for HTTP APIs, which allows both humans and computers to discover and understand the capabilities of a service without requiring access to source code, additional documentation, or inspection of network traffic. When properly defined via OpenAPI, a consumer can understand and interact with the remote service with a minimal amount of implementation logic. Similar to what interface descriptions have done for lower-level programming, the OpenAPI Specification removes guesswork in calling a service [^1]. 

Furthermore, OAS enforces consistency in describing API endpoints. This consistency simplifies automated documentation and ensures it remains organized and user-friendly. Maintaining the OAS document as Odyssey's backend evolves is easier than updating manual written documentation. Additionally, a rich ecosystem of OAS-compatible tools exists. Tools like swagger-jsdoc can automatically generate an OAS document from Javascript comments, while postman-to-openapi converts Postman collections (often containing valuable examples and tests) into OAS specifications. These tools streamline the automation process.

## Exploring Effective Automation Techniques

Here, we delve into three leading techniques for automating API documentation for RESTful backends, catering to different project requirements and preferences:

### Using JsDoc Comments as the Source of Truth

This method utilizes existing JsDoc comments within your codebase to generate OpenAPI specifications (OAS). Libraries like [swagger-jsdoc](https://github.com/Surnet/swagger-jsdoc) parse these comments to create a machine-readable representation of your API. We can then use [swagger-tools](https://swagger.io/tools/) to render the API documentation.

#### Benefits
In case JsDoc comments are already present for code documentation, the need for additional documentation effort is reduced. Moreover, tools like swagger-ui-express enable embedding an interactive Swagger UI directly into your Express server, providing a user-friendly interface for exploring your API.

#### Drawbacks
However, writing JsDoc comments for every route handler can become tedious, especially for complex APIs with numerous endpoints. Additionally, reliance on existing comments might limit the level of detail or structure you can achieve in the generated documentation. A JsDoc for a route could look like this:

```
/**
 * @openapi
 * /:
 *   get:
 *     description: Welcome to swagger-jsdoc!
 *     responses:
 *       200:
 *         description: Returns a mysterious string.
 */
app.get('/', (req, res) => {
  res.send('Hello World!');
});
```

### Automatic Specification Generation with TSOA

[TSOA](https://github.com/lukeautry/tsoa) is a tool designed specifically for generating OAS from TypeScript code. It uses route definitions, controller functions, and data types to construct the OpenAPI specification.

#### Benefits
TSOA streamlines the documentation process by automatically generating the OAS from your TypeScript codebase, reducing manual effort. Changes in your code are reflected in the documentation, ensuring consistency and improved maintainability.

#### Drawbacks
However, TSOA necessitates adding annotations to routes, controllers, and enforces specific structures for entity models, which might introduce extra work or conflicts with existing coding styles. Additionally, if your project does not use TypeScript, TSOA might not be a suitable choice due to its TypeScript dependency.

### Postman + P20 + Redoc: A Testing-Driven Approach

This strategy revolves around using Postman, for creating API requests, tests, and saving response examples. The [postman-to-openapi (P20)](https://joolfe.github.io/postman-to-openapi/) library bridges the gap by converting Postman collections into OpenAPI specifications. Finally, [Redoc](https://github.com/Redocly/redoc), an open source tool for generating documentation from OpenAPI, transforms the OAS into user-friendly and interactive API documentation.


1. **Exporting Your Postman Collection**:  First things first, head over to the "Collection" menu within Postman. Select the "Export" option, and ensure you choose "Collection v2.1" for the export format. This creates a file containing all your API requests and tests.

2. **Installing P20**:  Next, we'll install P20 (postman-to-openapi).  Open your terminal and use the following command to install P20 using npm: 

``` 
npm i postman-to-openapi -g
```

3. **Converting Postman to OAS**: Here's a breakdown of the command used to transform your Postman collection into an OpenAPI Specification (OAS) file:

``` 
p2o .\Odyssey.postman_collection.json -f .\static\odyssey_api\odyssey_api.yml -o options.json 
```

- p2o: This invokes the P20 tool for the conversion.
- .<path to your Postman collection>.postman_collection.json: Replace this with the actual location of your exported Postman collection file.
- -f <output file path>: Specify the desired location and filename for the generated OAS file.
- -o options.json: While not strictly required, this option allows you to provide additional configuration for the conversion process (refer to the P20 documentation for details).


4. **Setup Redoc**: Finally, we'll integrate Redoc into your web application to present the generated OAS in a user-friendly and interactive format. Here's a code snippet demonstrating how to embed Redoc in your HTML:

```
<html>
<body>
    <redoc spec-url="./odyssey_api.yml"></redoc>
    <script src="https://cdn.redoc.ly/redoc/latest/bundles/redoc.standalone.js"> </script>
</body>
</html>
```

#### Benefits:
This approach merges API testing with documentation creation, fostering a more comprehensive development workflow. Moreover, Postman collections can store response examples, which are invaluable for understanding the format and structure of data returned by the API. The postman-to-openapi (P20) library bridges the gap by converting these collections into OpenAPI specifications. This ensures that the generated documentation includes not only endpoint details but also illustrative examples of API responses, which you can save in the Postman collection. For me, this is the most important difference to the other two approaches. By feeding different examples of how the API should respond into the documentation, the users will get a much better understanding on how to actually use your API.

#### Drawbacks
This approach of course requires you to use Postman for creating API requests and tests. If your workflow doesn't involve Postman, this method might not be the most suitable choice.

## Choosing the Right Approach

The optimal approach for your project depends on various factors. Consider the following:

- Project size and complexity: Larger or more intricate APIs might benefit from TSOA's automation capabilities.
- Existing codebase: JsDoc integration leverages existing comments, reducing additional documentation overhead.
- Testing strategy: If testing is a priority, Postman's testing features combined with P20 and Redoc offers the most complete solution. I choose this strategy for Odyssey, as I have a need for automated testing anyways.

By adopting an automated approach, you can streamline API documentation, reduce maintenance efforts, and ensure your developers and consumers have access to clear and up-to-date API specifications.

## References

[^1]: Darrel Miller, Jeremy Whitlock, Marsh Gardiner, Ron Ratovsky - OpenAPI Specification v3.1.0 https://spec.openapis.org/oas/v3.1.0
