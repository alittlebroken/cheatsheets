# Setting up your project for testing
The below set of steps allow you to setup a new or existing ReactJS project to use vitest for testing

Test files should live in the same location as the Component being tested and have either spec or test 
in the name.

```bash
Button.jsx
Button.css
Button.test.jsx
```

## Install packages

```bash
npm i --save-dev @testing-library/jest-dom @testing-library/react @testing-library/user-event
npm i --save-dev vitest msw jsdom @types/react @types/react-dom
```

## Configuration

### MSW

- Create a folder called mocks in the project root
- Inside here create a new file called handlers.js with the following initial code

 ```javascript
    import { http, HttpResponse } from 'msw';

    export const handlers = [
        // Add handlers here
    ]
 ```

### MSW Route Handlers

- Open up the mocks/handler.js file created earlier
- For a GET Http request you can do something similar to the following
```javascript
    export const handlers = [
       http.get('https://api.example.com/users', ({ request }) => {
         return HttpResponse.json({
           id: 1,
           username: 'tuser1',
           displayName: 'Test User # 1',
           email: 'user1@test.com'
         });
       }),
    ]
```

- To specify params passed in do the following
```javascript
 export const handlers = [
        http.get('https://api.example.com/users/:id', async ({ params, request }) => {
           const { id } = params;

           if(id === "abc-123"){
              return new HttpResponse(
                { errorMessage: 'User not found' },
                { status: 404 }
              );
           };
 
        }),
    ]
```

- To specify query params do the following
```javascript
 export const handlers = [
        http.get('https://api.example.com/users', async ({ request }) => {
           const url = new URL(request.url);
           const userId = urel.searchParams.get('id');           

           if(!userId) return new HttpResponse(null, { status: 404 });

           return httpResponse.json({
             id: userId
           });
 
        }),
    ]
```

- To Specify cookies do the following
```javascript
 export const handlers = [
        http.get('https://api.example.com/users', async ({ cookies }) => {

           if(!cookies.authToken){
             return new HttpResponse(null, { status: 403 });
           };

           return HttpResponse.json({ name: 'Jason' });

        }),
    ]
```

- For a POST/PUT request it is something similar
```javascript
    export const handlers = [
       http.get(...),
       // For PUT requests just use put instead of post after http
       http.post('https://api.example.com/users', async ({ request }) => {

          // Clone the request body for PUT and POST requests
          const reqBody = await request.clone().json();

          if(reqBody.id === "user1"){
            return HttpResonse.json({ id: "user1" });
          };

       }),
    ]
```

### VITEST

- In the project root create the file vitest.setup.ts and add the following initial code

```javascript
    import '@testing-library/jest-dom';
    import { afterEach } from 'vitest';
    import { cleanup } from '@testing-library/react';

    afterEach(() => {
        cleanup()
    });
```

- Modify the file vite.config.js file in the project root with the following code
```javascript
    import react from '@vitejs/plugin-react';
    import { defineConfig } from 'vitest/config';
    
    // https://vite.dev/config/
    export default defineConfig({
      plugins: [react()],
      test: {
        environment: 'jsdom',
        globals: true,
        setupFiles: './vitest.setup.js'
      }
    })
```

- In the root of the project folder, modify the scripts section of the package.json file to include the following.
```javascript
    /**  ... Below is the other script settings if you have any */
    "scripts": {
        ...,
        "test": "vitest run",
        "test:watch": "vitest"
    }
```
 
### Test files

- Each test file should end in either spec.jsx or test.jsx I.E Button.test.jsx
- The format of the file should be as follows

```javascript
import { render, screen, fireEvent } from '@testing-library/react';
import { describe, it, expect, vi} from 'vitest';
import { userEvent } from '@testing-library/user-event';

/** Import component here
 * I.E. import Button from './Button.jsx;
 */

describe('Component', () => {

    beforeEach(() => {});
    afterEach(() => {});

    // Sample test case
    it('should render the component', async () => {

        // Setup any props needed for the component and render it
        const propTitle = 'Click Me';
        render(<Button title={title} />);

        // Check that the Button appears in the dom as you'd expect
        expect(screen.getByRole('button', { name: /Click Me/i })).ToBeInTheDocument();

    });

};
```
