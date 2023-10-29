# Context API

### Prop Drilling:-
**Prop drilling**, also known as **component tree traversal**, is a term used in React to describe the process of passing data from a parent component to a deeply nested child component by threading the data through each intermediate component in between. This can lead to code that is harder to maintain, especially as your component tree becomes larger and more complex.

- Let me explain prop drilling in detail with an example:

    Suppose you're building a component hierarchy for a simple e-commerce website. You have three levels of components: `App` (the root component), `ProductList`, and `Product`.

    ```jsx
    // App.js
    import React, { Component } from 'react';
    import ProductList from './ProductList';
    
    class App extends Component {
      constructor() {
        super();
        this.state = {
          products: [
            { id: 1, name: 'Product A', price: 10 },
            { id: 2, name: 'Product B', price: 20 },
            { id: 3, name: 'Product C', price: 30 },
          ],
        };
      }
    
      render() {
        return (
          <div>
            <h1>My E-Commerce Store</h1>
            <ProductList products={this.state.products} />
          </div>
        );
      }
    }
    
    export default App;
    
    ```

    ```jsx
    // ProductList.js
    import React from 'react';
    import Product from './Product';
    
    const ProductList = (props) => {
      return (
        <div>
          <h2>Product List</h2>
          <Product products={props.products} />
        </div>
      );
    };
    
    export default ProductList;
    
    ```
    
    ```jsx
    // Product.js
    import React from 'react';
    
    const Product = (props) => {
      return (
        <div>
          <h3>Product Details</h3>
          <ul>
            {props.products.map((product) => (
              <li key={product.id}>
                {product.name} - ${product.price}
              </li>
            ))}
          </ul>
        </div>
      );
    };
    
    export default Product;
    
    ```

    In this example, we have an `App` component at the root, which stores a list of products in its state. The `ProductList` component is a child of  and receives the `products` array as a prop. Then, the `Product` and also receives  as a prop.

    Here's what happens:

    - `App` provides the `products` array as a prop to `ProductList`.
    - `ProductList` accepts the `products` prop and passes it down to `Product`.
    - `Product` finally uses the `products` prop to display the product details.

    This is the essence of prop drilling – the data passes through multiple intermediate components in a hierarchy to reach its final destination.

    Imagine if you wanted to add a new feature that requires access to the `products` array in a deeply nested component. You'd need to pass the data through all the intermediate components, which can become cumbersome and make your code less maintainable.
    To avoid prop drilling and make your code more efficient and readable, you can consider using React's Context API or state management libraries like Redux, which allow you to manage global state and share data across your entire application without explicitly passing it through every component.

[Doc UseContext](https://react.dev/reference/react/useContext)

and the same issue arises in other places also to solve that issue we have **Redux** (Redux solve the issue of state management ). There are many different versions of Redux are available e.g. Redux-Toolkit (RTK) this is the easier version of redux and for React there is a React-Redux.

## Context API

The **Context API** in React is a powerful feature that allows you to manage and share state data within a React application without the need to pass props down through multiple levels of components. It's especially useful for managing global state, where data needs to be accessible across different parts of your application.

Here's how the Context API works:

1. **Creating a Context:** You start by creating a context using the `React.createContext()` method. This context acts as a container for the data you want to share.
2. **Providing Data:** Next, you wrap the part of your component tree where you want to provide data with a `<Context.Provider>`. This provider component takes a `value` prop, where you place the data you want to share.
3. **Consuming Data:** In any component within the provider's subtree, you can access the shared data by using the `useContext` hook or by using the `<Context.Consumer>` component.
4. **Updating Data:** You can also define methods or functions to update the shared data within the provider component. These functions can be passed down through the context and used by components that need to modify the shared state.

The Context API offers several benefits:

1. **Avoid Prop Drilling:** It eliminates the need to pass props down through multiple levels of components, making your code cleaner and more maintainable.
2. **Global State Management:** It's an excellent choice for managing global state, such as user authentication, themes, or shopping carts.
3. **Easier Testing:** Context makes it easier to isolate and test individual components because you can replace the real context with a mock during testing.

**Common Use Cases:**

- **Theme Switching:** Allowing users to toggle between dark and light themes.
- **Authentication:** Managing user login state and user information.
- **Internationalization (i18n):** Storing and updating the user's preferred language.
- **Shopping Cart:** Keeping track of items in the cart across different pages.

To get started with the Context API, you can follow these steps:

1. Create a folder called `context` in the `src` folder of your React project.
2. Inside the `context` folder, create a file (e.g., `UserContext.js`). In this file, you can create a context using `React.createContext()` and export it.
3. Create a provider component (e.g., `UserContextProvider.jsx`) inside the `context` folder. This component will wrap the part of your component tree where you want to provide the data. It uses the `React.useState()` hook to create a state and the `UserContext.Provider` component to provide the data.
4. In your main component (e.g., `App.jsx`), import the `UserContextProvider` and wrap your component tree with it. This will give access to the shared data to all components within the provider's subtree.

Any component inside the `<UserContextProvider>` can access the shared data by using the `useContext` hook or the `<Context.Consumer>` component.
