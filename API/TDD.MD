# Test Driven Development with APIS

The below are just some notes I jotted down whilst building various APIS

## Unit Testing

Allows you to test a piece of code in isolation. You will need to mock any external dependencies that the code relies on

## Controllers

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

## Services

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
