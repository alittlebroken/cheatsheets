# Test Driven Development with APIS

The below are just some notes I jotted down whilst building various APIS

## NPM Packages used for TDD

- vitest
- supertest

## Integration Testing

Allows you to test how well the routes work with Express and the controllers. You can mock either the controller, service or model to ensure that your routes uses the correct ones

```javascript

  import { request } from 'supertest';
  import model from '../models/model.js';
  import app from '../app.js';

  describe('Route', () => {
    it('....', async () => {

        // Data for the controller to return
        const returnData = [1, 2, 3, 4];

        // Create the spy for the controller
        vi.spyOn(model, 'findAll').mockReturnValue(returnData);

        // Execute the request
        const res = await requesat(app).get('/data');

        // Check the response is as we expect
        expect(res.status).toBe(200);
        expect(res.headers['content-type']).toMatch(/json/);

        expect(res.body).tobeDefined();
        expect(res.body).toBeTypeOf(Array);
        expect(res.body).toHaveLength(4);
        expect(res.body).toEqual(returnData);

    });
  });

```



## Unit Testing

Allows you to test a piece of code in isolation. You will need to mock any external dependencies that the code relies on

### Controllers

Unit testing a controller means you need to mock the req and res objects as well as any services it uses.

```javascript

  import service from './service.js';

  describe('Controller', () => {
    it('....', async () => {

        // Mock the Service the controller uses
        const dataList = [1, 2, 3, 4];
        vi.spyOn(service, 'findAll').mockReturnValue(dataList);

        // Mock the request and response objects
        const req = {};
        const res = { status: vi.fn().mockReturnThis(), json: vi.fn() };

        // Execute the controller
        await controller.findAll(req, res);

        // Check the controller returns what is expected of it
        expect(res.status).toHaveBeenCalledWith(200);
        expect(res.json).toHaveBeenCalledWith(dataList);

    });
  });
```

### Services

To unit test a service you would need to Mock either the underlying model it uses or database if it connects to them directly.

```javascript
  import dataModel from '../models/data/data.model.js';

  describe('Service', () => {
    it('....', async () => {

        // Mock the DB methods the service uses
        const dataList = [1, 2, 3, 4];
        vi.spyOn(dataModel, 'findAll').mockReturnValue(dataList);

        // Execute the service
        const data = await findAll();

        // Check the controller returns what is expected of it
        expect(data).toBeDefined();
        expect(data).toBeTypeOf(Array);
        expect(data).toHaveLength(4);

    });
  });
  
```
